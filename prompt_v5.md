# Структурированный промпт V5

## Назначение промпта

Промпт используется для сохранения обратной совместимости и административных операций.

## REASONS Canvas

### Requirements

- Сохранить старый endpoint `POST /api/moderate`.
- Использовать для него заголовок `X-API-Key`.
- Вернуть старый формат ответа: `original_text`, `is_toxic`, `scores`, `detoxified_text`, `action_taken`.
- Добавить admin endpoint `/api/admin/keys`.
- Защитить admin endpoint заголовком `X-Admin-Token`.
- Добавить mock-auth для кабинета: login, me, logout.

### Entities

- `ModerateRequest`
- `ModerateResponse`
- `require_api_key`
- `require_admin`
- `AdminCreateKeyRequest`
- `LoginRequest`
- `UserStore`

### Approach

- Legacy endpoint не удалять и не переводить на camelCase.
- Admin endpoint отделить от обычных API-key операций.
- Mock-auth держать отдельно от API-key авторизации.
- Объяснить в коде, что это демонстрационная реализация.

### Structure

- `auth.py`
- `routes/moderation.py`
- `routes/keys.py`
- `user_store.py`
- `routes/auth.py`

### Operations

- `POST /api/moderate`
- `POST /api/admin/keys`
- `GET /api/admin/keys`
- `DELETE /api/admin/keys/{key}`
- `POST /api/auth/login`
- `GET /api/auth/me`
- `POST /api/auth/logout`

### Norms

- Не ломать старый frontend или старые curl-примеры.
- Не смешивать session token и API key.
- Не делать настоящую регистрацию пользователей в лабораторной работе.

### Safeguards

- Невалидный `X-API-Key` должен отклоняться.
- Невалидный `X-Admin-Token` должен отклоняться.
- Ошибка login не должна создавать сессию.

## Текст промпта

Сохрани legacy endpoint `/api/moderate` для TextGuard AI с заголовком `X-API-Key` и старым форматом ответа. Добавь admin-операции `/api/admin/keys`, защищенные `X-Admin-Token`, и mock-auth `/api/auth/login`, `/api/auth/me`, `/api/auth/logout` на основе простого `user_store.py`. Не смешивай session bearer token с API key для B2B-анализов.
