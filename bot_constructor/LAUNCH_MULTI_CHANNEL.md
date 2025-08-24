# 🚀 Запуск BotCraft Multi-Channel

## ✅ Что готово к запуску

**BotCraft расширен для поддержки трех каналов связи:**

1. **📱 Telegram** - мета-бот владельца
2. **💬 WhatsApp Business** - через Meta Cloud API  
3. **📸 Instagram DM** - через Meta Graph API

## 🔧 Быстрый запуск

### 1. Проверьте конфигурацию

```bash
cd bot_constructor
cat touch.env
```

Убедитесь, что заполнены:
- `TELEGRAM_TOKEN` - токен вашего бота
- `OPENAI_API_KEY` - ключ OpenAI API
- Остальные токены для WhatsApp/Instagram (опционально)

### 2. Запустите систему

```bash
# Активируйте виртуальное окружение
source ../.venv/bin/activate

# Запустите multi-channel систему
python run_dev_multi.py
```

**Или запустите вручную:**

```bash
# Терминал 1: ngrok
ngrok http 8000

# Терминал 2: FastAPI
cd bot_constructor
source ../.venv/bin/activate
python -m uvicorn app:app --host 127.0.0.1 --port 8000 --reload
```

## 🌐 Доступные URL

После запуска:

- **Локально**: http://127.0.0.1:8000
- **HTTPS (ngrok)**: https://your-ngrok-url.ngrok-free.app
- **WebApp интерфейс**: доступен по обоим URL

## 📱 Проверка каналов

### 1. Статус каналов

```bash
curl http://127.0.0.1:8000/channels/status
```

### 2. Через WebApp

1. Откройте WebApp интерфейс
2. Нажмите "🧪 Тест каналов"
3. Нажмите "🔗 Получить Webhook URLs"

### 3. Тест отправки

```bash
# Отправка в Telegram
curl -X POST http://127.0.0.1:8000/api/send_message \
  -H "Content-Type: application/json" \
  -d '{"channel": "telegram", "chat_id": "123", "content": "Тест"}'
```

## 🎯 Что можно настроить

### WebApp интерфейс

- **Основная информация**: описание бизнеса, возможности
- **Тон общения**: дружелюбный, деловой, экспертный
- **Материалы**: загрузка PDF/DOCX/XLSX/JPG/PNG
- **Системный промпт**: автоматическая генерация + редактирование
- **Интеграции**: настройка всех трех каналов

### Каналы связи

- **Telegram**: автоматический polling/webhook режим
- **WhatsApp**: настройка через Meta Developer Console
- **Instagram**: проверка разрешений + fallback

## 🔄 Управление

### Запуск/остановка каналов

```bash
# Запуск всех каналов
curl -X POST http://127.0.0.1:8000/channels/start

# Остановка всех каналов  
curl -X POST http://127.0.0.1:8000/channels/stop

# Статус каналов
curl http://127.0.0.1:8000/channels/status
```

### Webhook URLs

Система автоматически предоставляет:

- `/webhook/telegram` - для Telegram
- `/webhook/whatsapp` - для WhatsApp
- `/webhook/instagram` - для Instagram

## 🚨 Возможные проблемы

### 1. Каналы не запускаются

```bash
# Проверьте логи
python run_dev_multi.py

# Проверьте токены
cat touch.env | grep -E "(TOKEN|KEY)"
```

### 2. Webhook не работает

```bash
# Проверьте ngrok
curl http://localhost:4040/api/tunnels

# Проверьте статус каналов
curl http://127.0.0.1:8000/channels/status
```

### 3. Ошибки импорта

```bash
# Установите зависимости
pip install -r ../requirements.txt

# Проверьте Python версию
python --version
```

## 🎉 Готово к использованию!

**Ваш BotCraft теперь поддерживает:**

✅ **Telegram** - полноценный бот с WebApp  
✅ **WhatsApp Business** - через Meta API  
✅ **Instagram DM** - с проверкой разрешений  
✅ **Единый бэкенд** - FastAPI + RAG + AI  
✅ **WebApp интерфейс** - настройка всех каналов  
✅ **Webhook поддержка** - для продакшена  

## 🚀 Следующие шаги

1. **Протестируйте все каналы** через WebApp
2. **Настройте реальные токены** для WhatsApp/Instagram
3. **Настройте webhook URLs** в Meta Developer Console
4. **Переведите в продакшен** режим
5. **Создайте ИИ-ассистентов** для вашего бизнеса!

**Удачи с вашим multi-channel BotCraft!** 🎯
