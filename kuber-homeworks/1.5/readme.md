# Домашнее задание к занятию «Сетевое взаимодействие в K8S. Часть 2»

### Цель задания

В тестовой среде Kubernetes необходимо обеспечить доступ к двум приложениям снаружи кластера по разным путям.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://microk8s.io/docs/getting-started) по установке MicroK8S.
2. [Описание](https://kubernetes.io/docs/concepts/services-networking/service/) Service.
3. [Описание](https://kubernetes.io/docs/concepts/services-networking/ingress/) Ingress.
4. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### Задание 1. Создать Deployment приложений backend и frontend

1. Создать Deployment приложения _frontend_ из образа nginx с количеством реплик 3 шт.

[frontend](frontend.yaml)

![Alt text](image.png)

2. Создать Deployment приложения _backend_ из образа multitool. 

[backend](backend.yaml)

![Alt text](image-1.png)

3. Добавить Service, которые обеспечат доступ к обоим приложениям внутри кластера. 

[service-backend](service-backend.yaml)

[service-frontend](service-frontend.yaml)

![Alt text](image-2.png)

4. Продемонстрировать, что приложения видят друг друга с помощью Service.

![Alt text](image-3.png)
![Alt text](image-4.png)

------

### Задание 2. Создать Ingress и обеспечить доступ к приложениям снаружи кластера

1. Включить Ingress-controller в MicroK8S.

![Alt text](image-5.png)

2. Создать Ingress, обеспечивающий доступ снаружи по IP-адресу кластера MicroK8S так, чтобы при запросе только по адресу 
открывался _frontend_ а при добавлении /api - _backend_.

[ingress](ingress.yaml)

![Alt text](image-6.png)

3. Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера.

front

![Alt text](image-7.png)

back

![Alt text](image-8.png)

------

### Правила приема работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl` и скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------