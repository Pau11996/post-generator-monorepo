# 🚀 Post Generator Monorepo

Полнофункциональная система для создания и автоматической публикации контента в Telegram каналы, объединяющая AI-генерацию постов и автоматизированный постинг новостей.

## 📋 Обзор проекта

Этот монорепозиторий состоит из двух основных компонентов:

### 🎨 **Frontend** - AI-генератор постов
- **Технологии**: Next.js, React, TypeScript, Tailwind CSS
- **Функции**: Веб-интерфейс для создания Telegram постов с помощью AI
- **Deploy**: Vercel
- **Особенности**: Настройка стиля, тональности, форматирования, эмодзи и хэштегов

### 🤖 **Backend** - Автопостинг новостей
- **Технологии**: Python, OpenAI API, SQLite, Docker
- **Функции**: Автоматический парсинг RSS-лент, суммирование новостей и постинг в Telegram
- **Deploy**: Fly.io, Docker
- **Особенности**: Планировщик задач, предотвращение дубликатов, поддержка переводов

## 🚀 Быстрый старт

### Frontend (Генератор постов)

```bash
cd frontend
npm install
npm run dev
```

Откройте http://localhost:3000 для просмотра интерфейса генератора постов.

**Live Demo**: https://vercel.com/pashakozak96-2194s-projects/v0-telegram-post-generator

### Backend (Автопостинг)

#### С помощью Docker (рекомендуется)

```bash
cd backend

# Сборка образа
docker build -t autoposting .

# Тестовый запуск (без постинга)
docker run --rm \
  -e MAX_ARTICLES=2 \
  -e OFFLINE_MODE=true \
  -v "$(pwd)/data:/app/data" \
  autoposting --offline --max-articles 2

# Живой постинг
docker run --rm \
  --env-file .env \
  -v "$(pwd)/data:/app/data" \
  autoposting --post --max-articles 6
```

#### Локальный запуск

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Тестовый запуск
python main.py --offline --max-articles 6

# Живой постинг (требуются переменные окружения)
python main.py --post --max-articles 6
```

## ⚙️ Конфигурация

### Frontend Environment Variables

Frontend автоматически синхронизируется с v0.app и не требует дополнительной настройки для базового использования.

### Backend Environment Variables

Создайте файл `.env` в папке `backend`:

```env
# Обязательные переменные
OPENAI_API_KEY=sk-your_openai_api_key
TG_BOT_TOKEN=your_telegram_bot_token
TG_CHAT_ID=your_telegram_chat_id

# Опциональные настройки
LLM_MODEL=gpt-4o-mini
MAX_ARTICLES=6
RSS_SOURCES=https://news.ycombinator.com/rss,https://feeds.feedburner.com/TechCrunch/startups
TARGET_LANGUAGE=russian
TRANSLATE_POSTS=true
POSTING_HOUR=12
POSTING_MINUTE=0
DB_PATH=data/autoposting.db
```

### Получение Telegram Chat ID

1. Добавьте бота как администратора в канал
2. Отправьте сообщение в канал
3. Выполните запрос:
```bash
curl "https://api.telegram.org/bot$TG_BOT_TOKEN/getUpdates"
```
4. Найдите `chat.id` в ответе (например, `-1001234567890`)

## 🌐 Деплой в продакшн

### Frontend (Vercel)

Frontend автоматически деплоится на Vercel при изменениях в v0.app проекте.

### Backend (Fly.io)

```bash
# Установите Fly.io CLI
curl -L https://fly.io/install.sh | sh
flyctl auth login

# Создайте приложение и volume
flyctl apps create autoposting
flyctl volumes create autoposting_data --region ord --size 1

# Установите секреты
flyctl secrets set OPENAI_API_KEY="your_openai_api_key"
flyctl secrets set TG_BOT_TOKEN="your_telegram_bot_token" 
flyctl secrets set TG_CHAT_ID="your_telegram_chat_id"

# Деплой
flyctl deploy
```

## 📁 Структура проекта

```
post-generator-monorepo/
├── frontend/                 # Next.js веб-приложение
│   ├── app/                 # App Router страницы
│   ├── components/          # React компоненты
│   │   ├── post-generator.tsx  # Основной компонент генератора
│   │   └── ui/             # UI компоненты
│   ├── lib/                # Утилиты
│   └── package.json        # Frontend зависимости
├── backend/                 # Python автопостинг приложение
│   ├── autoposting_app/    # Основной модуль
│   │   ├── cli.py          # CLI интерфейс
│   │   ├── extract.py      # Извлечение контента
│   │   ├── fetch_feeds.py  # RSS парсер
│   │   ├── summarize.py    # AI суммирование
│   │   ├── post_telegram.py # Telegram API
│   │   └── scheduler.py    # Планировщик задач
│   ├── Dockerfile          # Docker конфигурация
│   ├── fly.toml           # Fly.io конфигурация
│   ├── main.py            # Точка входа
│   └── requirements.txt   # Python зависимости
└── README.md              # Этот файл
```

## 🎯 Основные функции

### Frontend - Генератор постов
- ✨ AI-генерация постов по теме
- 🎨 Настройка стиля и тональности
- 📝 Форматирование существующих постов
- 😀 Автоматические эмодзи и хэштеги
- 📏 Контроль длины постов
- 🎭 Множественные цветовые темы
- 📋 Копирование и экспорт результатов

### Backend - Автопостинг новостей
- 📡 Парсинг множественных RSS-лент
- 🤖 AI-суммирование статей (OpenAI)
- 📱 Автоматический постинг в Telegram
- 🔄 Предотвращение дубликатов
- 🌍 Поддержка переводов
- ⏰ Планировщик задач
- 💾 Persistent SQLite база данных
- 🐳 Docker контейнеризация

## 🛠️ Технический стек

### Frontend
- **Framework**: Next.js 15.2.4
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: Radix UI
- **Icons**: Lucide React
- **Forms**: React Hook Form + Zod
- **Deployment**: Vercel

### Backend
- **Language**: Python 3.10+
- **AI**: OpenAI API (GPT-4o-mini)
- **Database**: SQLite + SQLAlchemy
- **RSS Parsing**: feedparser
- **Content Extraction**: trafilatura
- **HTTP Requests**: requests
- **Scheduling**: pytz
- **Containerization**: Docker
- **Deployment**: Fly.io

## 📊 Использование и мониторинг

### Логи и отладка

**Frontend**: Используйте инструменты разработчика браузера и Vercel Analytics

**Backend**: 
```bash
# Просмотр логов в Fly.io
flyctl logs

# Локальные логи
docker run autoposting --help
```

### Мониторинг расходов

- **Frontend**: Бесплатный тариф Vercel
- **Backend**: ~$2-4/месяц на Fly.io
- **OpenAI API**: Зависит от использования (GPT-4o-mini ~$0.15/1M токенов)

## 🤝 Вклад в проект

1. Fork репозитория
2. Создайте feature branch (`git checkout -b feature/amazing-feature`)
3. Commit изменения (`git commit -m 'Add amazing feature'`)
4. Push в branch (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## 📄 Лицензия

Этот проект использует MIT лицензию. См. файл `LICENSE` для деталей.

## 🆘 Поддержка

Для вопросов и поддержки:
- Создайте Issue в GitHub
- Проверьте документацию в папках `frontend/` и `backend/`
- Изучите примеры конфигурации в коде

---

**🌟 Если проект полезен, поставьте звезду на GitHub!**
