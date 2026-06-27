---
title: "Обзор функций"
url: "https://docs.claude.com/ru/build-with-claude/overview"
lang: "ru"
---

Сообщения/Разработка с Claude
# Обзор функций
Изучите расширенные функции и возможности Claude.Поверхность API Claude организована в пять областей:

- **Возможности модели:** управляйте тем, как Claude рассуждает и форматирует ответы.
- **Инструменты:** позволяйте Claude выполнять действия в интернете или в вашей среде.
- **Инфраструктура инструментов:** обеспечивает обнаружение и оркестрацию в масштабе.
- **Управление контекстом:** поддерживает эффективность длительных сессий.
- **Файлы и ресурсы:** управляйте документами и данными, которые вы предоставляете Claude.

Если вы новичок, начните с разделов [возможности модели](#model-capabilities) и [инструменты](#tools). Вернитесь к другим разделам, когда будете готовы оптимизировать стоимость, задержку или масштаб.

Для администрирования и управления см. [Admin API](/docs/ru/manage-claude/admin-api), [Usage and Cost API](/docs/ru/manage-claude/usage-cost-api) и [Compliance API](/docs/ru/manage-claude/compliance-api).

## Доступность функций

Функциям на Claude Platform присваивается одна из следующих классификаций доступности для каждой платформы (показана в столбце «Доступность» в каждой из следующих таблиц). Не все функции проходят через каждую стадию. Функция может появиться на любой стадии классификации и может пропускать стадии.

КлассификацияОписание**Бета***Предварительные функции, используемые для сбора обратной связи и итерации над менее зрелым сценарием использования. Доступность может быть ограничена, в том числе через требования регистрации или списки ожидания, и может не объявляться публично. 

 Функции могут значительно измениться или быть прекращены на основе обратной связи. Не гарантируются для постоянного использования в продакшене. Возможны критические изменения с уведомлением, и могут применяться некоторые ограничения, специфичные для платформы. Бета-функции в Claude API и [Claude Platform на AWS](/docs/ru/build-with-claude/claude-platform-on-aws) имеют [бета-заголовок](/docs/ru/api/beta-headers).**Общедоступно (GA)**Функция стабильна, полностью поддерживается и рекомендуется для использования в продакшене. Не должна иметь бета-заголовка или другого индикатора того, что функция находится в состоянии предварительного просмотра. Покрывается стандартными гарантиями [версионирования](/docs/ru/api/versioning) API.**Устаревшая**Функция всё ещё работает, но больше не рекомендуется. Предоставляется путь миграции и график удаления.**Выведена из эксплуатации**Функция больше недоступна.
** Может содержать уточнение, указывающее на более узкую доступность или дополнительные ограничения (например, «бета: исследовательская предварительная версия»). Подробности см. на странице функции.*

**Обозначения платформ:** Claude API (собственная платформа Anthropic) · [Claude Platform на AWS](/docs/ru/build-with-claude/claude-platform-on-aws) (управляется Anthropic на AWS) · [Bedrock](/docs/ru/build-with-claude/claude-in-amazon-bedrock) (управляется AWS) · [Vertex AI](/docs/ru/build-with-claude/claude-on-vertex-ai) (управляется Google) · [Microsoft Foundry](/docs/ru/build-with-claude/claude-in-microsoft-foundry) (управляется Anthropic на Azure)

## Возможности модели

Способы управления Claude и его непосредственными выходными данными, включая формат ответа, глубину рассуждений и модальности ввода.

Вы можете программно узнать, какие возможности поддерживает модель. [Models API](/docs/ru/api/models/list) возвращает `max_input_tokens`, `max_tokens` и объект `capabilities` для каждой доступной модели.

Столбец ZDR указывает, доступна ли функция в рамках соглашения о нулевом хранении данных — «Zero Data Retention» (нулевое хранение данных). Для большинства функций это зависит только от того, что сохраняет механизм функции; для функций, привязанных к конкретным моделям, также применяется доступность ZDR на уровне модели. См. [Требования к хранению данных для конкретных моделей](/docs/ru/manage-claude/api-and-data-retention#model-specific-data-retention-requirements).

ФункцияОписаниеZero Data Retention (ZDR)Доступность[Контекстные окна](/docs/ru/build-with-claude/context-windows)До 1 млн токенов для обработки больших документов, обширных кодовых баз и длинных разговоров.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Адаптивное мышление](/docs/ru/build-with-claude/adaptive-thinking)Позвольте Claude динамически решать, когда и сколько думать. Единственный режим мышления в Claude Opus 4.8 и Claude Opus 4.7. Используйте параметр effort для управления глубиной мышления.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Пакетная обработка](/docs/ru/build-with-claude/batch-processing)Обрабатывайте большие объёмы запросов асинхронно для экономии средств. Отправляйте пакеты с большим количеством запросов в каждом. Вызовы Batch API стоят на 50% меньше, чем стандартные вызовы API.Несовместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)[Цитирование](/docs/ru/build-with-claude/citations)Обосновывайте ответы Claude исходными документами. С помощью цитирования Claude может предоставлять подробные ссылки на точные предложения и фрагменты, которые он использует для генерации ответов, что приводит к более проверяемым и достоверным результатам.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Резидентность данных](/docs/ru/manage-claude/data-residency)Контролируйте, где выполняется инференс модели, с помощью географических настроек. Указывайте маршрутизацию `"global"` или `"us"` для каждого запроса через параметр `inference_geo`.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)[Effort](/docs/ru/build-with-claude/effort)Контролируйте, сколько токенов Claude использует при ответе, с помощью параметра effort, балансируя между полнотой ответа и эффективностью использования токенов.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Расширенное мышление](/docs/ru/build-with-claude/extended-thinking)Улучшенные возможности рассуждения для сложных задач, обеспечивающие прозрачность пошагового процесса мышления Claude перед выдачей окончательного ответа.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Резервный кредит](/docs/ru/build-with-claude/fallback-credit)Избегайте двойной оплаты стоимости кэша подсказок при повторной попытке отклонённого запроса на другой модели. Отказ содержит кредитный токен, и его передача при повторной попытке приводит к тарификации повторного запроса так, как если бы разговор изначально вёлся на новой модели. Кредитные токены, возвращённые в результатах Message Batches, не могут быть использованы.Несовместимо с ZDR*Claude API (Beta)

Claude Platform on AWS (Beta)

Bedrock (Beta)

Google Cloud (Beta)

Microsoft Foundry (Beta)[Поддержка PDF](/docs/ru/build-with-claude/pdf-support)Обрабатывайте и анализируйте текстовое и визуальное содержимое из PDF-документов.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Результаты поиска](/docs/ru/build-with-claude/search-results)Обеспечьте естественное цитирование для RAG-приложений, предоставляя результаты поиска с надлежащей атрибуцией источников. Достигайте качества цитирования уровня веб-поиска для пользовательских баз знаний и инструментов.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Серверный резервный вариант](/docs/ru/build-with-claude/refusals-and-fallback)Повторяйте отклонённый запрос внутри одного вызова API. Укажите до трёх резервных моделей, и когда запрошенная модель отклоняет запрос, API запускает следующую модель в цепочке с тем же запросом. Параметр `fallbacks` недоступен в Message Batches API.Несовместимо с ZDR*Claude API (Beta)

Claude Platform on AWS (Beta)[Структурированные выходные данные](/docs/ru/build-with-claude/structured-outputs)Гарантируйте соответствие схеме с помощью двух подходов: JSON-вывод для структурированных ответов с данными и строгое использование инструментов для валидированных входных данных инструментов.[Совместимо с ZDR (с оговорками)](/docs/ru/build-with-claude/structured-outputs#data-retention)*Claude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)

## Инструменты

Встроенные инструменты, которые Claude вызывает через `tool_use`. Серверные инструменты выполняются платформой; клиентские инструменты реализуются и выполняются вами.

### Серверные инструменты

ФункцияОписаниеZDRДоступность[Инструмент советника](/docs/ru/agents-and-tools/tool-use/advisor-tool)Объедините более быструю модель-исполнителя с более интеллектуальной моделью-советником, которая предоставляет стратегические рекомендации в процессе генерации для долгосрочных агентных рабочих нагрузок.Совместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)[Выполнение кода](/docs/ru/agents-and-tools/tool-use/code-execution-tool)Запускайте код в изолированной среде для расширенного анализа данных, вычислений и обработки файлов. Бесплатно при использовании с веб-поиском или веб-загрузкой.Несовместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Microsoft Foundry (Beta)[Веб-загрузка](/docs/ru/agents-and-tools/tool-use/web-fetch-tool)Получайте полное содержимое с указанных веб-страниц и PDF-документов для углублённого анализа.Совместимо с ZDR*Claude API (GA)

Claude Platform on AWS (GA)

Microsoft Foundry (Beta)[Веб-поиск](/docs/ru/agents-and-tools/tool-use/web-search-tool)Дополняйте обширные знания Claude актуальными данными из реального мира со всего интернета.Совместимо с ZDR*Claude API (GA)

Claude Platform on AWS (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)

### Клиентские инструменты

ФункцияОписаниеZDRДоступность[Bash](/docs/ru/agents-and-tools/tool-use/bash-tool)Выполняйте команды и скрипты bash для взаимодействия с системной оболочкой и выполнения операций командной строки.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Использование компьютера](/docs/ru/agents-and-tools/tool-use/computer-use-tool)Управляйте компьютерными интерфейсами, делая снимки экрана и отправляя команды мыши и клавиатуры.Совместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Bedrock (Beta)

Google Cloud (Beta)

Microsoft Foundry (Beta)[Память](/docs/ru/agents-and-tools/tool-use/memory-tool)Позвольте Claude сохранять и извлекать информацию между разговорами. Создавайте базы знаний со временем, поддерживайте контекст проекта и учитесь на прошлых взаимодействиях.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Текстовый редактор](/docs/ru/agents-and-tools/tool-use/text-editor-tool)Создавайте и редактируйте текстовые файлы с помощью встроенного интерфейса текстового редактора для задач манипулирования файлами.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)

## Инфраструктура инструментов

Инфраструктура, поддерживающая обнаружение, оркестрацию и масштабирование использования инструментов.

ФункцияОписаниеZDRДоступность[Agent Skills](/docs/ru/agents-and-tools/agent-skills/overview)Расширяйте возможности Claude с помощью навыков (Skills). Используйте готовые навыки (PowerPoint, Excel, Word, PDF) или создавайте собственные навыки с инструкциями и скриптами. Навыки используют прогрессивное раскрытие для эффективного управления контекстом.Несовместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Microsoft Foundry (Beta)[Детализированная потоковая передача инструментов](/docs/ru/agents-and-tools/tool-use/fine-grained-tool-streaming)Передавайте параметры использования инструментов потоком без буферизации/валидации JSON, снижая задержку при получении больших параметров.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (GA)[Коннектор MCP](/docs/ru/agents-and-tools/mcp-connector)Подключайтесь к удалённым серверам [MCP](/docs/ru/mcp) напрямую из Messages API без отдельного клиента MCP.Несовместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Microsoft Foundry (Beta)[Программный вызов инструментов](/docs/ru/agents-and-tools/tool-use/programmatic-tool-calling)Позвольте Claude вызывать ваши инструменты программно изнутри контейнеров выполнения кода, снижая задержку и потребление токенов для рабочих процессов с несколькими инструментами.Несовместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Microsoft Foundry (Beta)[Поиск инструментов](/docs/ru/agents-and-tools/tool-use/tool-search-tool)Масштабируйтесь до тысяч инструментов, динамически обнаруживая и загружая инструменты по требованию с помощью поиска на основе регулярных выражений, оптимизируя использование контекста и повышая точность выбора инструментов.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)

## Управление контекстом

Инфраструктура для контроля и оптимизации контекстного окна Claude.

ФункцияОписаниеZDRДоступность[Уплотнение](/docs/ru/build-with-claude/compaction)Серверное суммирование контекста для длительных разговоров. Когда контекст приближается к лимиту окна, API автоматически суммирует более ранние части разговора.Совместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Bedrock (Beta)

Google Cloud (Beta)

Microsoft Foundry (Beta)[Редактирование контекста](/docs/ru/build-with-claude/context-editing)Автоматически управляйте контекстом разговора с помощью настраиваемых стратегий. Поддерживает очистку результатов инструментов при приближении к лимитам токенов и управление блоками мышления в разговорах с расширенным мышлением.Совместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Bedrock (Beta)

Google Cloud (Beta)

Microsoft Foundry (Beta)[Автоматическое кэширование подсказок](/docs/ru/build-with-claude/prompt-caching#automatic-caching)Упростите кэширование подсказок до одного параметра API. Система автоматически кэширует последний кэшируемый блок в вашем запросе, перемещая точку кэша вперёд по мере роста разговоров.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Microsoft Foundry (Beta)[Кэширование подсказок (5 мин)](/docs/ru/build-with-claude/prompt-caching)Предоставляйте Claude больше фоновых знаний и примеров выходных данных для снижения затрат и задержки.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Кэширование подсказок (1 ч)](/docs/ru/build-with-claude/prompt-caching#1-hour-cache-duration)Расширенная длительность кэша в 1 час для менее часто используемого, но важного контекста, дополняющая стандартный 5-минутный кэш.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)[Подсчёт токенов](/docs/ru/build-with-claude/token-counting)Подсчёт токенов позволяет вам определить количество токенов в сообщении перед его отправкой в Claude, помогая принимать обоснованные решения о ваших подсказках и использовании.Совместимо с ZDRClaude API (GA)

Claude Platform on AWS (GA)

Bedrock (GA)

Google Cloud (GA)

Microsoft Foundry (Beta)

## Файлы и ресурсы

Управляйте файлами и ресурсами для использования с Claude.

ФункцияОписаниеZDRДоступность[Files API](/docs/ru/build-with-claude/files)Загружайте файлы и управляйте ими для использования с Claude без повторной загрузки содержимого с каждым запросом. Поддерживает PDF, изображения и текстовые файлы.Несовместимо с ZDRClaude API (Beta)

Claude Platform on AWS (Beta)

Microsoft Foundry (Beta)
* **Структурированные выходные данные:** ваши подсказки и выходные данные Claude не сохраняются. Кэшируются только JSON-схемы, на срок до 24 часов с момента последнего использования. **Веб-поиск и веб-загрузка:** совместимы с ZDR, за исключением случаев, когда включена [динамическая фильтрация](/docs/ru/agents-and-tools/tool-use/web-search-tool#dynamic-filtering). **Резервный кредит и серверный резервный вариант:** эти функции не сохраняют содержимое сообщений, но обе обрабатывают отказы от Claude Fable 5, которая [недоступна в рамках ZDR](/docs/ru/manage-claude/api-and-data-retention#model-specific-data-retention-requirements). См. [подробности о ZDR](/docs/ru/manage-claude/api-and-data-retention#feature-eligibility).
Was this page helpful?