Задание 1
1) создать docker compose файл, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД
2) запустить docker compose файл
3) по итогу на БД контейнере должно быть 2 реплики, на админере должна быть 1 реплика. Всего должно получиться 3 контейнера
4) выводы зафиксировать

Смотрите скриншоты в папке: Домашнее задание 1

Скриншот 1 - устанавливаю docker-compose

Скриншот 2 - готовлю два файла

файл №1: docker-compose.yml

    version: '3.9'

    services:    
      db:
        build:
          docekrfile: ./Dockerfile
        environment:
          MYSQL_ROOT_PASSWORD: 12345
        deploy:
          mode: replicated
          replicas: 2

      adminer:
        image: adminer:4.8.1
        restart: always
        ports:
          - 6080:8080
        volumes:
          - ./myfolder:/myfolder
        deploy:
          mode: replicated
          replicas: 1

файл №2: Dockerfile

    FROM mariadb:10.10.2

    RUN mkdir /test

запускаю командой "docker compose up"

Скриншот 3 - запускаем новое окно терминала и командой "docker ps" смотрим запущенные контейнеры

Скриншот 4 - через веб-браузер по адресу "localhost:6030" заходим на страницу входа на Adminer

Скриншот 5 - попадаем в систему упраления базой данный MySQL / MariaDB
