
# 📦 Дипломный проект по API‑тестированию

Минималистичный и практичный проект для тестирования REST API (на примере `reqres.in`).  
Демонтстрирует:
- модели **request/response** на `pydantic`,
- **авторизацию** по API (получение `token`),
- подготовку тестовых данных/сущностей для UI‑проекта,
- **валидацию схем**, **логирование** в консоль и вложения в **Allure**,
- работу через **endpoint** (базовый URI хранится в `browser.config.base_url` в `conftest.py`).

> База: `https://reqres.in/api` 

---

## ⚙️ Запуск

```bash

pip install -r requirements.txt
pytest
```

Allure‑отчёт:
```bash
allure serve allure-results
```

---

## 🧱 Структура

```
api_diploma/
├─ client/
│  ├─ api_client.py        # обёртка над requests.Session
│  └─ endpoints.py         # константы эндпоинтов
├─ models/
│  └─ user.py              # pydantic‑модели: request/response
├─ tests/                  # ≥5 тестов: GET, POST, DELETE, auth и негативный кейс
│  ├─ test_get_users.py
│  ├─ test_create_user.py
│  ├─ test_delete_user.py
│  ├─ test_auth_login.py
│  └─ test_login_negative.py
├─ utils/
│  ├─ allure_helpers.py    # вложения request/response
│  └─ logging_config.py    # красивое консольное логирование
├─ conftest.py             # base_url через browser.config.base_url + фикстуры клиента/авторизации
├─ .env.example            # переменные окружения для base_url и логина
├─ pytest.ini              # конфиг pytest (allure dir)
└─ requirements.txt
```

---

## ✅ Что закрыто требованиями

- [x] **≥5 тестов** (GET/POST/DELETE + позитив/негатив).
- [x] Базовый **URI** в `browser.config.base_url` (см. `conftest.py`), запросы только через **endpoint** (`/users`, `/login` и т.д.).
- [x] **Модели** для запроса и ответа (см. `models/user.py`).
- [x] **Схемы** валидации: разбор ответа через `pydantic` + формирование тела из модели запроса.
- [x] **Allure**‑вложения: запрос и ответ (метод, URL, headers, body, статус, время).
- [x] **Консольное логирование** (уровень, дата/время, код ответа, метод, client URL, длительность).
- [x] **Авторизация** `POST /login` → `token` (фикстура `auth_token`), пригодно для использования в UI‑тестах.

---

## 🔗 Интеграция с UI‑проектом

1. Фикстура `auth_token` получает `token` и может экспортировать его в файл `reports/api_token.txt`.  
2. В UI‑тестах читайте токен и подставляйте в заголовок/локальное хранилище/куки в pre‑stepах.

