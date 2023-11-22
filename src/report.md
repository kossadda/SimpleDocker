# Simple Docker

Введение в докер. Разработка простого докер образа для собственного сервера.


## Оглавление

1. [Part 1. Готовый докер](#part-1-готовый-докер)
2. [Part 2. Операции с контейнером](#part-2-операции-с-контейнером)
3. [Part 3. Мини веб-сервер](#part-3-мини-веб-сервер)
4. [Part 4. Свой докер](#part-4-свой-докер)
5. [Part 5. **Dockle**](#part-5-dockle)
6. [Part 6. Базовый **Docker Compose**](#part-6-базовый-docker-compose)

## Part 1. Готовый докер

- Взять официальный докер образ с **nginx** и выкачать его при помощи `docker pull` <br>

`docker pull nginx` <br>
<img src="../misc/images/part_1/1.jpg" alt="1_1" /> <br>

- Проверить наличие докер образа через `docker images` <br>

`docker images` <br>
<img src="../misc/images/part_1/2.jpg" alt="1_2" /> <br>

- Запустить докер образ через `docker run -d [image_id|repository]` <br>

`docker run -d --name my_nginx nginx` <br>
<img src="../misc/images/part_1/3.jpg" alt="1_3" />

- Проверить, что образ запустился через `docker ps` <br>

`docker ps` <br>
<img src="../misc/images/part_1/4.jpg" alt="1_4" />

- Посмотреть информацию о контейнере через `docker inspect [container_id|container_name]` <br>

`docker inspect my_nginx` <br>
<img src="../misc/images/part_1/5.jpg" alt="1_5" />

- По выводу команды определить размер контейнера, список замапленных портов и ip контейнера <br>

1) Размер контейнера:

<img src="../misc/images/part_1/6.jpg" alt="1_6" />

2) Список замапленных портов:

<img src="../misc/images/part_1/7.jpg" alt="1_7" />

3) IP контейнера:

<img src="../misc/images/part_1/8.jpg" alt="1_8" />

- Остановить докер образ через `docker stop [container_id|container_name]` <br>

`docker stop my_nginx` <br>
<img src="../misc/images/part_1/9.jpg" alt="1_9" />

- Проверить, что образ остановился через `docker ps` <br>

`docker ps -a` <br>
<img src="../misc/images/part_1/10.jpg" alt="1_10" />

- Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run* <br>

`sudo docker run -d -p 80:80 -p 443:443 nginx` <br>
<img src="../misc/images/part_1/11.jpg" alt="1_11" />

- Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx** <br>

<img src="../misc/images/part_1/12.jpg" alt="1_12" />

- Перезапустить докер контейнер через `docker restart [container_id|container_name]` <br>

`docker restart 7baedf41265a` <br>
<img src="../misc/images/part_1/13.jpg" alt="1_13" />

- Проверить любым способом, что контейнер запустился <br>

`docker ps` <br>
<img src="../misc/images/part_1/14.jpg" alt="1_14" />


## Part 2. Операции с контейнером

- Прочитать конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec* <br>

`docker run -d -p 80:80 --name part2 nginx` <br>
`docker exec part2 cat /etc/nginx/nginx.conf` <br>
<img src="../misc/images/part_2/1.jpg" alt="2_1" /> <br>

- Создать на локальной машине файл *nginx.conf* <br>

`touch nginx.conf` <br>
<img src="../misc/images/part_2/2.jpg" alt="2_2" /> <br>

- Настроить в нем по пути */status* отдачу страницы статуса сервера **nginx** <br>

`vim nginx.conf` <br>
<img src="../misc/images/part_2/3.jpg" alt="2_3" /> <br>

- Скопировать созданный файл *nginx.conf* внутрь докер образа через команду `docker cp` <br>

`docker cp nginx.conf part2:/etc/nginx/` <br>
<img src="../misc/images/part_2/4.jpg" alt="2_4" /> <br>

- Перезапустить **nginx** внутри докер образа через команду *exec* <br>

`docker exec part2 nginx -s reload` <br>
<img src="../misc/images/part_2/5.jpg" alt="2_5" /> <br>

- Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx** <br>

`curl localhost:80/status` <br>
<img src="../misc/images/part_2/6.jpg" alt="2_6" /> <br>

- Экспортировать контейнер в файл *container.tar* через команду *export* <br>

`docker export part2 > container.tar` <br>
<img src="../misc/images/part_2/7.jpg" alt="2_7" /> <br>

- Остановить контейнер <br>

`docker stop part2` <br>
<img src="../misc/images/part_2/8.jpg" alt="2_8" /> <br>

- Удалить образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры <br>

`docker rmi nginx -f` <br>
`docker images` <br>
<img src="../misc/images/part_2/9.jpg" alt="2_9" /> <br>

- Удалить остановленный контейнер <br>

`docker rm part2` <br>
`docker ps -a` <br>
<img src="../misc/images/part_2/10.jpg" alt="2_10" /> <br>

- Импортировать контейнер обратно через команду *import* <br>

`sudo docker import -c 'cmd ["nginx", "-g", "daemon off;"]' -c 'ENTRYPOINT ["/docker-entrypoint.sh"]' container.tar import_nginx` <br>
`docker images` <br>
<img src="../misc/images/part_2/11.jpg" alt="2_11" /> <br>

- Запустить импортированный контейнер <br>

`docker run -d -p 80:80 --name import_part2 import_nginx` <br>
<img src="../misc/images/part_2/12.jpg" alt="2_12" /> <br>

- Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx** <br>

`curl localhost:80/status` <br>
<img src="../misc/images/part_2/13.jpg" alt="2_13" /> <br>

## Part 3. Мини веб-сервер

- Написать мини сервер на **C** и **FastCgi**, который будет возвращать простейшую страничку с надписью `Hello World!` <br>

`vim miniserver.c` <br>
<img src="../misc/images/part_3/1.jpg" alt="3_1" /> <br>

- Написать свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080* <br>

`vim nginx.conf` <br>
<img src="../misc/images/part_3/2.jpg" alt="3_2" /> <br>

- Запустить написанный мини сервер через *spawn-fcgi* на порту 8080 <br>

1) Загрузить заново образ с **nginx**:

`docker pull nginx` <br>
<img src="../misc/images/part_3/3.jpg" alt="3_3" /> <br>

2) Проверить наличие образа **nginx**:

`docker images` <br>
<img src="../misc/images/part_3/4.jpg" alt="3_4" /> <br>

3) Запустить контейнер с образа **nginx**:

`docker run -d -p 81:81 --name part3 nginx` <br>
<img src="../misc/images/part_3/5.jpg" alt="3_5" /> <br>

4) Проверить наличие контейнера с образа **nginx**:

`docker ps` <br>
<img src="../misc/images/part_3/6.jpg" alt="3_6" /> <br>

5) Скопировать созданные файлы *nginx.conf* и *miniserver.c* внутрь докер образа через команду `docker cp`:

`docker cp nginx.conf part3:/etc/nginx/` <br>
`docker cp miniserver.c part3:/home/` <br>
<img src="../misc/images/part_3/7.jpg" alt="3_7" /> <br>

6) Запустить интерактивную оболочку внутри запущенного докер-контейнера:

`docker exec -it part3 bash` <br>
<img src="../misc/images/part_3/8.jpg" alt="3_8" /> <br>

7) Запустить установку gcc компилятора, утилиты spawn-fcgi и библиотеки libfcgi-dev:

`apt-get update` <br>
`apt-get install gcc` <br>
`apt-get install spawn-fcgi` <br>
`apt-get install libfcgi-dev` <br>
<img src="../misc/images/part_3/9.jpg" alt="3_9" /> <br>

8) Скомпилировать miniserver.c:

`gcc miniserver.c -lfcgi -o mini` <br>
<img src="../misc/images/part_3/10.jpg" alt="3_10" /> <br>

9) Запустить FastCGI-сервера с использованием spawn-fcgi:

`spawn-fcgi -p 8080 mini` <br>
<img src="../misc/images/part_3/11.jpg" alt="3_11" /> <br>

10) Перезагрузить конфигурации nginx:

`nginx -s reload` <br>
<img src="../misc/images/part_3/12.jpg" alt="3_12" /> <br>

- Проверить, что в браузере по *localhost:81* отдается написанная вами страничка <br>

`curl localhost:81` <br>
<img src="../misc/images/part_3/13.jpg" alt="3_13" /> <br>

- Положить файл *nginx.conf* по пути *./nginx/nginx.conf* (это понадобится позже) <br>

`mkdir nginx` <br>
`mv nginx.conf nginx` <br>
<img src="../misc/images/part_3/14.jpg" alt="3_14" /> <br>

## Part 4. Свой докер

**Написать свой докер образ, который:**
1) собирает исходники мини сервера на FastCgi из [Части 3](#part-3-мини-веб-сервер)
2) запускает его на 8080 порту
3) копирует внутрь образа написанный *./nginx/nginx.conf*
4) запускает **nginx**.

>**nginx** можно установить внутрь докера самостоятельно, а можно воспользоваться готовым образом с **nginx**'ом, как базовым.

`vim Dockerfile` <br>
<img src="../misc/images/part_4/1.jpg" alt="4_1" /> <br>

`vim run.sh` <br>
<img src="../misc/images/part_4/2.jpg" alt="4_2" /> <br>

- Собрать написанный докер образ через `docker build` при этом указав имя и тег <br>

`docker build -t part4:1 .` <br>
<img src="../misc/images/part_4/3.jpg" alt="4_3" /> <br>

- Проверить через `docker images`, что все собралось корректно <br>

`docker images` <br>
<img src="../misc/images/part_4/4.jpg" alt="4_4" /> <br>

- Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы **nginx**'а (см. [Часть 2](#part-2-операции-с-контейнером)) <br>

`docker run -it -p 80:81 -v /home/deadline/s21_projects/DO5_SimpleDocker/src/part_4/nginx.conf:/etc/nginx/nginx.conf -d part4:1 bash` <br>
<img src="../misc/images/part_4/5.jpg" alt="4_5" /> <br>

- Проверить, что по localhost:80 доступна страничка написанного мини сервера <br>

`curl localhost` <br>
<img src="../misc/images/part_4/6.jpg" alt="4_6" /> <br>

- Дописать в *./nginx/nginx.conf* проксирование странички */status*, по которой надо отдавать статус сервера **nginx** <br>

`vim nginx.conf` <br>
<img src="../misc/images/part_4/7.jpg" alt="4_7" /> <br>

- Перезапустить докер образ <br>

`sudo docker exec -it epic_bose /bin/bash` <br>
`nginx -s reload` <br>
<img src="../misc/images/part_4/8.jpg" alt="4_8" /> <br>

>*Если всё сделано верно, то, после сохранения файла и перезапуска контейнера, конфигурационный файл внутри докер образа должен обновиться самостоятельно без лишних действий*. <br>

- Проверить, что теперь по *localhost:80/status* отдается страничка со статусом **nginx** <br>

<img src="../misc/images/part_4/9.jpg" alt="4_9" /> <br>

## Part 5. **Dockle**

- Установить dockle (fedora):

`VERSION=$(curl --silent "https://api.github.com/repos/goodwithtech/dockle/releases/latest" | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/') && curl -L -o dockle.rpm https://github.com/goodwithtech/dockle/releases/download/v${VERSION}/dockle_${VERSION}_Linux-64bit.rpm` <br>
`sudo dnf install -y dockle.rpm` <br>

- Просканировать образ из предыдущего задания через `dockle [image_id|repository]` <br>

`dockle -i CIS-DI-0010 part4:1` <br>
<img src="../misc/images/part_5/1.jpg" alt="5_1" /> <br>

- Исправить образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений <br>

`vim Dockerfile` <br>
<img src="../misc/images/part_5/2.jpg" alt="5_2" /> <br>

- Просканировать исправленный образ через `dockle [image_id|repository]` <br>

`dockle -i CIS-DI-0010 part4:part5` <br>
<img src="../misc/images/part_5/3.jpg" alt="5_3" /> <br>

- Проверить, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранее <br>

`curl localhost` <br>
<img src="../misc/images/part_5/4.jpg" alt="5_4" /> <br>

## Part 6. Базовый **Docker Compose**

- Установить docker-compose (fedora):

`sudo dnf install docker-compose` <br>

**Написать файл *docker-compose.yml*, с помощью которого:**

1) Поднять докер контейнер из [Части 5](#part-5-dockle) _(он должен работать в локальной сети, т.е. не нужно использовать инструкцию **EXPOSE** и мапить порты на локальную машину)_
2) Поднять докер контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера
3) Замапить 8080 порт второго контейнера на 80 порт локальной машины

- Остановить все запущенные контейнеры <br>

`docker stop *"process"*` <br>
`docker ps` <br>
<img src="../misc/images/part_6/1.jpg" alt="6_1" /> <br>

- Собрать и запустить проект с помощью команд `docker-compose build` и `docker-compose up` <br>

`sudo docker-compose build` <br>
<img src="../misc/images/part_6/2.jpg" alt="6_2" /> <br>

`sudo docker compose up -d` <br>
<img src="../misc/images/part_6/3.jpg" alt="6_3" /> <br>

- Проверить, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранее <br>

`curl localhost` <br>
<img src="../misc/images/part_6/4.jpg" alt="6_4" /> <br>

[**К оглавлению**](#оглавление) <br>