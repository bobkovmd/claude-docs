---
title: "Использование Messages API"
url: "https://docs.claude.com/ru/build-with-claude/working-with-messages"
lang: "ru"
---

Сообщения/Разработка с Claude
# Использование Messages API
Практические шаблоны и примеры эффективного использования Messages APIAnthropic предлагает два способа разработки с Claude, каждый из которых подходит для разных сценариев использования:

Messages APIClaude Managed Agents**Что это**Прямой доступ к модели через подсказкиГотовая настраиваемая агентная оболочка, работающая в управляемой инфраструктуре**Лучше всего подходит для**Пользовательских агентных циклов и детального контроляДлительных задач и асинхронной работы**Подробнее**[Документация по Messages API](/docs/ru/build-with-claude/working-with-messages)[Документация по Claude Managed Agents](/docs/ru/managed-agents/overview)
Это руководство охватывает распространённые шаблоны работы с Messages API, включая базовые запросы, многоходовые диалоги, техники предзаполнения и возможности работы с изображениями. Полные спецификации API см. в [справочнике по Messages API](/docs/ru/api/messages/create).

Эта функция соответствует требованиям [Zero Data Retention (ZDR)](/docs/ru/build-with-claude/api-and-data-retention) (нулевого хранения данных). Если у вашей организации действует соглашение ZDR, данные, отправленные через эту функцию, не сохраняются после возврата ответа API.

## Базовый запрос и ответ

Параметры сэмплирования `temperature`, `top_p` и `top_k` не поддерживаются в Claude Opus 4.7 и более поздних моделях, включая Claude Opus 4.8. Установка для них значения, отличного от значения по умолчанию, возвращает ошибку 400. Исключите их из тела запроса и используйте подсказки для управления поведением модели. См. [руководство по миграции](/docs/ru/about-claude/models/migration-guide#migrating-from-claude-opus-47).

```
`message = anthropic.Anthropic().messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello, Claude"}],
)
print(message)`
```

Output
```
`{
  "id": "msg_01XFDUDYJgAACzvnptvVoYEL",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "Hello!"
    }
  ],
  "model": "claude-opus-4-8",
  "stop_reason": "end_turn",
  "stop_sequence": null,
  "usage": {
    "input_tokens": 12,
    "output_tokens": 6
  }
}`
```

В Claude Opus 4.7 и более поздних моделях ответы с отказом (`stop_reason: "refusal"`) также включают объект `stop_details`, указывающий категорию политики, которая вызвала отказ. См. [Обработка причин остановки](/docs/ru/build-with-claude/refusals-and-fallback#refusal-response) для справки по полям и примера кода обработки.

## Несколько ходов диалога

Messages API не хранит состояние, что означает, что вы всегда отправляете в API полную историю диалога. Вы можете использовать этот шаблон для постепенного построения диалога. Более ранние ходы диалога не обязательно должны действительно исходить от Claude. Вы можете использовать синтетические сообщения с ролью `assistant`.

```
`message = anthropic.Anthropic().messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Hello, Claude"},
        {"role": "assistant", "content": "Hello!"},
        {"role": "user", "content": "Can you describe LLMs to me?"},
    ],
)
print(message)`
```

Output
```
`{
  "id": "msg_018gCsTGsXkYJVqYPxTgDHBU",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "Sure, I'd be happy to provide..."
    }
  ],
  "model": "claude-opus-4-8",
  "stop_reason": "end_turn",
  "stop_sequence": null,
  "usage": {
    "input_tokens": 30,
    "output_tokens": 309
  }
}`
```

### Роль system в сообщениях

В Claude Opus 4.8 вы можете включать сообщения с `"role": "system"` после хода пользователя (с учётом [правил размещения](/docs/ru/build-with-claude/mid-conversation-system-messages#limitations)), чтобы добавить новую системную инструкцию в середине диалога. Сообщение `system` не может быть первым элементом в `messages`; используйте поле верхнего уровня `system` для инструкций, которые применяются с самого начала.

Системное сообщение в середине диалога имеет тот же приоритет, что и поле верхнего уровня `system`, но поскольку оно добавляется в конец истории сообщений, оно не делает недействительным какой-либо кэшированный префикс, который был до него. Используйте поле верхнего уровня `system` для инструкций, которые должны применяться с самого первого хода, а системное сообщение в середине диалога — для инструкций, которые становятся актуальными только позже.

См. [Системные сообщения в середине диалога](/docs/ru/build-with-claude/mid-conversation-system-messages) для полного руководства, включая информацию о том, как сочетать их с [кэшированием подсказок](/docs/ru/build-with-claude/prompt-caching).

## Вкладывание слов в уста Claude

Вы можете предварительно заполнить часть ответа Claude в последней позиции списка входных сообщений. Это можно использовать для формирования ответа Claude. В примере ниже используется `"max_tokens": 1`, чтобы получить от Claude единственный ответ на вопрос с множественным выбором.

Предзаполнение не поддерживается в Claude Fable 5, [Claude Mythos 5](https://anthropic.com/glasswing), [Claude Mythos Preview](https://anthropic.com/glasswing), Claude Opus 4.8, Claude Opus 4.7, Claude Opus 4.6 и Claude Sonnet 4.6. Запросы с использованием предзаполнения для этих моделей возвращают ошибку 400. Вместо этого используйте [структурированные выходные данные](/docs/ru/build-with-claude/structured-outputs) на моделях, которые их поддерживают, или инструкции в системной подсказке. См. [руководство по миграции](/docs/ru/about-claude/models/migration-guide) для шаблонов миграции.

```
`message = anthropic.Anthropic().messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1,
    messages=[
        {
            "role": "user",
            "content": "What is latin for Ant? (A) Apoidea, (B) Rhopalocera, (C) Formicidae",
        },
        {"role": "assistant", "content": "The answer is ("},
    ],
)
print(message)`
```

Output
```
`{
  "id": "msg_01Q8Faay6S7QPTvEUUQARt7h",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "C"
    }
  ],
  "model": "claude-sonnet-4-5",
  "stop_reason": "max_tokens",
  "stop_sequence": null,
  "usage": {
    "input_tokens": 42,
    "output_tokens": 1
  }
}`
```

## Работа с изображениями

Claude может читать как текст, так и изображения в запросах. Изображения могут быть предоставлены с использованием типов источников `base64`, `url` или `file`. Тип источника `file` ссылается на изображение, загруженное через [Files API](/docs/ru/build-with-claude/files). Поддерживаемые типы медиа: `image/jpeg`, `image/png`, `image/gif` и `image/webp`. Подробнее см. в [руководстве по работе с изображениями](/docs/ru/build-with-claude/vision).

```
`import base64
import httpx

# Вариант 1: изображение в кодировке Base64
image_url = "https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg"
image_media_type = "image/jpeg"
image_data = base64.standard_b64encode(httpx.get(image_url).content).decode("utf-8")

message = anthropic.Anthropic().messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "image",
                    "source": {
                        "type": "base64",
                        "media_type": image_media_type,
                        "data": image_data,
                    },
                },
                {"type": "text", "text": "What is in the above image?"},
            ],
        }
    ],
)
print(message)

# Вариант 2: изображение по URL-ссылке
message_from_url = anthropic.Anthropic().messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "image",
                    "source": {
                        "type": "url",
                        "url": "https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg",
                    },
                },
                {"type": "text", "text": "What is in the above image?"},
            ],
        }
    ],
)
print(message_from_url)`
```

Output
```
`{
  "id": "msg_01EcyWo6m4hyW8KHs2y2pei5",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "This image shows an ant, specifically a close-up view of an ant. The ant is shown in detail, with its distinct head, antennae, and legs clearly visible. The image is focused on capturing the intricate details and features of the ant, likely taken with a macro lens to get an extreme close-up perspective."
    }
  ],
  "model": "claude-opus-4-8",
  "stop_reason": "end_turn",
  "stop_sequence": null,
  "usage": {
    "input_tokens": 1551,
    "output_tokens": 71
  }
}`
```

## Дальнейшие шаги

[Причины остановки и резервные вариантыОбрабатывайте каждое значение `stop_reason` и решайте, что делать, когда ответ завершается.
](/docs/ru/build-with-claude/handling-stop-reasons)[Использование инструментов с ClaudeПредоставьте Claude инструменты для вызова внешних сервисов и API из Messages API.
](/docs/ru/agents-and-tools/tool-use/overview)[Инструмент управления компьютеромУправляйте окружениями настольных компьютеров с помощью Messages API.
](/docs/ru/agents-and-tools/tool-use/computer-use-tool)[Структурированные выходные данныеПолучайте гарантированный, проверенный по схеме JSON-вывод от Claude.
](/docs/ru/build-with-claude/structured-outputs)[[icon]Бюджеты задачУстановите рекомендательный бюджет токенов для всего агентного цикла с помощью `output_config.task_budget`.
](/docs/ru/build-with-claude/task-budgets)Was this page helpful?