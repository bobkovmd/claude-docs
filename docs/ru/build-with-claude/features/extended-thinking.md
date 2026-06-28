# Расширенное мышление (Extended Thinking)

## Обзор

Расширенное мышление даёт Claude улучшенные возможности рассуждения за счёт генерации внутренних блоков контента `thinking` перед выдачей финального ответа. Доступно на всех текущих моделях Claude, хотя метод включения варьируется.

## Матрица поддержки моделей

| Модель | Ручная настройка (`budget_tokens`) | Рекомендуемый подход |
|---|---|---|
| Claude Fable 5 / Mythos 5 | Не поддерживается | Адаптивное мышление (всегда включено) |
| Claude Opus 4.8 / 4.7 | Не поддерживается | Адаптивное мышление с `effort` |
| Claude Opus 4.6 / Sonnet 4.6 | Устаревший | Адаптивное мышление с `effort` |
| Claude Opus 4.5 / Haiku 4.5 | Поддерживается | N/A |

## Как это работает

```json
{
  "content": [
    {"type": "thinking", "thinking": "Давайте проанализирую...", "signature": "..."},
    {"type": "text", "text": "На основе моего анализа..."}
  ]
}
```

## Включение расширенного мышления

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    thinking={"type": "enabled", "budget_tokens": 10000},
    messages=[{"role": "user", "content": "..."}],
)
```

### Ключевые параметры
- **`type`**: `"enabled"` или `"adaptive"`
- **`budget_tokens`**: Максимум токенов для внутреннего рассуждения (минимум 1,024)
- **`display`**: `"summarized"` или `"omitted"`

## Адаптивное мышление (Claude Opus 4.8+)

- По умолчанию мышление ВЫКЛЮЧЕНО
- Глубина контролируется через параметр `effort`
- При `max`/`xhigh` effort установите большой максимальный выход (начните с 64k токенов)

## Потоковое мышление (Streaming Thinking)

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    thinking={"type": "enabled", "budget_tokens": 10000},
    messages=[...],
) as stream:
    for event in stream:
        if event.type == "thinking_delta":
            print(event.thinking_delta, end="")
```
