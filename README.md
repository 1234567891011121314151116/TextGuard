# Лабораторная работа №8

В этой папке собраны материалы по лабораторной работе на тему Structured-Prompt-Driven Development (SPDD). Работа выполнена для курсового проекта TextGuard AI и ограничена backend-частью приложения.

Важно: frontend-интерфейс, обучение моделей, подготовка датасетов и изменение локальных весов RuBERT/ruT5 не входят в область данной лабораторной работы. В SPDD-артефактах рассматривается серверная B2B-обвязка проекта: FastAPI, API-ключи, тарифы, dashboard, авторизация и интеграция существующих ML-сервисов.

## Состав папки

- `report.md` - основной отчет по лабораторной работе;
- `test_scenarios.md` - тестовые сценарии для функциональной проверки backend;
- `prompts/prompt_v0.md` - анализ контекста backend и границ ответственности;
- `prompts/prompt_v1.md` - проектирование B2B API и маршрутов FastAPI;
- `prompts/prompt_v2.md` - проектирование API-ключей, тарифов и in-memory store;
- `prompts/prompt_v3.md` - реализация основного endpoint `/api/v1/moderation/analyze`;
- `prompts/prompt_v4.md` - реализация управления ключами, подписками и dashboard;
- `prompts/prompt_v5.md` - сохранение legacy `/api/moderate` и admin endpoint;
- `prompts/prompt_v6.md` - генерация backend-тестовых сценариев;
- `prompts/prompt_v7.md` - финальная проверка контрактов, ошибок и соответствия проекту;
- `lab8_textguard_backend_готово.docx` - версия отчета в формате Word.

## Тема задания

Разработка backend-части TextGuard AI для B2B-доступа к AI-модерации и детоксикации текста: Bearer API-ключи, тарифные ограничения, журналирование запросов, dashboard, legacy-совместимость и вызов существующих сервисов `ClassifierService` и `DetoxService`.

## Файлы backend, на которые опирается работа

- `backend/main.py`
- `backend/deps.py`
- `backend/auth.py`
- `backend/store.py`
- `backend/plans.py`
- `backend/user_store.py`
- `backend/routes/v1_moderation.py`
- `backend/routes/moderation.py`
- `backend/routes/api_keys.py`
- `backend/routes/subscriptions.py`
- `backend/routes/dashboard.py`
- `backend/routes/auth.py`
- `backend/routes/keys.py`
- `backend/services/classifier_service.py`
- `backend/services/detox_service.py`

## Методологическая основа

Martin Fowler, Thoughtworks: Structured-Prompt-Driven Development  
https://martinfowler.com/articles/structured-prompt-driven/
