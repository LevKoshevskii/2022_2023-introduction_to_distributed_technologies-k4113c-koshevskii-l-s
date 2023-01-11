# Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."

## Общая информация

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4113c

Author: Koshevskii Lev Sergeevich

Lab: Lab3

Date of create: 11.01.2023

Date of finished: 

## Ход работы

### Создание ConfigMap

ConfigMap позволят определять конфигурационные файлы и переменные, которые потом могут использоваться контейнерами с приложениями. Таким образом, ConfigMap избавляет от необходимости упаковывать конфиги в docker-образ.

Манифест configMap для приложения находится в файле configmap.yaml. 

Создание ConfigMap:

```bash
kubectl apply -f C:\Users\koshe\Desktop\lab3\configmap.yaml
```

### Передача значений ConfigMap в ReplicaSet

Для развертывания приложения был создан манифест replicaset.yaml. Для передачи значений ConfigMap в контейнер в качестве переменных окружений используется поле envFrom с указанием имени созданного ConfigMap.

Также был создан манифест service.yaml, описывающий сервис типа ClusterIP для доступа к подам приложения lab3-app.

![image_2022_12_07T18_41_50_247Z (1)](https://user-images.githubusercontent.com/46699832/211781366-a983051b-4652-4060-98e5-85f76c40bfcd.png)

### Развертывание ingress-controller и создание TLS сертификата

Ingress Controller представляет собой специальный балансировщик нагрузки для Kubernetes. Ingress controller позволяет принимать траффик извне кластера и направлять его на поды. 

В данной работе был развернут Nginx Ingress controller:

![image_2022_12_07T18_42_33_107Z](https://user-images.githubusercontent.com/46699832/211781614-1f0809be-a64b-4987-8bb4-885e68959107.png)

После развертывания ingress controller в неймспейсе ingress-nginx появился сервис типа LoadBalancer - `ingress-nginx-controller`. 
Именно через этот сервис будет проходить входящий траффик. Траффик будет распределяться на поды, согласно правилам, которые будут описаны объектами Ingress.

![image_2022_12_07T18_44_10_774Z](https://user-images.githubusercontent.com/46699832/211781857-2d2303d1-9503-4b46-9290-17c7e968f78a.png)

Для генерации TLS-сертификата была использована утилита mkcert:

![image](https://user-images.githubusercontent.com/46699832/211782026-120f84aa-98a9-4fe2-8fc6-0538766ea7e4.png)

Далее TLS-сертификат необходимо импортировать в кластер в виде секрета (Secret): 

![image](https://user-images.githubusercontent.com/46699832/211782075-8dbb35a3-b578-4489-9436-e5446d86cc46.png)

### Создание Ingress в minikube

В файле ingress.yaml приведен манифест Ingress, который описывает dns-имена и правила распределения траффика на поды.

![image](https://user-images.githubusercontent.com/46699832/211782278-e394c1f1-4a4f-4ed5-8822-90d96f088b7a.png)

### Получение доступа к приложению через FQDN

Для получения доступа к сервису ingress-nginx-controller необходимо запустить туннель minikube.

Чтобы приложение lab3-app было доступно по FQDN на хосте, необходимо внести изменения в файл hosts, добавив следующую строку:

![image](https://user-images.githubusercontent.com/46699832/211782662-30d0a723-4378-426c-bbc9-59e4150920e2.png)

После этого приложение может быть открыто в браузере по адресу https://lab3-app.lev.com/

Сертификат:

![image](https://user-images.githubusercontent.com/46699832/211782804-57094b57-2171-4679-bb5b-289c3fac70ab.png)

### Схема организации контейнеров и сервисов


![image](https://user-images.githubusercontent.com/46699832/211807040-3aefde14-1a54-44a1-a30a-0fdf9d751847.png)
