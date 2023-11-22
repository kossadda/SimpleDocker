# Simple Docker

Введение в докер. Разработка простого докер образа для собственного сервера.


## Оглавление

1. [Part 1. Инструмент ipcalc](#part-1-готовый-докер) <br>





## Part 1. Готовый докер

- Взять официальный докер образ с **nginx** и выкачать его при помощи `docker pull` <br>

`docker pull nginx` <br>
<img src="../misc/images/1_1.jpg" alt="1_1" /> <br>

- Проверить наличие докер образа через `docker images` <br>

`docker images` <br>
<img src="../misc/images/1_2.jpg" alt="1_2" /> <br>

- Запустить докер образ через `docker run -d [image_id|repository]` <br>

`docker run -d --name my_nginx nginx` <br>
<img src="../misc/images/1_3.jpg" alt="1_3" />

- Проверить, что образ запустился через `docker ps` <br>

`docker ps` <br>
<img src="../misc/images/1_4.jpg" alt="1_4" />

- Посмотреть информацию о контейнере через `docker inspect [container_id|container_name]` <br>

`docker inspect my_nginx` <br>
<img src="../misc/images/1_5.jpg" alt="1_5" />

- По выводу команды определить размер контейнера, список замапленных портов и ip контейнера <br>

1) Размер контейнера:

<img src="../misc/images/1_6.jpg" alt="1_6" />

2) Список замапленных портов:

<img src="../misc/images/1_7.jpg" alt="1_7" />

3) IP контейнера:

<img src="../misc/images/1_8.jpg" alt="1_8" />

- Остановить докер образ через `docker stop [container_id|container_name]` <br>

`docker stop my_nginx` <br>
<img src="../misc/images/1_9.jpg" alt="1_9" />

- Проверить, что образ остановился через `docker ps` <br>

`docker ps -a` <br>
<img src="../misc/images/1_10.jpg" alt="1_10" />

- Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run* <br>

`sudo docker run -d -p 80:80 -p 443:443 nginx` <br>
<img src="../misc/images/1_11.jpg" alt="1_11" />

- Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx** <br>

<img src="../misc/images/1_12.jpg" alt="1_12" />

- Перезапустить докер контейнер через `docker restart [container_id|container_name]` <br>

`docker restart 7baedf41265a` <br>
<img src="../misc/images/1_13.jpg" alt="1_13" />

- Проверить любым способом, что контейнер запустился <br>

`docker ps` <br>
<img src="../misc/images/1_14.jpg" alt="1_14" />