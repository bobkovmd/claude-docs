---
title: "Пакетная обработка"
url: "https://docs.claude.com/ru/build-with-claude/batch-processing"
lang: "ru"
---

Сообщения/Возможности модели
# Пакетная обработка
Пакетная обработка — это мощный подход для эффективной обработки больших объёмов запросов. Вместо обработки запросов по одному с немедленными ответами, пакетная обработка позволяет отправлять несколько запросов вместе для асинхронной обработки. Этот паттерн особенно полезен, когда:

- Вам нужно обработать большие объёмы данных
- Немедленные ответы не требуются
- Вы хотите оптимизировать затраты
- Вы выполняете масштабные оценки или анализ

Message Batches API — это первая реализация данного паттерна от Anthropic.

Эта функция **не** подпадает под действие политики [Zero Data Retention (ZDR)](/docs/ru/build-with-claude/api-and-data-retention). Данные хранятся в соответствии со стандартной политикой хранения данных для этой функции.

# Message Batches API

Message Batches API — это мощный и экономичный способ асинхронной обработки больших объёмов запросов [Messages](/docs/ru/api/messages/create). Этот подход хорошо подходит для задач, не требующих немедленных ответов: большинство пакетов завершаются менее чем за 1 час, при этом затраты снижаются на 50%, а пропускная способность увеличивается.

В дополнение к этому руководству вы можете [изучить справочник по API напрямую](/docs/ru/api/creating-message-batches).

## Как работает Message Batches API

Когда вы отправляете запрос в Message Batches API:

- Система создаёт новый Message Batch с предоставленными запросами Messages.
- Затем пакет обрабатывается асинхронно, причём каждый запрос обрабатывается независимо.
- Вы можете опрашивать статус пакета и получать результаты, когда обработка всех запросов завершена.

Это особенно полезно для массовых операций, не требующих немедленных результатов, таких как:

- Масштабные оценки: эффективная обработка тысяч тестовых случаев.
- Модерация контента: асинхронный анализ больших объёмов пользовательского контента.
- Анализ данных: генерация аналитических выводов или сводок для больших наборов данных.
- Массовая генерация контента: создание больших объёмов текста для различных целей (например, описания продуктов, краткие изложения статей).

### Ограничения пакетов

- Message Batch ограничен либо 100 000 запросов Message, либо размером 256 МБ — в зависимости от того, что будет достигнуто первым.
- Система обрабатывает каждый пакет максимально быстро, и большинство пакетов завершаются в течение 1 часа. Вы можете получить доступ к результатам пакета, когда все сообщения завершены или по истечении 24 часов — в зависимости от того, что наступит раньше. Срок действия пакетов истекает, если обработка не завершается в течение 24 часов.
- Результаты пакета доступны для скачивания в течение 29 дней после создания. После этого вы по-прежнему можете просматривать пакет, но его результаты больше не будут доступны для скачивания.
- Пакеты привязаны к [Workspace](/settings/workspaces). Вы можете просматривать все пакеты (и их результаты), созданные в рамках Workspace, к которому принадлежит ваш ключ API.
- Ограничения скорости применяются как к HTTP-запросам Batches API, так и к количеству запросов внутри пакета, ожидающих обработки. См. [Ограничения скорости Message Batches API](/docs/ru/api/rate-limits#message-batches-api). Кроме того, обработка может замедляться в зависимости от текущей нагрузки и объёма ваших запросов. В этом случае вы можете увидеть больше запросов, срок действия которых истекает через 24 часа.
- Из-за высокой пропускной способности и параллельной обработки пакеты могут немного превышать настроенный [лимит расходов](/settings/limits) вашего Workspace.
- Каждый запрос в пакете должен иметь значение `max_tokens` не менее `1`. `max_tokens: 0` ([предварительный прогрев кэша](/docs/ru/build-with-claude/prompt-caching#pre-warming-the-cache)) не поддерживается внутри пакета, поскольку эфемерная запись кэша, созданная во время пакетной обработки, скорее всего истечёт до выполнения последующего запроса.

### Поддерживаемые модели

Все [активные модели](/docs/ru/about-claude/models/overview) поддерживают Message Batches API.

### Что можно включать в пакет

Практически любой запрос, который вы можете отправить в Messages API, может быть включён в пакет. Это включает:

- Обработку изображений (Vision)
- Использование инструментов, включая все [серверные инструменты](/docs/ru/agents-and-tools/tool-use/server-tools) (веб-поиск, веб-загрузка, выполнение кода, MCP-коннекторы, advisor и поиск инструментов)
- Системные сообщения
- Многоходовые диалоги
- Расширенное мышление
- Большинство бета-функций

Поскольку каждый запрос в пакете обрабатывается независимо, вы можете смешивать различные типы запросов в одном пакете.

Небольшое количество параметров Messages API **не** поддерживается в пакетных запросах. Включение любого из них приводит к ошибке валидации:

ПараметрПричина`stream: true`Результаты пакета возвращаются в виде единого файла, а не потока.`speed` ([Быстрый режим](/docs/ru/build-with-claude/fast-mode))Быстрый режим настраивает задержку синхронных запросов, что неприменимо к асинхронной пакетной обработке.`store` / `previous_thread_event_id` (Threads)Threads хранят состояние; пакетные запросы — нет.`cache_hint` / `context_hint`Эти подсказки маршрутизации применяются только к планированию синхронных запросов.`max_tokens: 0`См. [Ограничения пакетов](#batch-limitations).`research_preview_2026_02: "active"`Режим исследовательского предпросмотра недоступен для пакетной обработки.
Поскольку обработка пакетов может занимать более 5 минут, рассмотрите возможность использования [1-часовой длительности кэша](/docs/ru/build-with-claude/prompt-caching#1-hour-cache-duration) с кэшированием подсказок для повышения частоты попаданий в кэш при обработке пакетов с общим контекстом.

## Цены

Batches API обеспечивает значительную экономию средств. Всё использование тарифицируется по ставке 50% от стандартных цен API.

МодельПакетный вводПакетный выводClaude Fable 5$5 / MTok$25 / MTokClaude Mythos 5 ([ограниченная доступность](https://anthropic.com/glasswing))$5 / MTok$25 / MTokClaude Opus 4.8$2.50 / MTok$12.50 / MTokClaude Opus 4.7$2.50 / MTok$12.50 / MTokClaude Opus 4.6$2.50 / MTok$12.50 / MTokClaude Opus 4.5$2.50 / MTok$12.50 / MTokClaude Opus 4.1 ([устаревшая](/docs/ru/about-claude/model-deprecations))$7.50 / MTok$37.50 / MTokClaude Opus 4 ([выведена из эксплуатации, кроме Vertex AI](/docs/ru/about-claude/model-deprecations))$7.50 / MTok$37.50 / MTokClaude Sonnet 4.6$1.50 / MTok$7.50 / MTokClaude Sonnet 4.5$1.50 / MTok$7.50 / MTokClaude Sonnet 4 ([выведена из эксплуатации, кроме Bedrock и Vertex AI](/docs/ru/about-claude/model-deprecations))$1.50 / MTok$7.50 / MTokClaude Haiku 4.5$0.50 / MTok$2.50 / MTokClaude Haiku 3.5 ([выведена из эксплуатации, кроме Bedrock и Vertex AI](/docs/ru/about-claude/model-deprecations))$0.40 / MTok$2 / MTok

## Как использовать Message Batches API

### Подготовка и создание пакета

Message Batch состоит из списка запросов на создание Message. Структура отдельного запроса включает:

- Уникальный `custom_id` для идентификации запроса Messages. Должен содержать от 1 до 64 символов и включать только буквенно-цифровые символы, дефисы и подчёркивания (соответствовать шаблону `^[a-zA-Z0-9_-]{1,64}$`).
- Объект `params` со стандартными параметрами [Messages API](/docs/ru/api/messages/create)

Вы можете [создать пакет](/docs/ru/api/creating-message-batches), передав этот список в параметр `requests`:

```
`from anthropic.types.message_create_params import MessageCreateParamsNonStreaming
from anthropic.types.messages.batch_create_params import Request

client = anthropic.Anthropic()

message_batch = client.messages.batches.create(
    requests=[
        Request(
            custom_id="my-first-request",
            params=MessageCreateParamsNonStreaming(
                model="claude-opus-4-8",
                max_tokens=1024,
                messages=[
                    {
                        "role": "user",
                        "content": "Hello, world",
                    }
                ],
            ),
        ),
        Request(
            custom_id="my-second-request",
            params=MessageCreateParamsNonStreaming(
                model="claude-opus-4-8",
                max_tokens=1024,
                messages=[
                    {
                        "role": "user",
                        "content": "Hi again, friend",
                    }
                ],
            ),
        ),
    ]
)

print(message_batch)`
```

В этом примере два отдельных запроса объединены в пакет для асинхронной обработки. Каждый запрос имеет уникальный `custom_id` и содержит стандартные параметры, которые вы бы использовали для вызова Messages API.

**Протестируйте ваши пакетные запросы с помощью Messages API**
Валидация объекта `params` для каждого запроса сообщения выполняется асинхронно, и ошибки валидации возвращаются после завершения обработки всего пакета. Вы можете убедиться, что правильно формируете входные данные, предварительно проверив структуру вашего запроса с помощью [Messages API](/docs/ru/api/messages/create).

При первоначальном создании пакета ответ будет иметь статус обработки `in_progress`.

Output
```
`{
  "id": "msgbatch_01HkcTjaV5uDC8jWR4ZsDV8d",
  "type": "message_batch",
  "processing_status": "in_progress",
  "request_counts": {
    "processing": 2,
    "succeeded": 0,
    "errored": 0,
    "canceled": 0,
    "expired": 0
  },
  "ended_at": null,
  "created_at": "2024-09-24T18:37:24.100435Z",
  "expires_at": "2024-09-25T18:37:24.100435Z",
  "cancel_initiated_at": null,
  "results_url": null
}`
```

### Отслеживание вашего пакета

Поле `processing_status` объекта Message Batch указывает, на какой стадии обработки находится пакет. Оно начинается со значения `in_progress`, затем обновляется до `ended`, когда все запросы в пакете завершили обработку и результаты готовы. Вы можете отслеживать состояние вашего пакета, посетив [Console](/settings/workspaces/default/batches) или используя [эндпоинт получения](/docs/ru/api/retrieving-message-batches).

#### Опрос завершения Message Batch

Для опроса Message Batch вам понадобится его `id`, который предоставляется в ответе при создании пакета или при получении списка пакетов. Вы можете реализовать цикл опроса, который периодически проверяет статус пакета до завершения обработки:

```
`import time

client = anthropic.Anthropic()

MESSAGE_BATCH_ID = "msgbatch_01HkcTjaV5uDC8jWR4ZsDV8d"

message_batch = None
while True:
    message_batch = client.messages.batches.retrieve(MESSAGE_BATCH_ID)
    if message_batch.processing_status == "ended":
        break

    print(f"Batch {MESSAGE_BATCH_ID} is still processing...")
    time.sleep(60)
print(message_batch)`
```

### Получение списка всех Message Batches

Вы можете получить список всех Message Batches в вашем Workspace, используя [эндпоинт списка](/docs/ru/api/listing-message-batches). API поддерживает пагинацию, автоматически загружая дополнительные страницы по мере необходимости:

```
`client = anthropic.Anthropic()

# Автоматически загружает дополнительные страницы по мере необходимости.
for message_batch in client.messages.batches.list(limit=20):
    print(message_batch)`
```

### Получение результатов пакета

После завершения обработки пакета каждый запрос Messages в пакете имеет результат. Существует 4 типа результатов:

Тип результатаОписание`succeeded`Запрос выполнен успешно. Включает результат сообщения.`errored`Запрос столкнулся с ошибкой, и сообщение не было создано. Возможные ошибки включают недопустимые запросы и внутренние ошибки сервера. Плата за эти запросы не взимается.`canceled`Пользователь отменил пакет до того, как этот запрос мог быть отправлен модели. Плата за эти запросы не взимается.`expired`Срок действия пакета (24 часа) истёк до того, как этот запрос мог быть отправлен модели. Плата за эти запросы не взимается.
Вы увидите обзор ваших результатов в поле `request_counts` пакета, которое показывает, сколько запросов достигло каждого из этих четырёх состояний.

Результаты пакета доступны для скачивания по свойству `results_url` объекта Message Batch, а также в Console, если это разрешено настройками организации. Из-за потенциально большого размера результатов рекомендуется [получать результаты потоком](/docs/ru/api/retrieving-message-batch-results), а не скачивать их все сразу.

```
`client = anthropic.Anthropic()

# Потоковая передача файла результатов эффективными по памяти фрагментами, обрабатывая по одному за раз
for result in client.messages.batches.results(
    "msgbatch_01HkcTjaV5uDC8jWR4ZsDV8d",
):
    match result.result.type:
        case "succeeded":
            print(f"Success! {result.custom_id}")
        case "errored":
            if result.result.error.error.type == "invalid_request_error":
                # Тело запроса должно быть исправлено перед повторной отправкой запроса
                print(f"Validation error {result.custom_id}")
            else:
                # Запрос можно повторить напрямую
                print(f"Server error {result.custom_id}")
        case "expired":
            print(f"Request expired {result.custom_id}")`
```

Результаты представлены в формате `.jsonl`, где каждая строка является валидным JSON-объектом, представляющим результат одного запроса в Message Batch. Для каждого полученного потоком результата вы можете выполнять различные действия в зависимости от его `custom_id` и типа результата. Вот пример набора результатов:

.jsonl file
```
`{"custom_id":"my-second-request","result":{"type":"succeeded","message":{"id":"msg_014VwiXbi91y3JMjcpyGBHX5","type":"message","role":"assistant","model":"claude-opus-4-8","content":[{"type":"text","text":"Hello again! It's nice to see you. How can I assist you today? Is there anything specific you'd like to chat about or any questions you have?"}],"stop_reason":"end_turn","stop_sequence":null,"usage":{"input_tokens":11,"output_tokens":36}}}}
{"custom_id":"my-first-request","result":{"type":"succeeded","message":{"id":"msg_01FqfsLoHwgeFbguDgpz48m7","type":"message","role":"assistant","model":"claude-opus-4-8","content":[{"type":"text","text":"Hello! How can I assist you today? Feel free to ask me any questions or let me know if there's anything you'd like to chat about."}],"stop_reason":"end_turn","stop_sequence":null,"usage":{"input_tokens":10,"output_tokens":34}}}}`
```

Если ваш результат содержит ошибку, его поле `result.error` будет установлено в стандартную [структуру ошибки](/docs/ru/api/errors#error-shapes).

**Результаты пакета могут не соответствовать порядку входных данных**
Результаты пакета могут возвращаться в любом порядке и могут не соответствовать порядку запросов при создании пакета. В приведённом выше примере результат для второго запроса пакета возвращается раньше первого. Чтобы правильно сопоставить результаты с соответствующими запросами, всегда используйте поле `custom_id`.

### Отмена Message Batch

Вы можете отменить Message Batch, который в данный момент обрабатывается, используя [эндпоинт отмены](/docs/ru/api/canceling-message-batches). Сразу после отмены `processing_status` пакета будет иметь значение `canceling`. Вы можете использовать ту же технику опроса, описанную выше, чтобы дождаться завершения отмены. Отменённые пакеты в итоге получают статус `ended` и могут содержать частичные результаты для запросов, которые были обработаны до отмены.

```
`client = anthropic.Anthropic()

MESSAGE_BATCH_ID = "msgbatch_01HkcTjaV5uDC8jWR4ZsDV8d"

message_batch = client.messages.batches.cancel(
    MESSAGE_BATCH_ID,
)
print(message_batch)`
```

Ответ покажет пакет в состоянии `canceling`:

Output
```
`{
  "id": "msgbatch_013Zva2CMHLNnXjNJJKqJ2EF",
  "type": "message_batch",
  "processing_status": "canceling",
  "request_counts": {
    "processing": 2,
    "succeeded": 0,
    "errored": 0,
    "canceled": 0,
    "expired": 0
  },
  "ended_at": null,
  "created_at": "2024-09-24T18:37:24.100435Z",
  "expires_at": "2024-09-25T18:37:24.100435Z",
  "cancel_initiated_at": "2024-09-24T18:39:03.114875Z",
  "results_url": null
}`
```

### Использование кэширования подсказок с Message Batches

Message Batches API поддерживает кэширование подсказок, что позволяет потенциально снизить затраты и время обработки пакетных запросов. Ценовые скидки от кэширования подсказок и Message Batches могут суммироваться, обеспечивая ещё большую экономию при совместном использовании обеих функций. Однако, поскольку пакетные запросы обрабатываются асинхронно и параллельно, попадания в кэш предоставляются по принципу «насколько возможно». Пользователи обычно наблюдают частоту попаданий в кэш от 30% до 98% в зависимости от характера их трафика.

Чтобы максимизировать вероятность попаданий в кэш в ваших пакетных запросах:

- Включайте идентичные блоки `cache_control` в каждый запрос Message внутри вашего пакета
- Поддерживайте постоянный поток запросов, чтобы предотвратить истечение срока действия записей кэша после их 5-минутного времени жизни
- Структурируйте ваши запросы так, чтобы они разделяли как можно больше кэшированного контента

Пример реализации кэширования подсказок в пакете:

```
`from anthropic.types.message_create_params import MessageCreateParamsNonStreaming
from anthropic.types.messages.batch_create_params import Request

client = anthropic.Anthropic()

message_batch = client.messages.batches.create(
    requests=[
        Request(
            custom_id="my-first-request",
            params=MessageCreateParamsNonStreaming(
                model="claude-opus-4-8",
                max_tokens=1024,
                system=[
                    {
                        "type": "text",
                        "text": "You are an AI assistant tasked with analyzing literary works. Your goal is to provide insightful commentary on themes, characters, and writing style.\n",
                    },
                    {
                        "type": "text",
                        "text": "<the entire contents of Pride and Prejudice>",
                        "cache_control": {"type": "ephemeral"},
                    },
                ],
                messages=[
                    {
                        "role": "user",
                        "content": "Analyze the major themes in Pride and Prejudice.",
                    }
                ],
            ),
        ),
        Request(
            custom_id="my-second-request",
            params=MessageCreateParamsNonStreaming(
                model="claude-opus-4-8",
                max_tokens=1024,
                system=[
                    {
                        "type": "text",
                        "text": "You are an AI assistant tasked with analyzing literary works. Your goal is to provide insightful commentary on themes, characters, and writing style.\n",
                    },
                    {
                        "type": "text",
                        "text": "<the entire contents of Pride and Prejudice>",
                        "cache_control": {"type": "ephemeral"},
                    },
                ],
                messages=[
                    {
                        "role": "user",
                        "content": "Write a summary of Pride and Prejudice.",
                    }
                ],
            ),
        ),
    ]
)`
```

В этом примере оба запроса в пакете включают идентичные системные сообщения и полный текст «Гордости и предубеждения», помеченный `cache_control` для увеличения вероятности попаданий в кэш.

### Серверные инструменты и агентный цикл

Все [серверные инструменты](/docs/ru/agents-and-tools/tool-use/server-tools) (веб-поиск, веб-загрузка, выполнение кода, MCP-коннекторы, advisor и поиск инструментов) работают в пакетных запросах. Обработчик пакетов выполняет тот же серверный агентный цикл, что и синхронный Messages API.

Поскольку открытое соединение поддерживать не требуется, пакетный цикл выполняет **больше итераций за ход**, чем синхронный запрос, прежде чем вернуть `stop_reason: "pause_turn"`. Если результат пакета возвращается с `pause_turn`, ход не завершён; вы можете продолжить его, отправив приостановленный контент ассистента в последующем запросе (пакетном или синхронном) точно так, как показано в [паттерне продолжения pause_turn](/docs/ru/agents-and-tools/tool-use/server-tools#the-server-side-loop-and-pause-turn).

Обработчик пакетов дополнительно ограничивает `web_search` на уровне организации, чтобы высокопараллельная пакетная обработка не исчерпала ограничение скорости веб-поиска вашей организации. Пакет автоматически повторяет ограниченные запросы; вам не нужно обрабатывать это самостоятельно, но очень большие пакеты с веб-поиском могут выполняться дольше.

### Расширенный вывод (бета)

Бета-заголовок `output-300k-2026-03-24` повышает лимит `max_tokens` до 300 000 для пакетных запросов, использующих Claude Opus 4.8, Claude Opus 4.7, Claude Opus 4.6 или Claude Sonnet 4.6. Включите этот заголовок, чтобы генерировать выводы, значительно превышающие стандартный лимит (от 64k до 128k в зависимости от модели), за один ход.

Расширенный вывод доступен только в Message Batches API, но не в синхронном Messages API. Он поддерживается в Claude API и Claude Platform на AWS и в настоящее время недоступен в Amazon Bedrock, Vertex AI или Microsoft Foundry.

Используйте расширенный вывод для генерации длинных текстов, таких как черновики объёмом с книгу и техническая документация, исчерпывающее извлечение структурированных данных, крупные каркасы генерации кода и длинные цепочки рассуждений.

Одна генерация на 300k токенов может занять более часа, поэтому планируйте отправку пакетов с учётом 24-часового окна обработки. Применяется стандартное ценообразование для пакетов (50% от стандартных цен API).

```
`from anthropic.types.beta.message_create_params import MessageCreateParamsNonStreaming
from anthropic.types.beta.messages.batch_create_params import Request

client = anthropic.Anthropic()

message_batch = client.beta.messages.batches.create(
    betas=["output-300k-2026-03-24"],
    requests=[
        Request(
            custom_id="long-form-request",
            params=MessageCreateParamsNonStreaming(
                model="claude-opus-4-8",
                max_tokens=300_000,
                messages=[
                    {
                        "role": "user",
                        "content": "Write a comprehensive technical guide to building distributed systems, covering architecture patterns, consistency models, fault tolerance, and operational best practices.",
                    }
                ],
            ),
        ),
    ],
)

print(message_batch)`
```

### Лучшие практики для эффективной пакетной обработки

Чтобы получить максимальную отдачу от Batches API:

- Регулярно отслеживайте статус обработки пакета и реализуйте соответствующую логику повторных попыток для неудачных запросов.
- Используйте осмысленные значения `custom_id`, чтобы легко сопоставлять результаты с запросами, поскольку порядок не гарантируется.
- Рассмотрите возможность разбиения очень больших наборов данных на несколько пакетов для лучшей управляемости.
- Выполните пробный запуск структуры одного запроса с помощью Messages API, чтобы избежать ошибок валидации.

### Устранение распространённых проблем

Если вы наблюдаете неожиданное поведение:

- Убедитесь, что общий размер пакетного запроса не превышает 256 МБ. Если размер запроса слишком велик, вы можете получить ошибку 413 `request_too_large`.
- Проверьте, что вы используете [поддерживаемые модели](#supported-models) для всех запросов в пакете.
- Убедитесь, что каждый запрос в пакете имеет уникальный `custom_id`.
- Убедитесь, что с момента `created_at` пакета (а не `ended_at` обработки) прошло менее 29 дней. Если прошло более 29 дней, результаты больше не будут доступны для просмотра.
- Подтвердите, что пакет не был отменён.

Обратите внимание, что сбой одного запроса в пакете не влияет на обработку других запросов.

## Хранение пакетов и конфиденциальность

- 
**Изоляция Workspace**: пакеты изолированы в рамках Workspace, в котором они созданы. Доступ к ним возможен только с помощью ключей API, связанных с этим Workspace, или для пользователей с разрешением на просмотр пакетов Workspace в Console.

- 
**Доступность результатов**: результаты пакета доступны в течение 29 дней после создания пакета, что даёт достаточно времени для получения и обработки.

## Хранение данных

Пакетная обработка хранит данные запросов и ответов до 29 дней после создания пакета. Вы можете удалить пакет сообщений в любое время после обработки, используя эндпоинт `DELETE /v1/messages/batches/{batch_id}`. Чтобы удалить пакет, находящийся в процессе обработки, сначала отмените его. Асинхронная обработка требует хранения на стороне сервера как входных, так и выходных данных до завершения пакета и получения результатов.

Информацию о соответствии требованиям ZDR для всех функций см. в разделе [API и хранение данных](/docs/ru/manage-claude/api-and-data-retention).

## Часто задаваемые вопросы

### 

### 

### 

### 

### 

### 

### 

### 

### 

## Дальнейшие шаги

[Результаты поискаОбеспечьте естественное цитирование для RAG-приложений, предоставляя результаты поиска с указанием источника.
](/docs/ru/build-with-claude/search-results)[[icon]Кэширование подсказокСнизьте затраты и задержку, кэшируя префиксы подсказок, общие для запросов в пакете.
](/docs/ru/build-with-claude/prompt-caching)Was this page helpful?