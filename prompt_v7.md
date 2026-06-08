# Структурированный промпт V7

## Назначение промпта

Седьмая версия промпта используется для финальной проверки backend-реализации, контрактов API и соответствия исходным требованиям.

## REASONS Canvas

### Requirements

- Проверить, что все роутеры подключены в `main.py`.
- Проверить, что endpoint-ы совпадают с README и frontend.
- Проверить, что Bearer API key используется для B2B endpoint.
- Проверить, что legacy `/api/moderate` сохранен.
- Проверить, что `store.touch_key` и `RequestLog` вызываются после успешного анализа.
- Проверить, что ключи и тарифы работают по лимитам.
- Проверить, что ошибки не запускают лишний инференс.

### Entities

- `RouteRegistry`
- `APIContract`
- `AuthorizationDependency`
- `RateLimit`
- `RequestLog`
- `LegacyCompatibility`
- `RegressionRisk`

### Approach

- Выполнить финальный проход по backend-файлам.
- Исправлять только несоответствия контракта и мелкие дефекты.
- Не добавлять новые большие функции.
- Сравнить итог с требованиями и тестовыми сценариями.

### Structure

- Проверить:
  - `backend/main.py`;
  - `backend/deps.py`;
  - `backend/auth.py`;
  - `backend/store.py`;
  - `backend/plans.py`;
  - `backend/routes/*.py`;
  - `backend/services/*.py`.

### Operations

- Проверить URL endpoint-ов.
- Проверить типы авторизации.
- Проверить формат ответов.
- Проверить обработку пустого текста.
- Проверить лимит ключей и запросов.
- Проверить dashboard-поля.
- Проверить legacy-ответ.

### Norms

- Финальные исправления должны быть небольшими.
- Не менять веса моделей.
- Не переносить ML-логику в роутеры.
- Сохранить русскоязычные сообщения ошибок.

### Safeguards

- Не удалять старые endpoint-ы.
- Не раскрывать полный ключ в списках.
- Не ломать frontend-контракты camelCase.
- Не выдавать in-memory store за production-БД.

## Текст промпта

Проведи финальную проверку backend TextGuard AI: `main.py`, `deps.py`, `auth.py`, `store.py`, `plans.py`, `user_store.py`, все файлы `routes/` и сервисы `classifier_service.py`, `detox_service.py`. Проверь URL endpoint-ов, авторизацию Bearer/X-API-Key/X-Admin-Token, форматы JSON, обработку пустого текста, лимиты тарифов, dashboard, legacy-совместимость и журналирование. Исправляй только backend-дефекты, не изменяй frontend, обучение моделей и локальные веса.
