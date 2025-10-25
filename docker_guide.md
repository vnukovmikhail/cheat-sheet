#### Содержимое:
- [How to use docker?](#how-to-use-docker)
- [How to use docker-compose.yml?](#how-to-use-docker-composeyml)
- [How to write .yml files?](#how-to-write-yml-files)
  - [Dockerfile](#dockerfile)
  - [docker-compose.yml](#docker-composeyml)


> `Quiz:` на тему `Docker`
> 
>  > `Question 1:` зачем нужен `Docker`?
>  > `Answer:` `docker` это сервис который позволяет запускать любой сайт, базу данных и остальное в `изолированной` оболочке. 
> 
>  > `Question 2:` зачем это нужно?
>  > `Answer:` `изолированный` контейнер гарантирует то что база данных будет работать сама по себе, без нужны в дополнительных библиотеках и зависимостях
>
>  > `Question 3:` как запустить созданный мной сделать?
>  > `Answer:` для запуска собственного файла необходимо создать новый файл `docker-compose.yml` и `Dockerfile`
>  
>  >  `Question 3:` как запустить созданный мной сайт?
>  > `Answer:` для запуска собственного файла необходимо создать новый файл `docker-compose.yml` и `Dockerfile`
> 

---

#### How to use `docker`?:

|Действие|Команда|
|---|---|
|Список **запущенных** контейнеров|`docker ps`|
|Список **всех** контейнеров (в т.ч. остановленных)|`docker ps -a`|
|Запустить контейнер|`docker start <id>`|
|Остановить контейнер|`docker stop <id>`|
|Перезапустить контейнер|`docker restart <id>`|
|Удалить контейнер|`docker rm <id>`|
|Удалить все контейнеры|`docker rm -f $(docker ps -aq)`|
|Зайти внутрь контейнера|`docker exec -it <id> bash`|
|Просмотреть логи контейнера|`docker logs <id>`|

| Что удалить                                         | Команда                            |
| --------------------------------------------------- | ---------------------------------- |
| Все остановленные контейнеры                        | `docker container prune`           |
| Все неиспользуемые образы                           | `docker image prune -a`            |
| Всё неиспользуемое (контейнеры, образы, тома, сети) | `docker system prune -a --volumes` |

| Действие     | Команда                    |
| ------------ | -------------------------- |
| Список сетей | `docker network ls`        |
| Список томов | `docker volume ls`         |
| Удалить сеть | `docker network rm <name>` |
| Удалить том  | `docker volume rm <name>`  |

---

#### How to use `docker-compose.yml`?:

|Действие|Команда|
|---|---|
|Запустить все сервисы|`docker compose up -d`|
|Остановить все сервисы|`docker compose down`|
|Перезапустить|`docker compose restart`|
|Посмотреть логи|`docker compose logs`|
|Войти в контейнер сервиса|`docker compose exec <service_name> bash`|

---


---
### How to write `.yml` files?

#### `Dockerfile`

>`Dockerfile` необходим для настройки запуска собственного образа, позволяет поэтапно расписать выполняемые команды требующиеся для запуска сайта или сервиса. _(пример `npm run build` для создания исполняемого файла `main.js` который уже и будет использоваться как файл запуска)_

```dockerfile
# Пример Dockerfile для создания сервиса на основе Nestjs

FROM node:24-alpine
# Указывает `базовый образ`, на основе которого будет собираться твой контейнер.
# `node:24-alpine` — это Node.js версии 24 на облегчённом Alpine Linux (меньше размер, меньше уязвимостей) 
# Все остальные команды будут выполняться поверх этого образа.

WORKDIR /usr/src/app
# Устанавливает `рабочую директорию` внутри контейнера.
# Все последующие команды `COPY`, `RUN`, `CMD` будут выполняться относительно `/usr/src/app`.
# Если директория не существует, Docker создаст её.

COPY package*.json ./
# Копирует файлы `package.json` и `package-lock.json` (или `npm-shrinkwrap.json`) из твоего проекта в контейнер* в текущую рабочую директорию (`/usr/src/app`).
# Это делается отдельно от остального кода для того, чтобы кэшировать установку зависимостей — если `package.json` не менялся, Docker не будет заново устанавливать пакеты при следующей сборке.

RUN npm install
# Выполняет команду `npm install` внутри контейнера.
# Устанавливает все зависимости, указанные в `package.json`.
# Результат сохраняется в слое образа, чтобы при сборке можно было использовать кэш.

COPY . .
# Копирует **весь проект** (все файлы и папки, кроме игнорируемых в `.dockerignore`) в рабочую директорию контейнера (`/usr/src/app`). 
# Теперь весь код приложения, конфиги, скрипты будут внутри контейнера.

RUN npm run build
# Выполняет скрипт сборки из `package.json`.
# В NestJS обычно это компиляция TypeScript в JavaScript: создаётся папка `dist/` с готовым JS-кодом.

CMD [ "node", "dist/main.js" ]
# Указывает *команду по умолчанию*, которая выполняется при запуске контейнера. 
# Здесь запускается Node.js и стартует твой NestJS сервер. 
# Если запускать контейнер через `docker run` без дополнительных команд, именно это и будет выполняться.

EXPOSE 3000
# Сообщает Docker, что контейнер слушает порт *3000*.
# Не открывает порт автоматически — нужно пробросить его через `docker run -p 3000:3000` или в `docker-compose.yml`.
# Это просто **метаданные**, чтобы другие могли понимать, на каком порту работает сервис.
```

```dockerfile
# Пример Dockerfile для создания сервиса на основе Django

# 1. Базовый образ Python (легковесный)
FROM python:3.12-slim
# Лёгкий образ Python 3.12 без лишних пакетов → меньше размер и меньше потенциальных уязвимостей.

# 2. Рабочая директория в контейнере
WORKDIR /app
# Создаёт рабочую директорию `/app` внутри контейнера.

# 3. Копируем зависимости
COPY requirements.txt ./
# Копируем файл зависимостей, отдельно от кода.

# 4. Устанавливаем зависимости
RUN pip install --no-cache-dir -r requirements.txt
# Устанавливаем все Python-библиотеки из requirements.txt, без сохранения кэша pip.

# 5. Копируем весь код проекта
COPY . .
# Копируем весь проект в контейнер (`manage.py`, папку `myproject/`, `templates/`, `static/` и т.д.).

# 6. Создаём переменные окружения для Python
ENV PYTHONDONTWRITEBYTECODE 1
# Python не создаёт `.pyc` файлы.

ENV PYTHONUNBUFFERED 1
# Вывод Python сразу идёт в консоль (без буферизации).

# 7. Открываем порт 8000 (Django dev server)
EXPOSE 8000
# Сообщаем, что контейнер слушает порт 8000 (Django dev server).

# 8. Команда по умолчанию для запуска сервера
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
# Запуск Django сервера на всех интерфейсах контейнера (`0.0.0.0`), порт 8000.
```

> `Information!` доступные образы можно посмотреть на сайте [Dockerhub](https://hub.docker.com/)

---

#### `docker-compose.yml`

> `docker-compose.yml` нужен для того чтоб прописать сами контейнеры, в этом файле указывается то в каком образе контейнера, под каким портом, переменный окружения необходимые для работы образа и тому подобное

```docker-compose.yml
services:
  nestjs:
    build: .
    container_name: nestjs_app
    ports:
      - "3000:3000"
    depends_on:
      - postgres
    environment:
      DB_HOST: ${DB_HOST}
      DB_PORT: ${DB_PORT}
      DB_USER: ${DB_USER}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_NAME: ${DB_NAME}
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
  postgres:
    container_name: nestjs_app_db
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - db-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  db-data:
```

```yaml
services:
  nestjs:
    ...
  postgres:
    ...
volumes:
  db-data:
# `services:` — раздел, где описываются контейнеры, которые будут подниматься.
# `volumes:` — раздел для томов, где Docker хранит данные вне контейнера (чтобы база не пропадала при удалении контейнера).

nestjs:
  build: .
# Говорит Docker Compose собрать образ из *Dockerfile* в текущей папке (`.`).

container_name: nestjs_app
# Задаёт имя контейнера (nestjs_app) вместо автоматически сгенерированного.

ports:
 - "3000:3000"
# Пробрасывает порт контейнера наружу.
# Левая часть (`3000`) — порт на хосте, правая (`3000`) — порт внутри контейнера, который слушает NestJS.

depends_on:
 - postgres
# Указывает, что контейнер NestJS зависит от Postgres.  
# Docker Compose сначала поднимет Postgres, затем NestJS.  
# Важно: `depends_on` не гарантирует, что база уже готова к подключению, просто контейнер поднят.

environment:
  DB_HOST: ${DB_HOST}
  DB_PORT: ${DB_PORT}
  DB_USER: ${DB_USER}
  DB_PASSWORD: ${DB_PASSWORD}
  DB_NAME: ${DB_NAME}
# Задаёт **переменные окружения** для контейнера NestJS.
# `${DB_HOST}` и т.д. подставляются из файла `.env` (если он лежит рядом с `docker-compose.yml`).
# NestJS через эти переменные будет подключаться к Postgres.

volumes:
 - .:/usr/src/app
 - /usr/src/app/node_modules
# Пробрасывает текущую директорию (`.`) внутрь контейнера (`/usr/src/app`), чтобы можно было менять код без пересборки образа (hot reload).
# `/usr/src/app/node_modules` создаётся как отдельный том, чтобы **не перезаписывать зависимости хостовой папки**.

postgres:
  container_name: nestjs_app_db
  image: postgres:latest
# Использует официальный образ Postgres последней версии.   
# Имя контейнера — `nestjs_app_db`.

environment:
  POSTGRES_PASSWORD: ${DB_PASSWORD}
  POSTGRES_USER: ${DB_USER}
  POSTGRES_DB: ${DB_NAME}
# Переменные окружения, необходимые для инициализации базы.   
# `${DB_PASSWORD}` и т.д. подставляются из `.env`.

volumes:
 - db-data:/var/lib/postgresql/data
# Пробрасываем том `db-data` внутрь контейнера, чтобы данные базы сохранялись между перезапусками.

ports:
 - "5432:5432"
# Пробрасываем порт базы наружу, чтобы можно было подключаться с хоста (например, через DBeaver или pgAdmin).

volumes:
  db-data:
# Создаём том `db-data`, который будет хранить данные Postgres вне контейнера.
# Благодаря этому база *не удаляется при пересборке или удалении контейнера*.
```


---

> Таким образом в `Dockerfile` построчно указывается то что именно будет происходить.

