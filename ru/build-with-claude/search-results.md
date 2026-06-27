---
title: "Результаты поиска"
url: "https://docs.claude.com/ru/build-with-claude/search-results"
lang: "ru"
---

Сообщения/Возможности модели
# Результаты поиска
Обеспечьте естественные цитирования для RAG-приложений, предоставляя результаты поиска с указанием источникаЭта функция соответствует требованиям [Zero Data Retention (ZDR)](/docs/ru/build-with-claude/api-and-data-retention) (нулевого хранения данных). Если у вашей организации действует соглашение ZDR, данные, отправленные через эту функцию, не сохраняются после возврата ответа API.

Блоки контента с результатами поиска обеспечивают естественные цитирования с корректным указанием источника, привнося качество цитирования уровня веб-поиска в ваши собственные приложения. Эта функция особенно полезна для приложений «Retrieval-Augmented Generation» (генерация с дополнением извлечёнными данными), или RAG, где требуется, чтобы Claude точно ссылался на источники.

Функция результатов поиска доступна в следующих моделях:

- Claude Opus 4.8 (claude-opus-4-8)
- Claude Opus 4.7 (`claude-opus-4-7`)
- Claude Opus 4.6 (`claude-opus-4-6`)
- Claude Sonnet 4.6 (`claude-sonnet-4-6`)
- Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)
- Claude Opus 4.5 (`claude-opus-4-5-20251101`)
- Claude Opus 4.1 ([устарела](/docs/ru/about-claude/model-deprecations)) (`claude-opus-4-1-20250805`)
- Claude Opus 4 ([выведена из эксплуатации, кроме Vertex AI](/docs/ru/about-claude/model-deprecations)) (`claude-opus-4-20250514`)
- Claude Sonnet 4 ([выведена из эксплуатации, кроме Bedrock и Vertex AI](/docs/ru/about-claude/model-deprecations)) (`claude-sonnet-4-20250514`)
- Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)
- Claude Haiku 3.5 ([выведена из эксплуатации, кроме Bedrock и Vertex AI](/docs/ru/about-claude/model-deprecations)) (`claude-3-5-haiku-20241022`)

## Ключевые преимущества

- **Естественные цитирования:** достигайте того же качества цитирования, что и при веб-поиске, для любого контента
- **Гибкая интеграция:** используйте в возвращаемых значениях инструментов для динамического RAG или как контент верхнего уровня для предварительно полученных данных
- **Корректное указание источника:** каждый результат включает информацию об источнике и заголовке для чёткой атрибуции
- **Не требуются обходные решения с документами:** устраняет необходимость в обходных решениях на основе документов
- **Единообразный формат цитирования:** соответствует качеству и формату цитирования функциональности веб-поиска Claude

## Как это работает

Результаты поиска могут предоставляться двумя способами:

- **Из вызовов инструментов:** ваши пользовательские инструменты возвращают результаты поиска, что позволяет создавать динамические RAG-приложения
- **Как контент верхнего уровня:** вы предоставляете результаты поиска непосредственно в сообщениях пользователя для предварительно полученного или кэшированного контента

В обоих случаях Claude может автоматически цитировать информацию из результатов поиска с корректным указанием источника.

### Схема результата поиска

Результаты поиска используют следующую структуру:

```
`{
  "type": "search_result",
  "source": "https://example.com/article", // Required: Source URL or identifier
  "title": "Article Title", // Required: Title of the result
  "content": [
    // Required: Array of text blocks
    {
      "type": "text",
      "text": "The actual content of the search result..."
    }
  ],
  "citations": {
    // Optional: Citation configuration
    "enabled": true // Enable/disable citations for this result
  }
}`
```

### Обязательные поля

ПолеТипОписание`type`stringДолжно быть `"search_result"``source`stringURL источника или идентификатор контента`title`stringОписательный заголовок для результата поиска`content`arrayМассив текстовых блоков, содержащих фактический контент

### Необязательные поля

ПолеТипОписание`citations`objectКонфигурация цитирования с булевым полем `enabled``cache_control`objectНастройки управления кэшем (например, `{"type": "ephemeral"}`)
Каждый элемент массива `content` должен быть текстовым блоком со следующими полями:

- `type`: должно быть `"text"`
- `text`: фактическое текстовое содержимое (непустая строка)

## Метод 1: результаты поиска из вызовов инструментов

Наиболее мощный сценарий использования — возврат результатов поиска из ваших пользовательских инструментов. Это позволяет создавать динамические RAG-приложения, в которых инструменты извлекают и возвращают релевантный контент с автоматическим цитированием.

### Пример: инструмент базы знаний

```
`from anthropic.types import (
    MessageParam,
    TextBlockParam,
    SearchResultBlockParam,
    ToolResultBlockParam,
)

client = Anthropic()

# Определите инструмент поиска по базе знаний
knowledge_base_tool = {
    "name": "search_knowledge_base",
    "description": "Search the company knowledge base for information",
    "input_schema": {
        "type": "object",
        "properties": {"query": {"type": "string", "description": "The search query"}},
        "required": ["query"],
    },
}

# Функция для обработки вызова инструмента
def search_knowledge_base(query):
    # Здесь ваша логика поиска
    # Возвращает результаты поиска в правильном формате
    return [
        SearchResultBlockParam(
            type="search_result",
            source="https://docs.company.com/product-guide",
            title="Product Configuration Guide",
            content=[
                TextBlockParam(
                    type="text",
                    text="To configure the product, navigate to Settings > Configuration. The default timeout is 30 seconds, but can be adjusted between 10-120 seconds based on your needs.",
                )
            ],
            citations={"enabled": True},
        ),
        SearchResultBlockParam(
            type="search_result",
            source="https://docs.company.com/troubleshooting",
            title="Troubleshooting Guide",
            content=[
                TextBlockParam(
                    type="text",
                    text="If you encounter timeout errors, first check the configuration settings. Common causes include network latency and incorrect timeout values.",
                )
            ],
            citations={"enabled": True},
        ),
    ]

# Создайте сообщение с инструментом
response = client.messages.create(
    model="claude-opus-4-8",  # Works with all supported models
    max_tokens=1024,
    tools=[knowledge_base_tool],
    messages=[
        MessageParam(role="user", content="How do I configure the timeout settings?")
    ],
)

# Когда Claude вызывает инструмент, предоставьте результаты поиска
if response.content[0].type == "tool_use":
    tool_result = search_knowledge_base(response.content[0].input["query"])

    # Отправьте результат инструмента обратно
    final_response = client.messages.create(
        model="claude-opus-4-8",  # Works with all supported models
        max_tokens=1024,
        messages=[
            MessageParam(
                role="user", content="How do I configure the timeout settings?"
            ),
            MessageParam(role="assistant", content=response.content),
            MessageParam(
                role="user",
                content=[
                    ToolResultBlockParam(
                        type="tool_result",
                        tool_use_id=response.content[0].id,
                        content=tool_result,  # Search results go here
                    )
                ],
            ),
        ],
    )`
```

## Метод 2: результаты поиска как контент верхнего уровня

Вы также можете предоставлять результаты поиска непосредственно в сообщениях пользователя. Это полезно для:

- Предварительно полученного контента из вашей поисковой инфраструктуры
- Кэшированных результатов поиска из предыдущих запросов
- Контента из внешних поисковых сервисов
- Тестирования и разработки

### Пример: прямые результаты поиска

```
`from anthropic.types import MessageParam, TextBlockParam, SearchResultBlockParam

client = Anthropic()

# Предоставьте результаты поиска непосредственно в сообщении пользователя
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=1024,
    messages=[
        MessageParam(
            role="user",
            content=[
                SearchResultBlockParam(
                    type="search_result",
                    source="https://docs.company.com/api-reference",
                    title="API Reference - Authentication",
                    content=[
                        TextBlockParam(
                            type="text",
                            text="All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard. Rate limits: 1000 requests per hour for standard tier, 10000 for premium.",
                        )
                    ],
                    citations={"enabled": True},
                ),
                SearchResultBlockParam(
                    type="search_result",
                    source="https://docs.company.com/quickstart",
                    title="Getting Started Guide",
                    content=[
                        TextBlockParam(
                            type="text",
                            text="To get started: 1) Sign up for an account, 2) Generate an API key from the dashboard, 3) Install our SDK using pip install company-sdk, 4) Initialize the client with your API key.",
                        )
                    ],
                    citations={"enabled": True},
                ),
                TextBlockParam(
                    type="text",
                    text="Based on these search results, how do I authenticate API requests and what are the rate limits?",
                ),
            ],
        )
    ],
)

print(response)`
```

## Ответ Claude с цитированиями

Независимо от того, как предоставляются результаты поиска, Claude автоматически включает цитирования при использовании информации из них:

```
`{
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard.",
      "citations": [
        {
          "type": "search_result_location",
          "cited_text": "All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard. Rate limits: 1000 requests per hour for standard tier, 10000 for premium.",
          "source": "https://docs.company.com/api-reference",
          "title": "API Reference - Authentication",
          "search_result_index": 0,
          "start_block_index": 0,
          "end_block_index": 1
        }
      ]
    },
    {
      "type": "text",
      "text": "\n\nTo set this up from scratch, you'll need to "
    },
    {
      "type": "text",
      "text": "sign up for an account, generate an API key from the dashboard, install the SDK using `pip install company-sdk`, and initialize the client with your API key.",
      "citations": [
        {
          "type": "search_result_location",
          "cited_text": "To get started: 1) Sign up for an account, 2) Generate an API key from the dashboard, 3) Install our SDK using pip install company-sdk, 4) Initialize the client with your API key.",
          "source": "https://docs.company.com/quickstart",
          "title": "Getting Started Guide",
          "search_result_index": 1,
          "start_block_index": 0,
          "end_block_index": 1
        }
      ]
    }
  ]
}`
```

### Поля цитирования

Каждое цитирование включает:

ПолеТипОписание`type`stringВсегда `"search_result_location"` для цитирований результатов поиска`source`stringИсточник из исходного результата поиска`title`string или nullЗаголовок из исходного результата поиска`cited_text`stringПолный текст процитированного блока (блоков), объединённый в одну строку. Равен содержимому `content[start_block_index:end_block_index]`, соединённому вместе. Не учитывается в выходных токенах.`search_result_index`integerИндекс (начиная с 0) процитированного результата поиска среди всех блоков `search_result` в запросе, в порядке их появления (по всем сообщениям и результатам инструментов).`start_block_index`integerИндекс (начиная с 0) первого процитированного блока в массиве `content` результата поиска.`end_block_index`integerИсключающий конечный индекс диапазона процитированных блоков в массиве `content` результата поиска. Всегда больше, чем `start_block_index`.
Индексы блоков определяют срез массива `content` результата поиска, а `cited_text` — это полный текст этого среза. Текстовый блок является минимальной цитируемой единицей: Claude цитирует блоки целиком, а не подстроки внутри блока. Чтобы получить более детальные цитирования, разбейте контент результата поиска на более мелкие блоки (см. [Несколько блоков контента](#multiple-content-blocks)).

## Несколько блоков контента

Результаты поиска могут содержать несколько текстовых блоков в массиве `content`:

```
`{
  "type": "search_result",
  "source": "https://docs.company.com/api-guide",
  "title": "API Documentation",
  "content": [
    {
      "type": "text",
      "text": "Authentication: All API requests require an API key."
    },
    {
      "type": "text",
      "text": "Rate Limits: The API allows 1000 requests per hour per key."
    },
    {
      "type": "text",
      "text": "Error Handling: The API returns standard HTTP status codes."
    }
  ]
}`
```

Цитирование, ссылающееся на блок об ограничениях скорости, выглядит так:

```
`{
  "type": "search_result_location",
  "cited_text": "Rate Limits: The API allows 1000 requests per hour per key.",
  "source": "https://docs.company.com/api-guide",
  "title": "API Documentation",
  "search_result_index": 0,
  "start_block_index": 1,
  "end_block_index": 2
}`
```

Когда этот результат поиска цитируется, `start_block_index` и `end_block_index` указывают, какие из этих блоков охватывает цитирование, а `cited_text` содержит в точности текст этих блоков. Разбиение контента на более мелкие, сфокусированные блоки даёт Claude более точные границы цитирования; объединение контента в один блок означает, что каждое цитирование возвращает полный текст. Это та же модель, что используется в [документах с пользовательским контентом](/docs/ru/build-with-claude/citations#custom-content-documents) в функции цитирования.

## Расширенное использование

### Сочетание обоих методов

Вы можете использовать как результаты поиска на основе инструментов, так и результаты поиска верхнего уровня в одном разговоре:

```
`from anthropic.types import MessageParam, SearchResultBlockParam, TextBlockParam

# Первое сообщение с результатами поиска верхнего уровня
messages = [
    MessageParam(
        role="user",
        content=[
            SearchResultBlockParam(
                type="search_result",
                source="https://docs.company.com/overview",
                title="Product Overview",
                content=[
                    TextBlockParam(
                        type="text", text="Our product helps teams collaborate..."
                    )
                ],
                citations={"enabled": True},
            ),
            TextBlockParam(
                type="text",
                text="Tell me about this product and search for pricing information",
            ),
        ],
    )
]

# Claude может ответить и вызвать инструмент для поиска цен
# Затем вы предоставляете результаты инструмента с дополнительными результатами поиска`
```

### Сочетание с другими типами контента

Оба метода поддерживают смешивание результатов поиска с другим контентом:

```
`from anthropic.types import SearchResultBlockParam, TextBlockParam

# В результатах инструментов
tool_result = [
    SearchResultBlockParam(
        type="search_result",
        source="https://docs.company.com/guide",
        title="User Guide",
        content=[TextBlockParam(type="text", text="Configuration details...")],
        citations={"enabled": True},
    ),
    TextBlockParam(
        type="text", text="Additional context: This applies to version 2.0 and later."
    ),
]

# В содержимом верхнего уровня
user_content = [
    SearchResultBlockParam(
        type="search_result",
        source="https://research.com/paper",
        title="Research Paper",
        content=[TextBlockParam(type="text", text="Key findings...")],
        citations={"enabled": True},
    ),
    {
        "type": "image",
        "source": {"type": "url", "url": "https://example.com/chart.png"},
    },
    TextBlockParam(
        type="text", text="How does the chart relate to the research findings?"
    ),
]`
```

### Управление кэшем

Добавьте управление кэшем для повышения производительности:

```
`{
  "type": "search_result",
  "source": "https://docs.company.com/guide",
  "title": "User Guide",
  "content": [{ "type": "text", "text": "..." }],
  "cache_control": {
    "type": "ephemeral"
  }
}`
```

### Управление цитированием

По умолчанию цитирования для результатов поиска отключены. Вы можете включить цитирования, явно задав конфигурацию `citations`:

```
`{
  "type": "search_result",
  "source": "https://docs.company.com/guide",
  "title": "User Guide",
  "content": [{ "type": "text", "text": "Important documentation..." }],
  "citations": {
    "enabled": true // Enable citations for this result
  }
}`
```

Когда `citations.enabled` установлено в `true`, Claude включает ссылки на источники при использовании информации из результата поиска. Это обеспечивает:

- Естественные цитирования для ваших пользовательских RAG-приложений
- Указание источника при взаимодействии с проприетарными базами знаний
- Цитирования уровня веб-поиска для любого пользовательского инструмента, возвращающего результаты поиска

Цитирования работают по принципу «всё или ничего»: либо у всех результатов поиска в запросе цитирования должны быть включены, либо у всех отключены. Смешивание результатов поиска с разными настройками цитирования приводит к ошибке.

## Рекомендации

### Для поиска на основе инструментов (Метод 1)

- **Динамический контент:** используйте для поиска в реальном времени и динамических RAG-приложений
- **Обработка ошибок:** возвращайте соответствующие сообщения при сбоях поиска
- **Ограничение результатов:** возвращайте только наиболее релевантные результаты, чтобы избежать переполнения контекста

### Для поиска верхнего уровня (Метод 2)

- **Предварительно полученный контент:** используйте, когда у вас уже есть результаты поиска
- **Пакетная обработка:** идеально подходит для обработки нескольких результатов поиска одновременно
- **Тестирование:** отлично подходит для тестирования поведения цитирования с известным контентом

### Общие рекомендации

- 
**Эффективно структурируйте результаты:**

Используйте понятные, постоянные URL источников
- Предоставляйте описательные заголовки
- Разбивайте длинный контент на логические текстовые блоки, чтобы дать Claude более точные границы цитирования

- 
**Поддерживайте единообразие:**

Используйте единообразные форматы источников во всём приложении
- Убедитесь, что заголовки точно отражают контент
- Сохраняйте единообразие форматирования

- 
**Корректно обрабатывайте ошибки:**

```
`def search_with_fallback(query):
    try:
        results = perform_search(query)
        if not results:
            return {"type": "text", "text": "No results found."}
        return format_as_search_results(results)
    except Exception as e:
        return {"type": "text", "text": f"Search error: {str(e)}"}`
```

## Ограничения

- Блоки контента с результатами поиска доступны в Claude API, Amazon Bedrock и Vertex AI от Google Cloud
- Внутри результатов поиска поддерживается только текстовый контент (без изображений или других медиа)
- Массив `content` должен содержать как минимум один текстовый блок

## Дальнейшие шаги

[ЦитированияУзнайте, как работают цитирования для документов, пользовательского контента и результатов поиска.
](/docs/ru/build-with-claude/citations)[Инструмент веб-поискаПозвольте Claude искать в интернете и автоматически цитировать источники с помощью серверного инструмента.
](/docs/ru/agents-and-tools/tool-use/web-search-tool)[Справочник Messages APIОзнакомьтесь с полной документацией Messages API, включая типы блоков контента.
](/docs/ru/api/messages/create)[[icon]Кэширование подсказокКэшируйте результаты поиска с помощью `cache_control`, чтобы снизить стоимость и задержку при повторных запросах.
](/docs/ru/build-with-claude/prompt-caching)Was this page helpful?