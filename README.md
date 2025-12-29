# Лабораторная работа: Docker и Docker Compose

Отчет по выполнению лабораторной работы с пошаговым описанием действий и местами для скриншотов.

---

## Задача 1. Установка Docker и Docker Compose, создание кастомного образа Nginx

### Шаг 1. Установка Docker и Docker Compose
---

<img width="1329" height="405" alt="Установленные компоненты" src="https://github.com/user-attachments/assets/2f7c82ad-dd51-4eb1-984e-93ef8885c4b7" />

---
### Шаг 2. Настройка зеркал Docker (если Docker Hub недоступен)
---

    Сайт был доступен, поэтому пропустил
    
---
### Шаг 3. Регистрация и создание репозитория на Docker Hub
---
    Зарегистрировался на https://hub.docker.com

    Создал публичный репозиторий с именем custom-nginx.
<img width="2146" height="1218" alt="image" src="https://github.com/user-attachments/assets/c7ff6296-0c5c-4b77-88ff-7896e913ac65" />

---
### Шаг 4. Скачивание образа Nginx

docker pull nginx:1.21.1

    Скриншот с образом на шаге 1

---
### Шаг 5. Создание Dockerfile и index.html
---

#### Файл Dockerfile
FROM nginx:1.21.1
COPY index.html /usr/share/nginx/html/index.html

#### Файл index.html
<html>
<head>
Hey, ZGU!
</head>
<body>
<p>I will be IT Engineer!</p>
</body>
</html>

    Команды
<img width="1467" height="1125" alt="indexHTML" src="https://github.com/user-attachments/assets/55472880-95c7-43cf-9bff-e82bd79bc125" />

    Файлы
<img width="1149" height="913" alt="папка с файлами" src="https://github.com/user-attachments/assets/a03aea3d-0470-4be3-ac1d-3a1300c228cd" />

---
### Шаг 6. Сборка и отправка образа на Docker Hub
---

docker build -t custom-nginx:1.0.0 .
<img width="1461" height="650" alt="болд докера" src="https://github.com/user-attachments/assets/f8ff67fa-d499-4158-baa0-4855e3cc5ffc" />

docker login
<img width="1447" height="575" alt="логин" src="https://github.com/user-attachments/assets/c8e62339-35ab-42bb-8028-6a77380a9933" />

docker tag custom-nginx:1.0.0 xanhashdocker/custom-nginx:1.0.0
docker push xanhashdocker/custom-nginx:1.0.0

<img width="1834" height="586" alt="пуш докера" src="https://github.com/user-attachments/assets/e588c9d9-825f-47d1-89b2-804d919ade49" />
<img width="2148" height="1400" alt="докерхаб с тэгом" src="https://github.com/user-attachments/assets/351f9fdf-7ad0-4c78-baa2-b3346be0aa69" />

https://hub.docker.com/repository/docker/xanhashdocker/custom-nginx/general

---
## Задача 2. Запуск контейнера custom-nginx:1.0.0

### 1. Запуск контейнера

Запускаю контейнер из своего образа custom-nginx:1.0.0:

docker run -d --name "Budarev-custom-nginx-t2" -p 127.0.0.1:8080:80 custom-nginx:1.0.0
<img width="1928" height="73" alt="имя" src="https://github.com/user-attachments/assets/0d706412-ae9d-4dc6-80f3-976993069b0b" />


Далее идет переименование контейнера

docker rename "Budarev-custom-nginx-t2" custom-nginx-t2
<img width="2493" height="268" alt="переименование" src="https://github.com/user-attachments/assets/0bc19898-f058-4730-bee3-7d5a51193c2e" />


Провекра доступности сайта

curl http://127.0.0.1:8080
<img width="881" height="310" alt="image" src="https://github.com/user-attachments/assets/027eaaa5-ea4d-425e-bf28-690fb0d3e1a4" />


## Задача 3. Работа со стандартными потоками, конфигурацией Nginx и портами

### 1. Узнаём, как подключиться к STDIN/STDOUT/STDERR контейнера

Использую команду:

docker attach custom-nginx-t2
<img src="images/attach.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

 
После выполнения:
docker attach custom-nginx-t2

Я нажимаю Ctrl-C.
Контейнер останавливается.

#### Проверка состояния контейнера
docker ps -a
<img src="images/dockerps.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

#### Перезапуск контейнера
<img src="images/restart.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

#### Заходим в контейнер в интерактивном режиме
<img src="images/exec.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

#### Редактируем конфиг nginx
<img src="images/listen.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

#### Перезагружаем nginx внутри контейнера
nginx -s reload
Проверяю доступность портов внутри контейнера
curl http://127.0.0.1:80
curl http://127.0.0.1:81
<img src="images/proverka.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

#### Удаление работающего контейнера, не останавливая его
docker rm -f custom-nginx-t2
<img src="images/udalit.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

## Задача 4. Работа с общими томами между контейнерами


### 1. Запускаю первый контейнер (CentOS) с примонтированной директорией

Текущая директория хоста монтируется в /data контейнера:

docker run -d --name z4-centos -v $(pwd):/data centos:7 sleep infinity

<img src="images/Centos.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

### 2. Запускаю второй контейнер (Debian)
docker run -d --name z4-debian -v $(pwd):/data debian:latest sleep infinity

<img src="images/debian.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

### Подключаюсь к первому контейнеру (CentOS) и создаю файл
docker exec -it z4-centos bash
echo "Hello from CentOS container!" > /data/from_centos.txt
<img src="images/centosfile.png" alt="Скриншот с заданными размерами" width="1000" height="auto">
### Добавляю файл на хостовой машине (в $(pwd))

echo "Файл с хоста" > host_file.txt

<img src="images/hostfile.png" alt="Скриншот с заданными размерами" width="1000" height="auto">


### Подключаюсь ко второму контейнеру (Debian) и проверяю содержимое /data
<img src="images/proverkaDebian.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

## Задача 5. Работа с Portainer, локальным Registry и Compose Includes

---

### Шаг 1. Создаю директорию и два compose-файла

mkdir -p /tmp/ZGU/docker/task
cd /tmp/ZGU/docker/task

<img src="images/create.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

Создаю файл compose.yaml
Создаю файл docker-compose.yaml

docker compose up -d
### Изменяю compose.yaml так, чтобы запускались оба файла
<img src="images/zapusk.png" alt="Скриншот с заданными размерами" width="1000" height="auto">


<img src="images/twoserv.png" alt="Скриншот с заданными размерами" width="1000" height="auto">


### Настройка Portainer
https://127.0.0.1:9000
Прохожу первичную настройку:
задаю логин и пароль администратора.

### Шаг 5. Деплой стека через web-editor Portainer

<img src="images/Json1.png" alt="Скриншот с заданными размерами" width="1000" height="auto">
<img src="images/Json2.png" alt="Скриншот с заданными размерами" width="1000" height="auto">
