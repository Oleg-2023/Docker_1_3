# Docker_1_3
# Topic #1.3. Docker. Home work. 

Эта работа использует 1 docker-контейнер.

**Для описания контейнера используется файл database\Dockerfile:
Используем официальный последний образ PostgreSQL
Создаем базу данных**
FROM postgres:latest
ENV POSTGRES_DB=database
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=password

**Копируем SQL-скрипт из относительного пути в контейнер**
COPY init.sql /docker-entrypoint-initdb.d/init.sql
**Контейнер использует VOLUME для хранения данных
что позволяет сохранить данные при перезапуске контейнера**
VOLUME data:/var/lib/postgressql/data


**В корне проекта создаем файл docker-compose.yml, 
который инструкцией build с помощью инструкций /database/dockerfile создает контейнер**
version: "3.9"
services:
  db:
    container_name: pg-1-3
    build: ./database
    ports:
      - 5430:5432

**Командой docker compose up -d запускаем в фоновом режиме инициализацию 
контейнера и выполнение  инструкций**

/docker-entrypoint-initdb.d/init.sql автоматически выполнятся после создания контейнера
и создает таблицу index_mass и заполняет ее начальными данными.

**Запускаем из командной строки psql:**
docker exec -it pg-1-3 psql -U postgres -W database ,
где pg-1-3 контейнер с базой данных

**Добавляем, редактируем, сохраняем данные в базу database**

