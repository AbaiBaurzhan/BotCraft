# 🚀 BotCraft Multi-Channel Setup Guide

## 🌟 Обзор новой архитектуры

BotCraft теперь поддерживает **три канала связи** с единым FastAPI-бэкендом:

1. **📱 Telegram** - мета-бот владельца (polling в dev, webhook в prod)
2. **💬 WhatsApp Business** - приём/ответ через вебхуки Meta
3. **📸 Instagram DM** - приём входящих через вебхуки, отправка с проверкой разрешений

## 🏗️ Архитектура системы

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Telegram      │    │   WhatsApp       │    │   Instagram     │
│   Channel       │    │   Channel        │    │   Channel       │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌──────────────────┐
                    │  Channel Manager │
                    └──────────────────┘
                                 │
                    ┌──────────────────┐
                    │   FastAPI App    │
                    │  + Webhooks      │
                    └──────────────────┘
                                 │
                    ┌──────────────────┐
                    │   RAG System     │
                    │  + AI Processing │
                    └──────────────────┘
```

## 📋 Структура файлов

```
bot_constructor/
├── channels/                 # Модуль каналов
│   ├── __init__.py          # Инициализация модуля
│   ├── base.py              # Базовый класс канала
│   ├── telegram.py          # Telegram канал
│   ├── whatsapp.py          # WhatsApp канал
│   ├── instagram.py         # Instagram канал
│   └── manager.py           # Менеджер каналов
├── app.py                   # Обновленный FastAPI с поддержкой каналов
├── run_dev_multi.py         # Новый runner для multi-channel
├── touch.env                # Конфигурация всех каналов
└── webapp/
    └── index.html           # Обновленный WebApp интерфейс
```

## 🔧 Настройка окружения

### 1. Обновите touch.env

```bash
# Telegram Bot Configuration
TELEGRAM_TOKEN=your_telegram_bot_token_here
TELEGRAM_WEBHOOK_MODE=false

# WebApp URL для локальной разработки
WEBAPP_URL=https://your-ngrok-url.ngrok-free.app

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
```

### 2. Получение токенов и ключей

#### Telegram Bot
1. Создайте бота через @BotFather
2. Получите токен
3. Для продакшена настройте webhook

#### WhatsApp Business
1. Создайте приложение в [Meta Developer Console](https://developers.facebook.com/)
2. Добавьте WhatsApp Business API
3. Получите Access Token, Phone Number ID
4. Создайте Verify Token и App Secret

#### Instagram Business
1. В том же Meta приложении добавьте Instagram Basic Display
2. Получите Access Token
3. Найдите Instagram Business Account ID и Page ID
4. Создайте Verify Token

## 🚀 Запуск системы

### 1. Запуск через новый runner

```bash
cd bot_constructor
source ../.venv/bin/activate
python run_dev_multi.py
```

### 2. Ручной запуск

```bash
# Запуск ngrok
ngrok http 8000

# В другом терминале
cd bot_constructor
source ../.venv/bin/activate
python -m uvicorn app:app --host 127.0.0.1 --port 8000 --reload
```

## 🌐 Webhook URLs

После запуска система предоставит следующие webhook URLs:

- **Telegram**: `https://your-domain.com/webhook/telegram`
- **WhatsApp**: `https://your-domain.com/webhook/whatsapp`
- **Instagram**: `https://your-domain.com/webhook/instagram`

## 📱 Настройка каналов

### Telegram
- **Режим разработки**: Polling (автоматически)
- **Режим продакшена**: Webhook (установите TELEGRAM_WEBHOOK_MODE=true)

### WhatsApp
1. В Meta Developer Console настройте webhook URL
2. Укажите Verify Token
3. Система автоматически обработает верификацию

### Instagram
1. В Meta Developer Console настройте webhook URL
2. Укажите Verify Token
3. Система проверит разрешения и настроит доступ

## 🧪 Тестирование

### 1. Проверка статуса каналов

```bash
curl http://127.0.0.1:8000/channels/status
```

### 2. Тест через WebApp
- Откройте WebApp интерфейс
- Нажмите "🧪 Тест каналов"
- Нажмите "🔗 Получить Webhook URLs"

### 3. Тест отправки сообщений

```bash
# Отправка в конкретный канал
curl -X POST http://127.0.0.1:8000/api/send_message \
  -H "Content-Type: application/json" \
  -d '{"channel": "telegram", "chat_id": "123", "content": "Тест"}'

# Отправка во все каналы
curl -X POST http://127.0.0.1:8000/api/send_message_all \
  -H "Content-Type: application/json" \
  -d '{"chat_id": "123", "content": "Тест всем каналам"}'
```

## 🔄 Управление каналами

### API эндпоинты

- `GET /channels/status` - статус всех каналов
- `POST /channels/start` - запуск всех каналов
- `POST /channels/stop` - остановка всех каналов
- `POST /api/send_message` - отправка в конкретный канал
- `POST /api/send_message_all` - отправка во все каналы

### Webhook эндпоинты

- `GET/POST /webhook/telegram` - Telegram webhook
- `GET/POST /webhook/whatsapp` - WhatsApp webhook
- `GET/POST /webhook/instagram` - Instagram webhook

## 🎯 Особенности реализации

### Telegram Channel
- ✅ Polling режим для разработки
- ✅ Webhook режим для продакшена
- ✅ Автоматическое переключение режимов
- ✅ Поддержка всех типов сообщений

### WhatsApp Channel
- ✅ Верификация webhook через Meta API
- ✅ Проверка подписи для безопасности
- ✅ Обработка всех типов сообщений
- ✅ Автоматическая отправка ответов

### Instagram Channel
- ✅ Проверка разрешений через Graph API
- ✅ Функция-обёртка для отправки
- ✅ Fallback при отсутствии разрешений
- ✅ Поддержка Direct Messages

## 🚨 Устранение неполадок

### Канал не запускается
1. Проверьте токены в touch.env
2. Убедитесь, что API доступны
3. Проверьте логи в консоли

### Webhook не работает
1. Проверьте URL в настройках Meta
2. Убедитесь, что ngrok запущен
3. Проверьте верификацию токенов

### Ошибки отправки
1. Проверьте статус канала
2. Убедитесь в наличии разрешений
3. Проверьте формат сообщения

## 🎉 Готово!

Теперь у вас есть полноценная multi-channel система BotCraft!

**Что вы получили:**
- ✅ Единый бэкенд для трех каналов
- ✅ Автоматическое управление каналами
- ✅ Webhook поддержка для продакшена
- ✅ Унифицированный интерфейс настройки
- ✅ Проверка разрешений и fallback
- ✅ Простое тестирование и мониторинг

**Следующие шаги:**
1. Настройте реальные токены в touch.env
2. Протестируйте все каналы
3. Настройте webhook URLs в Meta Developer Console
4. Переведите в продакшен режим

Удачи с вашим multi-channel ИИ-ассистентом! 🚀
