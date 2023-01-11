# Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."

## Общая информация

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4113c

Author: Koshevskii Lev Sergeevich

Lab: Lab1

Date of create: 11.01.2023

Date of finished: 

## Ход работы

### Запуск кластера minikube

Для создания кластера minikube была использована следующая команда:
![image_2022_11_20T16_59_50_994Z](https://user-images.githubusercontent.com/46699832/211772331-316c820c-7ea2-41e5-921a-c5d4c8414e65.png)

Данная команда использует Docker в качестве драйвера для запуска Kubernetes кластера. После создания кластера, необходимо проверить возможность подключения к нему.

Получим все ресурсы созданного кластера:

![image_2022_11_20T17_00_16_621Z](https://user-images.githubusercontent.com/46699832/211772369-9e1dfa9a-aee2-40bc-8603-a3fa465f9d87.png)

### Запуск пода в Kubernetes кластере

После запуска кластера и проверки подключения, был создан манифест pod.yaml. Для развертывания пода была использована следущая команда:

![image_2022_11_20T17_06_07_907Z](https://user-images.githubusercontent.com/46699832/211772582-201f8561-092e-4584-9765-0f692632cafd.png)

Убедимся, что pod стартовал успешно:

![image_2022_11_20T17_06_36_262Z](https://user-images.githubusercontent.com/46699832/211772731-56addddc-dd95-47a8-8464-b633d1f1a9ce.png)

### Создание сервиса

Для обеспечения доступа к поду, был создан сервис vault типа NodePort:

![image_2022_11_20T17_07_16_254Z](https://user-images.githubusercontent.com/46699832/211773668-795fb628-3b9e-44b9-b215-af855e39fc43.png)

Для получения созданного сервиса выполним следующую команду:

![image_2022_11_20T17_07_43_331Z](https://user-images.githubusercontent.com/46699832/211773769-600738dc-cf01-4af3-ad3c-8270e65b2362.png)

### Получение доступа к контейнеру

![image_2022_11_20T17_08_09_333Z](https://user-images.githubusercontent.com/46699832/211773936-75f241e2-1ab5-44ec-bb17-3bf2bbf02876.png)

Данная команда пробрасывает порт хоста в контейнер Vault. После выполнения данной команды, приложение будет доступно по адресу `http://localhost:8200`.

![image_2022_11_20T17_08_31_633Z](https://user-images.githubusercontent.com/46699832/211774122-011003b7-6888-4676-94bc-43b9c12065d3.png)

###  Получение токена для доступа в Vault

Получим логи контейнера и отфильтруем их:

![image_2022_11_20T17_09_40_882Z](https://user-images.githubusercontent.com/46699832/211774240-314ba6cd-aea2-43cc-b4da-474e342072a9.png)

Вставим найденный токен в форму авторизации на странице приложения. При успешной авторизации откроется главная страница приложения Vault: 

![image_2022_11_20T17_10_35_176Z](https://user-images.githubusercontent.com/46699832/211774464-0da57f69-ad00-437b-8e73-dc746a2b03e3.png)

### Схема организации контейнеров и сервисов
![image](https://user-images.githubusercontent.com/46699832/211806396-ed15df51-65ec-40ab-9510-62eaa5705fe3.png)
