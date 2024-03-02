Kittygram

Учебный проект Замечательные коты

Этот проект позволит вам выкладывать свои и любоваться чужими фото котов и кошек. В описание добавьте имя, цвет и достижения пушистого героя. Красиво и абсолютно безопасно для аллергиков:)

Описание проекта

Проект Kittygram находится по адресу: sprint14.zapto.org

Стэк технологий

Python 3.9
Django 3.2.3
djangorestframework 3.12.4
djoser 2.1.0
webcolors 1.11.1
gunicorn 20.1.0
psycopg2-binary 2.9.6
pytest-django 4.4.0
pytest-pythonpath 0.7.3
pytest 6.2.4
PyYAML 6.0
django-environ 0.10.0
Локальный запуск проекта

Клонировать репозиторий и перейти в него в командной строке:

git clone https://github.com/anzorkhamukov/kittygram_final.git
cd kittygram_final
Cоздать и активировать виртуальное окружение, установить зависимости:

python3 -m venv venv && \ 
    source venv/scripts/activate && \
    python -m pip install --upgrade pip && \
    pip install -r backend/requirements.txt
Установите docker compose на свой компьютер.

Запустите проект через docker-compose:

docker compose -f docker-compose.yml up --build -d
Выполнить миграции:

docker compose -f docker-compose.yml exec backend python manage.py migrate
Соберите статику и скопируйте ее:

docker compose -f docker-compose.yml exec backend python manage.py collectstatic  && \
docker compose -f docker-compose.yml exec backend cp -r /app/static_backend/. /backend_static/static/
.env

В корне проекта создайте файл .env и пропишите в него свои данные.

Пример:

POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_NAME=kittygram
Workflow

Для использования Continuous Integration (CI) и Continuous Deployment (CD): в репозитории GitHub Actions Settings/Secrets/Actions прописать Secrets - переменные окружения для доступа к сервисам:

SECRET_KEY                     # стандартный ключ, который создается при старте проекта

DOCKER_USERNAME                # имя пользователя в DockerHub
DOCKER_PASSWORD                # пароль пользователя в DockerHub
HOST                           # ip_address сервера
USER                           # имя пользователя
SSH_KEY                        # приватный ssh-ключ (cat ~/.ssh/id_rsa)
PASSPHRASE                     # кодовая фраза (пароль) для ssh-ключа

TELEGRAM_TO                    # id телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN                 # токен бота (получить токен можно у @BotFather, /token, имя бота)
При push в ветку main автоматически отрабатывают сценарии:

tests - проверка кода на соответствие стандарту PEP8 и запуск pytest. Дальнейшие шаги выполняются только если push был в ветку main;
build_and_push_to_docker_hub - сборка и доставка докер-образов на DockerHub
deploy - автоматический деплой проекта на боевой сервер. Выполняется копирование файлов из DockerHub на сервер;
send_message - отправка уведомления в Telegram.
