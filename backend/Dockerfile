# Создать образ на основе базового слоя,
# который содержит файлы ОС и интерпретатор Python 3.11.
FROM python:3.11-slim

# Переходим в образе в директорию /app: в ней будем хранить код проекта.
WORKDIR /app

# Установка системных зависимостей для psycopg2 и других пакетов
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Установка gunicorn
RUN pip install gunicorn==23.0.0

# Скопировать с локального компьютера файл зависимостей
COPY requirements.txt .

# Выполнить в текущей директории команду терминала
# для установки зависимостей.
RUN pip install -r requirements.txt --no-cache-dir

# Скопировать всё необходимое содержимое 
COPY . .

# При старте контейнера запустить production сервер.
CMD ["sh", "-c", "python manage.py migrate && gunicorn --bind 0.0.0.0:8000 backend.wsgi"]