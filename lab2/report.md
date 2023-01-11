# Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

## Общая информация

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4113c

Author: Koshevskii Lev Sergeevich

Lab: Lab2

Date of create: 11.01.2023

Date of finished: 

## Ход работы

## Создание Deployment

Для развертывания приложения был создан манифест deployment.yaml. Deployment - это абстракция Kubernetes, которая позволяет управлять приложением, его версиями и обновлениями. 
В объекте Deployment хранится информация о конфигурации подов, количестве необходимых реплик и методе обновления подов в случае изменения их конфигурации.

При развертывании deployment создается еще одна абстракция - ReplicaSet. Она отвечает за описание и контроль за несколькими экземплярами приложений.

Для развертывания deployment используем следующую команду:

![image_2022_11_20T17_20_04_001Z](https://user-images.githubusercontent.com/46699832/211776963-93703885-3234-406f-a6cd-a2c446ef067f.png)

## Создание Service типа LoadBalancer

Для предоставления доступа к подам деплоймента lab2-app был создан манифест service.yaml. Данный манифест определяет Service типа LoadBalancer и выбирает поды, которые имеют selectorLabel app=lab2-app. 

Сервисы в Kubernetes позволяют определить набор подов и правила доступа к ним. С помощью сервисов разные части приложения могут "общаться" друг с другом (например, фронтенд с бэкендом).

Для создания описанного в манифесте сервиса была использована следующая команда:

![image_2022_11_20T17_22_26_292Z](https://user-images.githubusercontent.com/46699832/211777474-187a38b2-6d92-49f6-94d7-095c0027a26f.png)

Для того, чтобы определить правильно ли настроены селекторы для подов, выведем описание созданного сервиса:

![image_2022_11_20T17_23_32_215Z](https://user-images.githubusercontent.com/46699832/211777587-b6eaa326-b9c2-4d25-9874-5cef5c22074a.png)

В строке Enpoints приведены IP адреса подов, на которые сервис будет проксировать трафик. Сравним их с IP-адресам подов деплоймента lab2-app:

![image_2022_11_20T17_24_48_741Z](https://user-images.githubusercontent.com/46699832/211777719-867f31cd-e7e0-4e56-bb76-b3d55296c1dd.png)

Как видно из вывода - IP-адреса совпадают.

## Тестирование работы сервиса

![image_2022_11_20T17_25_26_021Z](https://user-images.githubusercontent.com/46699832/211777904-0f04e2a7-b90a-423a-ae79-5f5283a56c1f.png)

Данная команда дает возможность получить доступ к сервису типа LoadBalancer на хосте. Используем ее для тестирования нашего приложения.

После выполнения данной команды откроем в браузере страницу http://localhost:3000 и обновим несколько раз.

![image_2022_11_20T17_25_46_927Z](https://user-images.githubusercontent.com/46699832/211778088-a32fdca6-4e19-440d-8c3c-8c68e31b20f6.png)

![image_2022_11_20T17_26_22_494Z](https://user-images.githubusercontent.com/46699832/211778116-9d3f8fd7-8b6c-40d4-abfd-2bf66810970c.png)

Как видно на рисунках, имя и IP контейнера изменяется. Это происходит потому, что сервис проксирует запросы то на один под, то на другой.

## Логи контейнеров

Для получения логов контейнера в Kubernetes используется следующая команда:

![image_2022_11_20T17_27_22_426Z](https://user-images.githubusercontent.com/46699832/211778407-09d1e889-7458-44b8-8810-380b4bb5adf1.png)

![image_2022_11_20T17_27_59_465Z](https://user-images.githubusercontent.com/46699832/211778427-5eb5c4a2-8a7b-4dee-9afd-beed66dd6c03.png)


## Схема организации контейнеров и сервисов

![image](https://user-images.githubusercontent.com/46699832/211806824-f32d69a4-994b-471a-a41b-d325516daa5f.png)
