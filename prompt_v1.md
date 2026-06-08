# Структурированный промпт V1

## Назначение промпта

Промпт используется для проектирования структуры B2B API TextGuard AI на FastAPI.

## REASONS Canvas

### Requirements

- Спроектировать новые B2B endpoint-ы поверх существующего ML-ядра.
- Оставить старый endpoint `/api/moderate` для совместимости.
- Разделить маршруты по файлам в `backend/routes/`.
- Подключить все роутеры в `backend/main.py`.
- Настроить CORS для локального frontend.

### Entities

- `FastAPI`
- `APIRouter`
- `B2B moderation endpoint`
- `Legacy moderation endpoint`
- `Subscription endpoint`
- `API key endpoint`
- `Dashboard endpoint`
- `Auth endpoint`

### Approach

- Использовать отдельные роутеры вместо одного большого `main.py`.
- Основной B2B-анализ вынести в `/api/v1/moderation/analyze`.
- Служебные операции разделить на `/api/api-keys`, `/api/subscriptions`, `/api/dashboard`, `/api/auth`, `/api/admin/keys`.
- Корневой endpoint `/` использовать как краткую карту API.

### Structure

- `main.py`:
  - создать `FastAPI(title=...)`;
  - подключить `CORSMiddleware`;
  - подключить роутеры.
- `routes/v1_moderation.py`:
  - B2B endpoint анализа.
- `routes/moderation.py`:
  - legacy endpoint.
- `routes/api_keys.py`, `subscriptions.py`, `dashboard.py`, `auth.py`, `keys.py`:
  - продуктовая обвязка.

### Operations

- Определить URL каждого endpoint.
- Определить метод HTTP.
- Определить тип авторизации.
- Определить краткий JSON-ответ.

### Norms

- Не смешивать код роутеров с кодом ML-сервисов.
- Не дублировать бизнес-логику в `main.py`.
- Использовать понятные `tags` для OpenAPI.
- Не менять существующие классы моделей без причины.

### Safeguards

- Не удалять `/api/moderate`.
- Не открывать B2B-анализ без ключа.
- Не использовать CORS `*` для демонстрационного frontend, если известны локальные порты.

## Текст промпта

Спроектируй backend API TextGuard AI на FastAPI. Нужно создать структуру роутеров: `/api/v1/moderation/analyze` для нового B2B-анализа, `/api/moderate` для legacy-совместимости, `/api/api-keys` для ключей, `/api/subscriptions` для тарифов, `/api/dashboard` для кабинета, `/api/auth` для mock-login и `/api/admin/keys` для admin-операций. Подключи роутеры в `main.py`, настрой CORS для локального frontend и не изменяй ML-сервисы.
