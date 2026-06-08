# Структурированный промпт V4

## Назначение промпта

Промпт используется для реализации endpoint-ов управления API-ключами, тарифами и dashboard.

## REASONS Canvas

### Requirements

- Создать маршруты для генерации, просмотра и удаления ключей.
- Создать маршруты для списка тарифов и выбора тарифа.
- Создать dashboard endpoint с usage, ключами и recent_requests.
- Использовать `require_bearer` для защищенных операций.
- Учитывать `api_keys_limit` при генерации ключа.

### Entities

- `GenerateKeyRequest`
- `SelectPlanRequest`
- `DashboardData`
- `APIKeyRecord`
- `Plan`
- `UsageInfo`
- `RequestLog`

### Approach

- Разделить ключи, тарифы и dashboard по разным файлам.
- Для списка ключей отдавать публичные данные и preview.
- Для dashboard агрегировать usage по ключам workspace.
- Для тарифов возвращать данные из `PLANS`.

### Structure

- `routes/api_keys.py`
- `routes/subscriptions.py`
- `routes/dashboard.py`

### Operations

- `POST /api/api-keys/generate`
- `GET /api/api-keys`
- `DELETE /api/api-keys/{key}`
- `GET /api/subscriptions/plans`
- `POST /api/subscriptions/select`
- `GET /api/dashboard`

### Norms

- Не показывать чужие ключи.
- Не создавать ключ сверх лимита тарифа.
- Не смешивать dashboard с логикой ML-инференса.
- Возвращать JSON, удобный для frontend.

### Safeguards

- При неизвестном тарифе возвращать HTTP 400.
- При чужом ключе возвращать HTTP 403.
- При отсутствии ключа rely on `require_bearer`.

## Текст промпта

Реализуй продуктовые endpoint-ы TextGuard AI: `routes/api_keys.py`, `routes/subscriptions.py`, `routes/dashboard.py`. Нужны операции генерации, списка и удаления API-ключей, список тарифов Free/Pro/Business, выбор тарифа и dashboard со сведениями workspace, current_key, usage, api_keys и recent_requests. Все защищенные операции должны использовать `require_bearer`, учитывать лимиты тарифа и не раскрывать чужие ключи.
