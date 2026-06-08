# Тестовые сценарии для лабораторной работы №8

## 1. Проверка основного B2B endpoint

### 1.1 Безопасный текст

Вход: `POST /api/v1/moderation/analyze` с Bearer API key и телом `{ "text": "Добрый день, предлагаю обсудить задачу" }`.

Ожидаемый результат:

- HTTP 200;
- в ответе есть `originalText`, `cleanedText`, `status`, `isToxic`, `scores`;
- при безопасном тексте `status = "approved"`;
- `cleanedText = null`;
- счетчик `requests_count` увеличивается.

### 1.2 Токсичный текст

Вход: `POST /api/v1/moderation/analyze` с Bearer API key и грубым сообщением.

Ожидаемый результат:

- HTTP 200;
- `isToxic = true`;
- `status = "cleaned"`;
- `cleanedText` содержит результат детоксикации;
- в `scores` есть `toxic`, `severe_toxic`, `obscene`, `threat`, `insult`, `identity_hate`;
- в dashboard появляется запись `recent_requests`.

### 1.3 Пустой текст

Вход: `{ "text": "   " }`.

Ожидаемый результат:

- HTTP 400;
- модель не должна запускаться;
- счетчик ключа не должен увеличиваться.

## 2. Проверка авторизации

### 2.1 Запрос без Bearer

Вход: запрос к `/api/v1/moderation/analyze` без заголовка `Authorization`.

Ожидаемый результат:

- HTTP 401;
- сообщение о необходимости `Authorization: Bearer <API_KEY>`.

### 2.2 Невалидный API-ключ

Вход: `Authorization: Bearer wrong_key`.

Ожидаемый результат:

- HTTP 401;
- инференс не выполняется.

### 2.3 Превышение лимита тарифа

Предусловие: `requests_count` ключа равен лимиту тарифа.

Ожидаемый результат:

- HTTP 429;
- сообщение о превышении лимита тарифа.

## 3. Проверка API-ключей

### 3.1 Генерация ключа

Вход: `POST /api/api-keys/generate` с Bearer API key.

Ожидаемый результат:

- HTTP 200;
- ответ содержит `id`, `name`, `key`, `created_at`;
- ключ имеет формат `textguard_sk_...`.

### 3.2 Список ключей

Вход: `GET /api/api-keys`.

Ожидаемый результат:

- HTTP 200;
- возвращается массив `keys`;
- у ключа есть `key_preview`;
- полный ключ в списке не раскрывается.

### 3.3 Превышение лимита ключей

Предусловие: тариф Free и уже создан 1 ключ.

Ожидаемый результат:

- повторное создание ключа возвращает HTTP 403.

### 3.4 Удаление ключа

Вход: `DELETE /api/api-keys/{key}`.

Ожидаемый результат:

- HTTP 200;
- `{ "revoked": true }`;
- удаленный ключ больше не проходит авторизацию.

## 4. Проверка тарифов

### 4.1 Получение списка тарифов

Вход: `GET /api/subscriptions/plans`.

Ожидаемый результат:

- HTTP 200;
- есть планы `free`, `pro`, `business`;
- у каждого плана есть `request_limit` и `api_keys_limit`.

### 4.2 Выбор тарифа

Вход: `POST /api/subscriptions/select` с `{ "plan": "pro" }`.

Ожидаемый результат:

- HTTP 200;
- workspace получает тариф `pro`;
- dashboard показывает новый тариф.

### 4.3 Неизвестный тариф

Вход: `{ "plan": "enterprise_fake" }`.

Ожидаемый результат:

- HTTP 400.

## 5. Проверка dashboard

Вход: `GET /api/dashboard`.

Ожидаемый результат:

- HTTP 200;
- ответ содержит `workspace`, `plan`, `current_key`, `usage`, `api_keys`, `recent_requests`;
- `usage.used` соответствует сумме запросов по ключам workspace;
- `usage.remaining` не уходит в отрицательное значение.

## 6. Проверка legacy endpoint

### 6.1 Успешный legacy-запрос

Вход: `POST /api/moderate` с заголовком `X-API-Key`.

Ожидаемый результат:

- HTTP 200;
- ответ содержит `original_text`, `is_toxic`, `scores`, `detoxified_text`, `action_taken`;
- формат ответа остается snake_case.

### 6.2 Невалидный X-API-Key

Ожидаемый результат:

- HTTP 403;
- запрос не проходит к модели.

## 7. Проверка admin endpoint

### 7.1 Создание ключа администратором

Вход: `POST /api/admin/keys` с `X-Admin-Token`.

Ожидаемый результат:

- HTTP 200;
- создается API-ключ для указанного workspace.

### 7.2 Нет admin-token

Ожидаемый результат:

- HTTP 401.

## 8. Проверка mock-аутентификации

### 8.1 Login

Вход: `POST /api/auth/login` с `admin/admin`.

Ожидаемый результат:

- HTTP 200;
- возвращается session token;
- пользователь имеет роль `admin`.

### 8.2 Me

Вход: `GET /api/auth/me` с session Bearer token.

Ожидаемый результат:

- HTTP 200;
- возвращается текущий пользователь.

### 8.3 Logout

Вход: `POST /api/auth/logout`.

Ожидаемый результат:

- HTTP 200;
- повторный `me` со старым токеном не проходит.

## 9. Ограничения проверки

- Сценарии проверяют backend-контракты и продуктовую логику.
- Качество ML-классификации и детоксикации зависит от локальных весов моделей и не является основной целью лабораторной работы.
- In-memory store используется как демонстрационная реализация; после перезапуска сервера состояние очищается.
