# Simple Docker

Введение в докер. Разработка простого докер образа для собственного сервера.


## Оглавление

1. [Part 1. Готовый докер](#part-1-готовый-докер) <br>
2. [Part 2. Операции с контейнером](#part-2-операции-с-контейнером) <br>

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