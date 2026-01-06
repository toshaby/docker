Чтобы развернуть проект выполните след команды в терминале, в пустой папке проекта<br>
(Должен быть установлен Docker)

git clone https://github.com/toshaby/docker.git .<br>

*****************************<br>
Для Linux машины нужно выполнить:<br>
--LINUX--<br>
cp .env.linux .env<br>
cp docker-compose.override.yml.linux docker-compose.override.yml<br>
затем в файле .env проверьте строки<br>
UID=1000<br>
GID=1000<br>
чтобы они соответствовали идентификатору пользователя/групы под которым вы будете работать на хосте<br>
--ENDLINUX--<br>
******************************<br>

git clone https://github.com/toshaby/smart.git src<br>

docker compose up -d --build<br>

Переходим в терминал контейнера:<br>
docker compose exec app /bin/bash<br>
Далее консольные команды выполняются именно в терминале контейнера<br>
Папка src является корнем Laravel проекта, через IDE работаем в ней<br>

composer install<br>
npm install<br>
npm run build<br>

cp .env.example .env<br>
php artisan key:generate<br>

Отредактируйте .env (src/.env), там уже указаны доступы к базе из докер контейнера, должно все работать. Для удобства рекомендую поменять часовой пояс<br>

php artisan migrate --seed<br>
в консоли, во время создания пользователей отобразится эмейл и пароль менеджера, который будет иметь доступ к админ панели

php artisan storage:link<br>

Проект открывается в браузере по урл:<br>
http://localhost:8080<br>

Для создания больше тестовых данных можно еще запустить дополнительные сидеры

php artisan db:seed --class=CustomerSeeder<br>
php artisan db:seed --class=TicketSeeder<br>

Пример вставки виджета для создания тикета<br>
<iframe style="position:fixed;bottom:10px;right:10px;width:400px;height:550px;" src="http://localhost:8080/feedback-widget"></iframe>

Примеры api

####
POST /api/tickets<br>
запрос или Content-type application/json или multipart/form-data(в этом случае можно передать файлы, множественное поле files[])<br>
обязателен http заголовок Accept: application/json
***
Создание тикета с одинковым телефоном\email возможно раз в сутки
***
<pre>
{
    "name":"toha",
    "phone":"+380988369752",
    "email":"toha233@mail.ru",
    "theme":"Тестовая тема",
    "text":"Пишем текст"
}
</pre>
ответ
<pre>
{
    "data": {
        "id": 12,
        "theme": "Тестовая тема",
        "text": "Пишем текст",
        "name": "toha",
        "phone": "+380988369752",
        "email": "toha233@mail.ru"
    }
}
</pre>
или
<pre>
{
    "message": "Укажите телефон если не указан Email (and 3 more errors)",
    "errors": {
        "phone": [
            "Укажите телефон если не указан Email"
        ],
        "email": [
            "Укажите Email если не указан телефон"
        ],
        "theme": [
            "Укажите тему сообщения"
        ],
        "text": [
            "Текст сообщения обязателен"
        ]
    }
}
</pre>
####

####
GET /api/tickets/statistics<br>
обязательно заголовок Accept: application/json
<pre>
ответ
{
    "data": {
        "day": 2,
        "week": 3,
        "month": 7
    }
}
</pre>

