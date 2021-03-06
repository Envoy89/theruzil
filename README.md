# Файлы для сайта theruzil.com

Файлы для развертывания nginx, который раздает статику для главной страницы и выступает в роли прокси для нескольких других проектов, а именно: 

- [myworstblog](https://github.com/Envoy89/myworstblog)

## Требования

1. Linux machine
2. [Git](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-20-04-ru) 
3. [Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-ru) и [Docker Compose](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04-ru)

## Первый запуск

1. Скопировать файлы репозитория на машину
    1. mkdir /root/theruzil/
    2. cd /root/
    3. git clone https://github.com/Envoy89/theruzil.git
2. Переименовать nginx.conf в nginx_new.conf и nginx_first.conf в nginx.conf
    1. mv nginx.conf nginx_new.conf
    2. mv nginx_first.conf nginx.conf
3. Запустить все приложения
    1. docker-compose up -d
4. Переименовать nginx_new.conf в nginx.conf
    1. mv nginx.conf nginx_first.conf
    2. mv nginx_new.conf nginx.conf
5. Создать ключ Diffie-Hellman
    1. mkdir /root/theruzil/dhparam/
    2. sudo openssl dhparam -out /root/theruzil/dhparam/dhparam-2048.pem 2048
6. Перезапустить только webserver
    1. docker-compose up -d --force-recreate --no-deps webserver
7. Настроить автообновление сертификатов
    1. chmod +x /root/theruzil/ssl_renew.sh
    2. sudo crontab -e
        1. Добавить строки 0 12 * * * /root/theruzil/ssl_renew.sh >> /var/log/cron.log 2>&1

## Обновление и перезапуск

1. Обновить файлы из репозитория
    1. cd /root/theruzil/
    2. git pull
2. Перезапустить изменившийся контейнер
    1. docker-compose up -d --force-recreate --no-deps "service_name"

## Локальный запуск
1. docker-compose -f docker-compose.local.yml up -d