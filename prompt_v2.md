# Структурированный промпт V2

## Назначение промпта

Промпт используется для проектирования API-ключей, тарифов и демонстрационного in-memory-хранилища.

## REASONS Canvas

### Requirements

- Реализовать хранение workspace, API-ключей и последних запросов.
- Реализовать тарифные планы Free, Pro и Business.
- Проверять лимит запросов перед запуском модели.
- Ограничивать количество ключей по тарифу.
- Хранить только демонстрационное состояние в памяти процесса.

### Entities

- `Workspace`
- `APIKeyRecord`
- `RequestLog`
- `Store`
- `Plan`
- `requests_count`
- `api_keys_limit`

### Approach

- Использовать `dataclasses` для простых структур.
- Использовать `threading.Lock` для потокобезопасного изменения словарей.
- Генерировать ключи формата `textguard_sk_...`.
- Возвращать наружу `key_preview`, а полный ключ показывать только при создании.

### Structure

- `store.py`:
  - `Workspace`;
  - `APIKeyRecord`;
  - `RequestLog`;
  - `Store`;
  - singleton `store`.
- `plans.py`:
  - словарь `PLANS`;
  - функция `get_plan`.

### Operations

- `get_or_create_demo_workspace()`
- `create_key(workspace_id, name)`
- `list_keys(workspace_id)`
- `revoke_key(key)`
- `touch_key(key)`
- `add_log(log)`
- `get_logs(workspace_id, limit)`
- `set_plan(ws_id, plan)`

### Norms

- Не хранить чувствительные данные в логах полностью.
- Не использовать настоящую платежную систему в лабораторной работе.
- Не усложнять demo-store базой данных.
- Четко указать, что in-memory store является ограничением.

### Safeguards

- При неизвестном ключе возвращать ошибку авторизации.
- При превышении лимита возвращать `429 Too Many Requests`.
- При попытке удалить чужой ключ возвращать `403`.

## Текст промпта

Сгенерируй backend-модули `store.py` и `plans.py` для демонстрационного B2B API TextGuard AI. Нужны `Workspace`, `APIKeyRecord`, `RequestLog`, потокобезопасный `Store`, генерация ключей `textguard_sk_...`, счетчик запросов, журнал последних запросов, тарифы Free/Pro/Business с лимитами запросов и количеством ключей. Реализация должна быть простой, in-memory, без базы данных и без настоящего биллинга.
