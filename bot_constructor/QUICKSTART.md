# 🚀 Быстрый старт SelinaAI

## ✅ Что уже готово
- Все зависимости установлены
- Код исправлен и протестирован
- Виртуальное окружение настроено

## 🔑 Шаг 1: Получи ключи

### Telegram Bot Token
1. Напиши @BotFather в Telegram
2. Команда: `/newbot`
3. Придумай имя и username для бота
4. Скопируй полученный токен

### OpenAI API Key
1. Зайди на [platform.openai.com](https://platform.openai.com)
2. Создай API ключ
3. Скопируй ключ

## ⚙️ Шаг 2: Настрой конфигурацию

Отредактируй файл `touch.env`:
```bash
TELEGRAM_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
OPENAI_API_KEY=sk-1234567890abcdef...
WEBAPP_URL=http://127.0.0.1:8000
FALLBACK_SYSTEM_PROMPT=Ты вежливый ассистент малого бизнеса
```

## 🚀 Шаг 3: Запуск

### Вариант 1: Автоматический запуск
```bash
source venv/bin/activate
python run_dev.py
```

### Вариант 2: Ручной запуск
```bash
# Терминал 1: FastAPI сервер
source venv/bin/activate
uvicorn app:app --host 127.0.0.1 --port 8000 --reload

# Терминал 2: Telegram бот
source venv/bin/activate
python main.py
```

## 📱 Шаг 4: Тестирование

1. Открой бота в Telegram
2. Нажми `/start`
3. Нажми кнопку "🔧 Открыть панель"
4. Пройди мастер настройки

## 🔧 Доступные команды

- `/start` - главное меню
- `/panel` - открыть панель настроек
- `/ask вопрос` - быстрый тест ассистента
- `/upload` - загрузить документ для RAG

## 🌐 WebApp URL

После запуска WebApp будет доступен по адресу:
http://127.0.0.1:8000

## 🚨 Возможные проблемы

### "No module named 'fastapi'"
```bash
source venv/bin/activate
pip install fastapi uvicorn
```

### "TELEGRAM_TOKEN не найден"
Проверь файл `touch.env` и убедись, что токен указан правильно

### "OPENAI_API_KEY не найден"
Проверь файл `touch.env` и убедись, что ключ указан правильно

## 📁 Структура проекта

```
bot_constructor/
├── app.py          # FastAPI сервер + API
├── main.py         # Telegram бот
├── rag.py          # RAG система
├── webapp/         # Telegram WebApp
├── touch.env       # Конфигурация
├── run_dev.py      # Скрипт запуска
└── venv/           # Виртуальное окружение
```

## 🎯 Что дальше?

1. **Настрой ассистента** через WebApp
2. **Загрузи документы** командой `/upload`
3. **Протестируй** команду `/ask`
4. **Интегрируй** с WhatsApp/Instagram
