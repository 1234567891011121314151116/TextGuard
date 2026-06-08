# Структурированный промпт V3

## Назначение промпта

Промпт используется для реализации основного B2B endpoint анализа текста.

## REASONS Canvas

### Requirements

- Создать endpoint `POST /api/v1/moderation/analyze`.
- Принимать JSON с полем `text`.
- Проверять Bearer API key через зависимость.
- Вызывать `ClassifierService.predict(text)`.
- При токсичности вызывать `DetoxService.detoxify(text)`.
- Возвращать camelCase JSON для frontend.
- Добавлять запись в `RequestLog`.

### Entities

- `AnalyzeRequest`
- `AnalyzeResponse`
- `ClassifierService`
- `DetoxService`
- `RequestLog`
- `Bearer API key`
- `scores`
- `isToxic`

### Approach

- Создать экземпляры ML-сервисов на уровне модуля, чтобы не загружать модели на каждый запрос.
- Пустой текст отклонять до инференса.
- Для безопасного текста возвращать `status="approved"`.
- Для токсичного текста возвращать `status="cleaned"` и `cleanedText`.

### Structure

- `routes/v1_moderation.py`:
  - router prefix `/api/v1/moderation`;
  - Pydantic-схемы;
  - endpoint `/analyze`;
  - вызов `store.touch_key`;
  - вызов `store.add_log`.

### Operations

- Извлечь `ctx` из `Depends(require_bearer)`.
- Проверить `req.text.strip()`.
- Получить `scores, is_toxic`.
- Сформировать `cleaned`.
- Обновить счетчик ключа.
- Вернуть `AnalyzeResponse`.

### Norms

- Не возвращать stack trace пользователю.
- Не менять формат `ClassifierService.predict`.
- Не блокировать безопасный текст без причины.
- Использовать понятные имена полей для TypeScript frontend.

### Safeguards

- Не запускать `DetoxService`, если `is_toxic=False`.
- Не увеличивать счетчик при ошибке авторизации.
- В журнал писать только короткий preview текста.

## Текст промпта

Реализуй `routes/v1_moderation.py` для TextGuard AI. Нужен `POST /api/v1/moderation/analyze` с Bearer API key через `require_bearer`. Схема запроса: `{ "text": "..." }`. Ответ: `originalText`, `cleanedText`, `status`, `isToxic`, `scores`. Используй `ClassifierService.predict()`, при токсичности вызывай `DetoxService.detoxify()`, обновляй счетчик ключа и добавляй `RequestLog`. Пустой текст должен давать HTTP 400.
