# 🔐 BotCraft - Система авторизации и управления агентами

## 📋 Обзор

BotCraft теперь включает полноценную систему авторизации пользователей и управления ИИ-агентами. Каждый пользователь может создавать несколько агентов, настраивать их под свои нужды и управлять интеграциями с различными каналами связи.

## 🏗️ Архитектура системы

### Компоненты:
1. **Database** (`database.py`) - Управление базой данных
2. **Auth** (`auth.py`) - Система авторизации
3. **Agents** (`agents.py`) - Управление ИИ-агентами
4. **Channels** (`channels/`) - Интеграции с каналами
5. **API** (`app.py`) - REST API эндпоинты
6. **WebApp** (`webapp/index_new.html`) - Веб-интерфейс

### Схема базы данных:
```
users (пользователи)
├── id (уникальный ID)
├── telegram_id (ID в Telegram)
├── email (опционально)
├── password_hash (хеш пароля)
├── salt (соль для пароля)
├── created_at (дата регистрации)
└── is_active (активен ли)

ai_agents (ИИ агенты)
├── id (уникальный ID)
├── user_id (связь с пользователем)
├── name (название агента)
├── business_description (описание бизнеса)
├── capabilities (возможности)
├── tone (тон общения)
├── system_prompt (системный промпт)
├── integrations_json (настройки каналов)
├── created_at (дата создания)
└── is_active (активен ли)

documents (документы)
├── id (уникальный ID)
├── agent_id (связь с агентом)
├── filename (имя файла)
├── file_path (путь к файлу)
├── file_type (тип файла)
├── uploaded_at (дата загрузки)
└── is_processed (обработан ли RAG)

sessions (сессии)
├── id (уникальный ID)
├── user_id (связь с пользователем)
├── session_token (токен сессии)
├── created_at (дата создания)
└── expires_at (дата истечения)
```

## 🔐 Система авторизации

### Способы входа:

#### 1. Telegram WebApp
- Автоматическая авторизация через `initData`
- HMAC проверка для безопасности
- Создание пользователя при первом входе

#### 2. Email/Password
- Классическая авторизация
- Хеширование паролей с солью
- JWT токены для API

### Безопасность:
- **HMAC** проверка Telegram данных
- **Хеширование** паролей (SHA-256 + соль)
- **JWT токены** с временем жизни
- **Сессии** в базе данных
- **Валидация** всех входных данных

## 🤖 Управление ИИ-агентами

### Создание агента:
1. **Основная информация:**
   - Название агента
   - Описание бизнеса
   - Возможности агента
   - Тон общения

2. **Автоматическая генерация:**
   - Системный промпт на основе настроек
   - Базовые интеграции

3. **Настройка каналов:**
   - Telegram (токен, режим webhook)
   - WhatsApp (access token, phone number ID)
   - Instagram (access token, business account ID)

### Лимиты:
- **Максимум 5 агентов** на пользователя
- **Размер файлов** до 10MB
- **Поддерживаемые форматы:** PDF, DOCX, XLSX, JPG, PNG

## 📱 Интеграции с каналами

### Telegram:
- **Режим разработки:** Polling
- **Режим продакшн:** Webhook
- **Автоматическое** создание бота

### WhatsApp Business:
- **Cloud API** через Meta
- **Webhook** для входящих сообщений
- **Отправка** сообщений через API

### Instagram:
- **Graph API** для Direct Messages
- **Webhook** для входящих сообщений
- **Проверка** разрешений на отправку

## 🚀 Запуск системы

### 1. Установка зависимостей:
```bash
pip install -r requirements.txt
```

### 2. Настройка окружения:
Создайте файл `touch.env`:
```env
# Telegram Bot Configuration
TELEGRAM_TOKEN=your_telegram_bot_token_here
TELEGRAM_WEBHOOK_MODE=false

# OpenAI API Configuration
OPENAI_API_KEY=your_openai_api_key_here

# WhatsApp Business API Configuration
WHATSAPP_ACCESS_TOKEN=your_whatsapp_access_token_here
WHATSAPP_PHONE_NUMBER_ID=your_whatsapp_phone_number_id_here
WHATSAPP_VERIFY_TOKEN=your_whatsapp_verify_token_here
WHATSAPP_APP_SECRET=your_whatsapp_app_secret_here

# Instagram Business API Configuration
INSTAGRAM_ACCESS_TOKEN=your_instagram_access_token_here
INSTAGRAM_BUSINESS_ACCOUNT_ID=your_instagram_business_account_id_here
INSTAGRAM_PAGE_ID=your_instagram_page_id_here
INSTAGRAM_VERIFY_TOKEN=your_instagram_verify_token_here

# JWT Secret (измените в продакшн)
JWT_SECRET_KEY=your-secret-key-change-in-production
```

### 3. Запуск:
```bash
python run_auth.py
```

## 🌐 API Эндпоинты

### Авторизация:
- `POST /api/auth/telegram` - Вход через Telegram
- `POST /api/auth/email` - Вход по email/password
- `POST /api/auth/logout` - Выход

### Управление агентами:
- `GET /api/agents` - Список агентов пользователя
- `POST /api/agents` - Создание агента
- `GET /api/agents/{id}` - Получение агента
- `PUT /api/agents/{id}` - Обновление агента
- `DELETE /api/agents/{id}` - Удаление агента

### Промпты:
- `POST /api/agents/{id}/generate_prompt` - Генерация промпта

### Интеграции:
- `PUT /api/agents/{id}/integrations` - Обновление интеграций

### Документы:
- `POST /api/agents/{id}/documents` - Загрузка документа
- `GET /api/agents/{id}/documents` - Список документов

### Каналы:
- `GET /channels/status` - Статус каналов
- `POST /channels/start` - Запуск каналов
- `POST /channels/stop` - Остановка каналов

### Webhooks:
- `GET/POST /webhook/telegram` - Telegram webhook
- `GET/POST /webhook/whatsapp` - WhatsApp webhook
- `GET/POST /webhook/instagram` - Instagram webhook

## 🔧 Разработка

### Структура проекта:
```
bot_constructor/
├── app.py              # Основной FastAPI сервер
├── database.py         # Управление базой данных
├── auth.py            # Система авторизации
├── agents.py          # Управление агентами
├── channels/          # Интеграции с каналами
│   ├── base.py       # Базовый класс канала
│   ├── telegram.py   # Telegram интеграция
│   ├── whatsapp.py   # WhatsApp интеграция
│   ├── instagram.py  # Instagram интеграция
│   └── manager.py    # Менеджер каналов
├── webapp/           # Веб-интерфейс
│   └── index_new.html # Новый интерфейс
├── run_auth.py       # Запуск с авторизацией
└── requirements.txt  # Зависимости
```

### Добавление нового канала:
1. Создайте класс в `channels/`
2. Наследуйтесь от `BaseChannel`
3. Реализуйте все абстрактные методы
4. Добавьте в `ChannelManager`

### Добавление новых полей агента:
1. Обновите схему в `database.py`
2. Добавьте поля в `AIAgent` dataclass
3. Обновите API эндпоинты
4. Обновите веб-интерфейс

## 🧪 Тестирование

### Проверка авторизации:
```bash
# Telegram авторизация
curl -X POST "http://127.0.0.1:8000/api/auth/telegram" \
  -H "Content-Type: application/json" \
  -d '{"initData": "your_telegram_init_data"}'

# Email авторизация
curl -X POST "http://127.0.0.1:8000/api/auth/email" \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "password123"}'
```

### Создание агента:
```bash
curl -X POST "http://127.0.0.1:8000/api/agents" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "Мой магазин",
    "business_description": "Интернет-магазин одежды",
    "capabilities": "Консультации, заказы, доставка",
    "tone": "дружелюбный"
  }'
```

## 🚨 Безопасность

### Рекомендации для продакшн:
1. **Измените** `JWT_SECRET_KEY`
2. **Используйте** HTTPS
3. **Настройте** CORS правильно
4. **Добавьте** rate limiting
5. **Настройте** логирование
6. **Используйте** переменные окружения
7. **Регулярно** обновляйте зависимости

### Мониторинг:
- Логи FastAPI
- Статус каналов
- Использование API
- Ошибки авторизации

## 📚 Дополнительные ресурсы

- [FastAPI документация](https://fastapi.tiangolo.com/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp)
- [Instagram Graph API](https://developers.facebook.com/docs/instagram-api)
- [JWT токены](https://jwt.io/)

## 🤝 Поддержка

При возникновении проблем:
1. Проверьте логи сервера
2. Убедитесь в правильности токенов
3. Проверьте настройки webhook
4. Убедитесь в доступности ngrok

---

**BotCraft v2.0** - Платформа для создания ИИ-ассистентов с полноценной системой авторизации и управления агентами.
