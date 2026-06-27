---
title: "Эмбеддинги"
url: "https://docs.claude.com/ru/build-with-claude/embeddings"
lang: "ru"
---

Сообщения/Возможности модели
# Эмбеддинги
Текстовые эмбеддинги — это числовые представления текста, позволяющие измерять семантическое сходство. В этом руководстве рассматриваются эмбеддинги, их применение и использование моделей эмбеддингов для таких задач, как поиск, рекомендации и обнаружение аномалий.
## Перед внедрением эмбеддингов

При выборе поставщика эмбеддингов можно учитывать несколько факторов в зависимости от ваших потребностей и предпочтений:

- Размер набора данных и специфика предметной области: размер обучающего набора данных модели и его релевантность предметной области, для которой вы хотите создавать эмбеддинги. Более крупные или более специализированные данные, как правило, дают более качественные эмбеддинги внутри предметной области
- Производительность инференса: скорость поиска эмбеддингов и сквозная задержка. Это особенно важный фактор для крупномасштабных производственных развёртываний
- Возможности настройки: варианты дообучения на приватных данных или специализации моделей для очень узких предметных областей. Это может улучшить производительность на уникальных словарях

## Как получить эмбеддинги с помощью Anthropic

Anthropic не предлагает собственную модель эмбеддингов. Одним из поставщиков эмбеддингов с широким спектром возможностей, охватывающих все перечисленные выше соображения, является Voyage AI.

Voyage AI создаёт передовые модели эмбеддингов и предлагает специализированные модели для конкретных отраслей, таких как финансы и здравоохранение, а также индивидуально дообученные модели для отдельных клиентов.

Остальная часть этого руководства посвящена Voyage AI, однако вам следует оценить различных поставщиков эмбеддингов, чтобы найти наиболее подходящий вариант для вашего конкретного сценария использования.

## Доступные модели

Voyage рекомендует использовать следующие модели текстовых эмбеддингов:

**Voyage 4 (последнее поколение)**

МодельДлина контекстаРазмерность эмбеддингаОписание`voyage-4-large`32 0001024 (по умолчанию), 256, 512, 2048Лучшее качество универсального и многоязычного поиска. Подробности см. в [блоге](https://blog.voyageai.com/2026/01/15/voyage-4/).`voyage-4`32 0001024 (по умолчанию), 256, 512, 2048Оптимизирована для качества универсального и многоязычного поиска. Баланс качества и эффективности. Подробности см. в [блоге](https://blog.voyageai.com/2026/01/15/voyage-4/).`voyage-4-lite`32 0001024 (по умолчанию), 256, 512, 2048Оптимизирована для задержки и стоимости. Подробности см. в [блоге](https://blog.voyageai.com/2026/01/15/voyage-4/).`voyage-4-nano`32 0001024 (по умолчанию), 256, 512, 2048Модель с открытыми весами (лицензия Apache 2.0), доступная на Hugging Face. Подробности см. в [блоге](https://blog.voyageai.com/2026/01/15/voyage-4/).
**Предыдущее поколение**

МодельДлина контекстаРазмерность эмбеддингаОписание`voyage-3-large`32 0001024 (по умолчанию), 256, 512, 2048Лучшее качество универсального и многоязычного поиска. Подробности см. в [блоге](https://blog.voyageai.com/2025/01/07/voyage-3-large/).`voyage-3.5`32 0001024 (по умолчанию), 256, 512, 2048Оптимизирована для качества универсального и многоязычного поиска. Подробности см. в [блоге](https://blog.voyageai.com/2025/05/20/voyage-3-5/).`voyage-3.5-lite`32 0001024 (по умолчанию), 256, 512, 2048Оптимизирована для задержки и стоимости. Подробности см. в [блоге](https://blog.voyageai.com/2025/05/20/voyage-3-5/).`voyage-code-3`32 0001024 (по умолчанию), 256, 512, 2048Оптимизирована для поиска по **коду**. Подробности см. в [блоге](https://blog.voyageai.com/2024/12/04/voyage-code-3/).`voyage-finance-2`32 0001024Оптимизирована для поиска и RAG в области **финансов**. Подробности см. в [блоге](https://blog.voyageai.com/2024/06/03/domain-specific-embeddings-finance-edition-voyage-finance-2/).`voyage-law-2`16 0001024Оптимизирована для поиска и RAG в **юридической** области и для **длинного контекста**. Также улучшена производительность во всех предметных областях. Подробности см. в [блоге](https://blog.voyageai.com/2024/04/15/domain-specific-embeddings-and-retrieval-legal-edition-voyage-law-2/).
Кроме того, рекомендуются следующие мультимодальные модели эмбеддингов:

МодельДлина контекстаРазмерность эмбеддингаОписание`voyage-multimodal-3.5`32 0001024 (по умолчанию), 256, 512, 2048Богатая мультимодальная модель эмбеддингов, способная векторизовать чередующиеся текст, изображения и видео. Включает поддержку видео как первая модель эмбеддингов видео производственного уровня. Подробности см. в [блоге](https://blog.voyageai.com/2026/01/15/voyage-multimodal-3-5/).`voyage-multimodal-3`32 0001024Богатая мультимодальная модель эмбеддингов, способная векторизовать чередующиеся текст и насыщенные содержанием изображения, такие как скриншоты PDF-файлов, слайды, таблицы, рисунки и многое другое. Подробности см. в [блоге](https://blog.voyageai.com/2024/11/12/voyage-multimodal-3/).
Нужна помощь в выборе модели текстовых эмбеддингов? Ознакомьтесь с [FAQ](https://docs.voyageai.com/docs/faq#what-embedding-models-are-available-and-which-one-should-i-use&ref=anthropic).

## Начало работы с Voyage AI

Чтобы получить доступ к эмбеддингам Voyage:

- Зарегистрируйтесь на сайте Voyage AI
- Получите ключ API
- Для удобства установите ключ API как переменную окружения:

```
`export VOYAGE_API_KEY="<your secret key>"`
```

Вы можете получить эмбеддинги, используя официальный [Python-пакет `voyageai`](https://github.com/voyage-ai/voyageai-python) или HTTP-запросы, как описано ниже.

### Python-библиотека Voyage

Пакет `voyageai` можно установить с помощью следующей команды:

```
`pip install -U voyageai`
```

Затем вы можете создать объект клиента и начать использовать его для создания эмбеддингов ваших текстов:

```
`import voyageai

vo = voyageai.Client()
# Будет автоматически использована переменная окружения VOYAGE_API_KEY.
# Либо можно использовать vo = voyageai.Client(api_key="<ваш секретный ключ>")

texts = ["Sample text 1", "Sample text 2"]

result = vo.embed(texts, model="voyage-4", input_type="document")
print(result.embeddings[0])
print(result.embeddings[1])`
```

`result.embeddings` будет списком из двух векторов эмбеддингов, каждый из которых содержит 1024 числа с плавающей запятой. После выполнения приведённого выше кода два эмбеддинга будут выведены на экран:

```
`[-0.013131560757756233, 0.019828535616397858, ...]   # embedding for "Sample text 1"
[-0.0069352793507277966, 0.020878976210951805, ...]  # embedding for "Sample text 2"`
```

При создании эмбеддингов вы можете указать несколько других аргументов для функции `embed()`.

Дополнительную информацию о Python-пакете Voyage см. в [документации Voyage](https://docs.voyageai.com/docs/embeddings#python-api).

### HTTP API Voyage

Вы также можете получить эмбеддинги, отправив запрос к HTTP API Voyage. Например, вы можете отправить HTTP-запрос через команду `curl` в терминале:

cURL
```
`curl https://api.voyageai.com/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VOYAGE_API_KEY" \
  -d '{
    "input": ["Sample text 1", "Sample text 2"],
    "model": "voyage-4"
  }'`
```

В ответ вы получите JSON-объект, содержащий эмбеддинги и информацию об использовании токенов:

```
`{
  "object": "list",
  "data": [
    {
      "embedding": [-0.013131560757756233, 0.019828535616397858 /* ... */],
      "index": 0
    },
    {
      "embedding": [-0.0069352793507277966, 0.020878976210951805 /* ... */],
      "index": 1
    }
  ],
  "model": "voyage-4",
  "usage": {
    "total_tokens": 10
  }
}`
```

Дополнительную информацию о HTTP API Voyage см. в [документации Voyage](https://docs.voyageai.com/reference/embeddings-api).

### AWS Marketplace

Эмбеддинги Voyage доступны на [AWS Marketplace](https://aws.amazon.com/marketplace/seller-profile?id=seller-snt4gb6fd7ljg). Инструкции по доступу к Voyage на AWS доступны в [документации Voyage по AWS Marketplace](https://docs.voyageai.com/docs/aws-marketplace-model-package?ref=anthropic).

## Пример быстрого старта

Следующий краткий пример показывает, как использовать эмбеддинги.

Предположим, у вас есть небольшой корпус из шести документов для поиска

```
`documents = [
    "The Mediterranean diet emphasizes fish, olive oil, and vegetables, believed to reduce chronic diseases.",
    "Photosynthesis in plants converts light energy into glucose and produces essential oxygen.",
    "20th-century innovations, from radios to smartphones, centered on electronic advancements.",
    "Rivers provide water, irrigation, and habitat for aquatic species, vital for ecosystems.",
    "Apple's conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.",
    "Shakespeare's works, like 'Hamlet' and 'A Midsummer Night's Dream,' endure in literature.",
]`
```

Сначала используйте Voyage для преобразования каждого документа в вектор эмбеддинга

```
`import voyageai

vo = voyageai.Client()

# Создаём эмбеддинги для документов
doc_embds = vo.embed(documents, model="voyage-4", input_type="document").embeddings`
```

Эмбеддинги позволяют выполнять семантический поиск / извлечение в векторном пространстве. Для примера запроса

```
`query = "When is Apple's conference call scheduled?"`
```

Далее преобразуйте его в эмбеддинг и выполните поиск ближайшего соседа, чтобы найти наиболее релевантный документ на основе расстояния в пространстве эмбеддингов.

```
`import numpy as np

# Создаём эмбеддинг запроса
query_embd = vo.embed([query], model="voyage-4", input_type="query").embeddings[0]

# Вычисляем сходство
# Эмбеддинги Voyage нормализованы до длины 1, поэтому скалярное произведение
# и косинусное сходство совпадают.
similarities = np.dot(doc_embds, query_embd)

retrieved_id = np.argmax(similarities)
print(documents[retrieved_id])`
```

Обратите внимание, что `input_type="document"` и `input_type="query"` используются для создания эмбеддингов документа и запроса соответственно. Более подробную спецификацию можно найти в разделе [Python-библиотека Voyage](/docs/ru/build-with-claude/embeddings#voyage-python-library).

Результатом будет 5-й документ, который действительно наиболее релевантен запросу:

```
`Apple's conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.`
```

Если вы ищете подробный набор руководств о том, как реализовать RAG с эмбеддингами, включая векторные базы данных, ознакомьтесь с [руководством по RAG](https://platform.claude.com/cookbook/third-party-pinecone-rag-using-pinecone).

## FAQ

### 

### 

### 

### 

### 

### 

### 

## Цены

Посетите [страницу цен](https://docs.voyageai.com/docs/pricing?ref=anthropic) Voyage для получения актуальной информации о ценах.
Was this page helpful?