# Лабораторная работа: Docker и Docker Compose

Отчет по выполнению лабораторной работы с пошаговым описанием действий и местами для скриншотов.

---

## Задача 1. Установка Docker и Docker Compose, создание кастомного образа Nginx

### Шаг 1. Установка Docker и Docker Compose

sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
docker --version
docker compose version

<img width="640" height="515" alt="Dockerfile" src="https://github.com/user-attachments/assets/652691db-6540-471d-a2d8-85f4e2533f2f" />

https://hub.docker.com/repository/docker/xanhashdocker/custom-nginx/general

### Шаг 2. Настройка зеркал Docker (если Docker Hub недоступен)
---
Создание файла /etc/docker/daemon.json:

<img width="452" height="176" alt="curl" src="https://github.com/user-attachments/assets/af8d723c-0e5c-481b-9e47-7396114164e6" />

<img width="1427" height="76" alt="Переименованнй докер" src="https://github.com/user-attachments/assets/20d14038-7be3-4fb0-b429-0e63736316d3" />



---
### Шаг 3. Регистрация и создание репозитория на Docker Hub

    Зарегистрировался на https://hub.docker.com

    Создал публичный репозиторий с именем custom-nginx.

<img width="728" height="320" alt="3 1" src="https://github.com/user-attachments/assets/12275190-aceb-44f3-87d5-f19bb7c93aed" />

<img width="1386" height="55" alt="3 1 1" src="https://github.com/user-attachments/assets/33e144d7-7992-436d-88d8-ca4a45556615" />


---
### Шаг 4. Скачивание образа Nginx
docker pull nginx:1.21.1
docker images

<img width="707" height="242" alt="При измененном порте" src="https://github.com/user-attachments/assets/57431d0b-b0b4-4dce-8f79-221cb5236e57" />
<img width="986" height="171" alt="Проблема 80 порта" src="https://github.com/user-attachments/assets/f3369c5e-f587-442d-8898-8bdfe4f85afe" />
<img width="1607" height="242" alt="допзадание" src="https://github.com/user-attachments/assets/62fa73da-240c-47e2-8fdc-caeef8d2efe4" />

---
### Шаг 5. Создание Dockerfile и index.html

#### Файл index.html
<html>
<head>
Hey, ZGU!
</head>
<body>
<p>I will be IT Engineer!</p>
</body>
</html>
#### Файл Dockerfile
FROM nginx:1.21.1
COPY index.html /usr/share/nginx/html/index.html



---
### Шаг 6. Сборка и отправка образа на Docker Hub
docker build -t wartabas/custom-nginx:1.0.0 .
docker login
docker push wartabas/custom-nginx:1.0.0
---

---

## Задача 2. Запуск контейнера custom-nginx:1.0.0

### 1. Запуск контейнера

Запускаю контейнер из своего образа custom-nginx:1.0.0:

docker run -d --name "NavruzovGamzabegZarikovich-custom-nginx-t2" -p 127.0.0.1:8080:80 custom-nginx:1.0.0

<img src="images/FIO.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

Далее идет переименование контейнера
docker rename "NavruzovGamzabegZarikovich-custom-nginx-t2" custom-nginx-t2
<img src="images/rename.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

Выполнение комплексной команды
date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080 ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
<img src="images/complex.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

Провекра доступности сайта
curl http://127.0.0.1:8080
<img src="images/firstcurl.png" alt="Скриншот с заданными размерами" width="1000" height="auto">


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
