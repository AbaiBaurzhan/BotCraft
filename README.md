# BotCraft 🤖

Платформа для создания ИИ-ассистентов без программирования. Создай своего бота за 2 минуты!

## 🚀 Быстрый старт

### 1. Установка зависимостей
```bash
cd bot_constructor
pip install -r ../requirements.txt
```

### 2. Настройка ключей
Отредактируй `touch.env`:
```bash
# Telegram Bot Token (получи у @BotFather)
TELEGRAM_TOKEN=your_actual_bot_token

# OpenAI API Key (получи на platform.openai.com)
OPENAI_API_KEY=your_actual_openai_key

# WebApp URL для локальной разработки
WEBAPP_URL=http://127.0.0.1:8000
```

### 3. Запуск
```bash
# Автоматический запуск обоих сервисов
python run_dev.py

# Или вручную:
# Терминал 1: FastAPI сервер
uvicorn app:app --host 127.0.0.1 --port 8000 --reload

# Терминал 2: Telegram бот
python main.py
```

## 🔧 Что исправлено

- ✅ Синтаксические ошибки в HTML
- ✅ Импорт CORS middleware
- ✅ Создан requirements.txt
- ✅ Создан touch.env шаблон
- ✅ Создан скрипт запуска

## 📱 Как использовать

1. **Создай бота в Telegram** через @BotFather
2. **Получи OpenAI API ключ** на platform.openai.com
3. **Запусти проект** локально
4. **Открой бота** в Telegram и нажми /start
5. **Настрой ассистента** через WebApp

## 🏗️ Архитектура

- `app.py` - FastAPI сервер + WebApp API
- `main.py` - Telegram бот
- `rag.py` - RAG система для документов
- `webapp/index.html` - Telegram WebApp интерфейс

## 🚨 Важно

- Проект использует SQLite для локальной разработки
- Все файлы загружаются в папку `uploads/`
- RAG система автоматически индексирует PDF/DOCX/XLSX/JPG/PNG
- WebApp работает только в Telegram (проверь initData)
