# Потоковая передача сообщений (Streaming Messages)

Установите `stream: true` для получения инкрементальных ответов через SSE.

## Типы событий
- `message_start` — Содержит объект Message с пустым контентом
- `content_block_start` — Начало нового блока контента
- `content_block_delta` — Дельта контента (text_delta, input_json_delta, thinking_delta, signature_delta)
- `content_block_stop` — Конец блока контента
- `message_delta` — Изменения верхнего уровня (stop_reason)
- `message_stop` — Терминатор потока

## Типы дельт
- `text_delta` — Текстовый контент
- `input_json_delta` — Частичный JSON для использования инструментов
- `thinking_delta` — Контент расширенного мышления
- `signature_delta` — Проверка целостности для блоков мышления

## Потоковые отказы
Начиная с Claude 4, потоковые ответы возвращают `stop_reason: "refusal"` когда классификаторы обнаруживают нарушения. Сбросьте контекст после отказа.
