# 5.3
## 1.
Запускаем команду 

        docker login

Далее собираем контейнер

Пишим Dockerfile

        FROM nginx
        COPY index.html /usr/share/nginx/html
Далее

        docker build -t html-server-image:v1 .
        Sending build context to Docker daemon  4.096kB
        Step 1/2 : FROM nginx
        latest: Pulling from library/nginx
Проверяем, запустился ли наш контейнер

        docker images
        REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
        html-server-image   v1        9b98f5c1714f   8 seconds ago   142MB
Запускаем

        docker run -d -p 80:80 html-server-image:v1

        7a70e389e06a3a19827d8cd269e9fd2084dc990d8b46b301f0ba89e6520aae9e

Проверяем работоспособность

        url localhost:80
        <html>
        <head>
        Hey, Netology
        </head>
        <body>
        <h1>I’m DevOps Engineer!</h1>
        </body>
        </html>

Всё ок, теперь необходимо разместить наш образ на докер хабе

        docker tag html-server-image:v1 mshapovalov/5.3_nginx

        docker push mshapovalov/5.3_nginx
        Using default tag: latest

Проверяем, докер образ записался в репозитории

https://hub.docker.com/repository/docker/mshapovalov/5.3_nginx

## 2.

Высоконагруженное монолитное java веб-приложени - я думаю тут подойдет отдельный сервер, так как java сама по себе медленная, + высокая нагрузка. Но надо будет подкрутить безопасность. 
Nodejs веб-приложение - для данного отлично поджойдет докер. Если приложение будет статическим.

Мобильное приложение c версиями для Android и iOS - я думаю подойдет виртуальная машина. Нам же нужно доступ к этому приложению и постоянно его тестить. 

Шина данных на базе Apache Kafka - Для нераспределенных систем характерно наличие так называемой единой точки отказа. Сюда не подойдет докер ни вирт машины, скорее всего нужен отдельный сервер причём не один. Это по факту аналаг Гит-а только для БД. Получится распределенный горизонтально масштабируемый отказоустойчивый журнал коммитов.

Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana по факту это NoSQL-хранилище документов, с возможностью полнотекстового поиска. Logstash настроен на приём логов по TCP/UDP-протоколам, читает сообщения из Redis и сохраняет в Elasticsearch. Kibana предоставляет визуальный интерфейс для поиска и отображения собранных данных. Я думаю это слишком тяжеловесная информация. Отдельный сервер.

Мониторинг-стек на базе Prometheus и Grafana - для данного подойдет Докер, я видел в задание 5.4 будет задание с разварачиванием данного комплекса.

MongoDB, как основное хранилище данных для java-приложения - можно, но не в продакшене. Слишком большая нагрузка.

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry - отдельный сервер. Много важной информации для организации.

## 3.
 
 Создаем контейнер centos

        docker run -it -d -v "/home/mikhail/L/docker/data:/dataM" --name ccc centos:7 bash
далее подключаемся к нему 

        docker exec -it ccc bash
        [root@1364d8aeec6b /]# ll 
        total 60
        -rw-r--r--   1 root root 12114 Nov 13  2020 anaconda-post.log
        lrwxrwxrwx   1 root root     7 Nov 13  2020 bin -> usr/bin
        drwxrwxr-x   2 1000 1000  4096 May 11 08:47 dataM
Далее переходим в папку dataM
и создаём там файл
        vi Proverka_centos
        [root@1364d8aeec6b dataM]# ll
        total 4
        -rw-r--r-- 1 root root 8 May 11 10:31 Proverka_centos
Возращаемся в хостувую машину и смотрим, файл действительно появился. 

        ll data/
        total 12
        drwxrwxr-x 2 mikhail mikhail 4096 мая 11 13:31 ./
        drwxrwxr-x 3 mikhail mikhail 4096 мая 11 11:55 ../
        -rw-r--r-- 1 root    root       8 мая 11 13:31 Proverka_centos
Добавляем файл на хост машине
Создаём аналогичный контейнер, но на дебиане

        docker run -it -d -v "/home/mikhail/L/docker/data:/dataD" --name ddd debian:8 bash
подключаемся к нему

        docker exec -it ddd bash

Проверяем наличие папки dataD

        ls
        bin   dataD  etc   lib	  media  opt   root  sbin  sys	usr
        boot  dev    home  lib64  mnt	 proc  run   srv   tmp	var
Проверяем наличие файлов

        root@6666462dd5c3:/dataD# ls
        Host_m	Proverka_centos
Всё ок
        cat Proverka_centos 


        Hello
