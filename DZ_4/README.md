Александр, добрый вечер!

Задачи по семинару №4 - все решены

---------------------------------------------------------------
Задача 1-2-3
Сбилдить свой имейдж на основе имейджа mariaDB используя Dockerfile. Можете еще в Dockerfile добавить функционал по уменьшению веса образа к примеру. - Оценка Хорошо
Сделать свой конфиг для mariaDB и заменить конфиг базового имейджа используя Dockerfile. Допустим поменять порт, максимальное использованеи памяти и т.д. - Оцена Отлично
Запустить phpmyadmin (в контейнере) и через веб проверить, что все введенные данные доступны. - Оцена Отлично

Подготовил свой конфиг файл: my.cnf (смотрите его в этой-же папке в репозитории)
подготовил Dockerfile:

    FROM mariadb:10.10.2
    COPY my.cnf /etc/mysql/
    RUN bash

команда COPY my.cnf /etc/mysql/ копирует подготовленный файл конфига внутрь контейнера в папку /etc/mysql/

Сбилдил образ командой:
    sudo docker build -t mariadb_test .

Запустиил контейнер командой:
    sudo docker run --name test-mariadb_container -e MARIADB_ROOT_PASSWORD=test123 -d mariadb_test

Зашел внутрь контейнера командой:
    sudo docker exec -it test-mariadb_container bash

Внутри проверил конфиг файл командой:
    cat /etc/mysql/my.cnf

Запустил контейнер с phpMyAdmin командой:
    sudo docker run --name phpmyadmin-maridb_test -d --link test-mariadb_container:db -p 8083:80 phpmyadmin/phpmyadmin

Запустил браузер и открыл страницу по адресу:
    localhost:8083

Запустился вкб-интерфейс phpMyAdmin - все работает!

---------------------------------------------------------------
Задача 4 - со звездочкой
Сбилдить образ любой базы данных на основе alpine или debian/ubuntu. Запустить контейнер и проверить, что БД запустилась (допустим зайти через командную строку и вывести список баз данных) и присоеденить phpmyadmin (в контейнере)

Выбрал такой вариант:

    FROM ubuntu:22.10
    
    ENV TZ=Asia/Yekaterinburg
    RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
    
    RUN apt update -y
    RUN apt install -y nginx php php-fpm php-mysqli
    RUN apt clean all
    RUN echo "daemon off;" >> /etc/nginx/nginx.conf
    RUN mkdir /run/php-fpm

    COPY ./html/ /var/www/html/

    CMD php-fpm -D ; nginx

    EXPOSE 80

на хостовой машине заранее в папку ./html/ поместил файл index.html:

    <!DOCTYPE html>
    <html lang="ru">
    <head>
      <meta charset="utf-8">
      <meta hyyp-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Стили</title>
      <link rel="stylesheet" href="styles.css">
     </head>
    
    <body>
      <h1>МОЙ ТЕСТОВЫЙ САЙТ</h1>
      <h2>Загрузка данных...</h2>
    </body>
    </html>

Команда в Dockerfile:  COPY ./html/ /var/www/html/ подменяет дефолтный index на мой с хостовой машины

Итак собираем образ будущего сервера командой:
    sudo docker build -t my_server_image .

Запускаем контейнер командой:
    sudo docker run --name my_server_container -d -p 8085:80 my_server_image
(назначил порт 8085)

Если зайти на веб-браузере по адресу localhost:8085 то открывается моя тестовая веб-страница.
Если набрать просто localhost - то увидим стандартную страницу Apache2 Default Page

Все работает!

---------------------------------------------------------------
Простите за задержку отправки д/з! Огромная загрузка на работе...
Хотел не просто абы-как сделать домашку - а нормально так вникнуть в механизмы работы Docker
Очень прошу вас исправить оценку на странице сайта GB !!