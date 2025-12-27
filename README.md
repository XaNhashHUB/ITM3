# Лабораторная работа: Docker и Docker Compose

Отчет по выполнению лабораторной работы с пошаговым описанием действий и местами для скриншотов.

---

## Задача 1. Установка Docker и Docker Compose, создание кастомного образа Nginx

### Шаг 1. Установка Docker и Docker Compose

<img width="640" height="515" alt="Dockerfile" src="https://github.com/user-attachments/assets/397ca452-326e-48e0-aed5-385a4757fbec" />

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


<img width="452" height="176" alt="curl" src="https://github.com/user-attachments/assets/fe05c367-abe5-4c8a-917f-d6c8b8678ded" />
<img width="1427" height="76" alt="Переименованнй докер" src="https://github.com/user-attachments/assets/66ff23ed-4793-4ef8-998c-c67a9cc6b3b2" />


---
### Шаг 3. Регистрация и создание репозитория на Docker Hub

    Зарегистрировался на https://hub.docker.com

    Создал публичный репозиторий с именем custom-nginx.

<img width="728" height="320" alt="3 1" src="https://github.com/user-attachments/assets/6089445b-2d2e-4668-b52a-394c8b7a6c96" />

<img width="1386" height="55" alt="3 1 1" src="https://github.com/user-attachments/assets/12fa3247-d1fb-4469-8d44-e8e1c68d7501" />

<img width="1607" height="242" alt="допзадание" src="https://github.com/user-attachments/assets/f2f789d2-6a7d-48dd-a5b8-014e8af61b76" />

<img width="707" height="242" alt="При измененном порте" src="https://github.com/user-attachments/assets/c34cf017-f7e6-47c8-ad96-6957513de7a8" />

<img width="986" height="171" alt="Проблема 80 порта" src="https://github.com/user-attachments/assets/0fda7b1e-f825-4a6f-8f23-c4eec567c6c1" />


---
### Шаг 4. Скачивание образа Nginx
docker pull nginx:1.21.1
docker images
<img src="images/nginx.png" alt="Скриншот с заданными размерами" width="1000" height="auto">

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
<img width="1608" height="212" alt="4" src="https://github.com/user-attachments/assets/705f01ac-0a06-4c61-ba53-e98f54e7e303" />
<img width="1122" height="80" alt="образы ОС" src="https://github.com/user-attachments/assets/d12bbcc2-a770-457f-93e9-88a29455fa31" />


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
