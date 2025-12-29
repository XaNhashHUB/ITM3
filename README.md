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


 
После выполнения:
docker attach custom-nginx-t2
<img width="728" height="320" alt="3 1" src="https://github.com/user-attachments/assets/20110bac-fa9d-46d0-8df7-4c344bca1135" />

Я нажимаю Ctrl-C.
Контейнер останавливается.
<img width="1386" height="55" alt="3 1 1" src="https://github.com/user-attachments/assets/ef2c8e20-bf0f-4381-b7ad-ec063c3c6755" />

#### Проверка состояния контейнера
docker ps -a

#### Перезапуск контейнера
docker start custom-nginx-t2


#### Заходим в контейнер в интерактивном режиме
docker exec -it custom-nginx-t2 /bin/bash

#### Редактируем конфиг nginx

#### Перезагружаем nginx внутри контейнера
nginx -s reload
Проверяю доступность портов внутри контейнера
curl http://127.0.0.1:80
curl http://127.0.0.1:81
<img width="707" height="242" alt="При измененном порте" src="https://github.com/user-attachments/assets/5c61ed43-ddcb-4b66-84eb-94d976613e37" />


#### Удаление работающего контейнера, не останавливая его
docker rm -f custom-nginx-t2


## Задача 4. Работа с общими томами между контейнерами
<img width="1607" height="242" alt="допзадание" src="https://github.com/user-attachments/assets/4b532dfa-3195-4fe7-a1d9-9c609f6107fb" />


### 1. Запускаю первый контейнер (CentOS) с примонтированной директорией

Текущая директория хоста монтируется в /data контейнера:

docker run -d --name centos-container -v "$(pwd):/data" alpine:latest tail -f /dev/null

### 2. Запускаю второй контейнер (Debian)
docker run -d --name debian-container -v "$(pwd):/data" alpine:latest tail -f /dev/null

<img width="1064" height="174" alt="образы" src="https://github.com/user-attachments/assets/a763c6f9-e438-4099-9ca7-3e9859f3655c" />

### Подключаюсь к первому контейнеру (CentOS) и создаю файл
docker exec -it z4-centos bash
docker exec centos-container sh -c "echo 'File created FROM CentOS container' > /data/centos_file.txt"

### Добавляю файл на хостовой машине (в $(pwd))

echo "File created ON Host machine" > host_file.txt


### Подключаюсь ко второму контейнеру (Debian) и проверяю содержимое /data
docker exec debian-container sh -c "echo '=== LISTING /data ===' && ls -la /data && echo '=== CONTENT ===' && cat /data/*.txt"

![photo_2025-12-29_14-42-40](https://github.com/user-attachments/assets/92c6c31b-a191-4f54-afc3-657816548a11)


## Задача 5. Работа с Portainer, локальным Registry и Compose Includes

---

### Шаг 1. Создаю директорию и два compose-файла

mkdir -p /tmp/ZGU/docker/task
cd /tmp/ZGU/docker/task

Создаю файл compose.yaml
Создаю файл docker-compose.yaml

![два файла](https://github.com/user-attachments/assets/8d383647-8533-4389-87a3-d9ecd6f55a4b)

docker compose up -d
### Изменяю compose.yaml так, чтобы запускались оба файла
<img width="2302" height="957" alt="1" src="https://github.com/user-attachments/assets/c739b956-0da4-4faa-a107-7bc3cb64cd8b" />



### Настройка Portainer
https://127.0.0.1:9000
Прохожу первичную настройку:
задаю логин и пароль администратора.

### Шаг 5. Деплой стека через web-editor Portainer
![12](https://github.com/user-attachments/assets/ebbfddd6-5fac-468c-9f54-18a20b334491)
![13](https://github.com/user-attachments/assets/0a5575ae-b3b0-4e92-86b3-905d675a2302)
![14](https://github.com/user-attachments/assets/75efa5b3-00b1-4e21-b574-f749a92bbbfd)


