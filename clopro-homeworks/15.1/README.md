# Домашнее задание к занятию «Организация сети»

### Подготовка к выполнению задания

1. Домашнее задание состоит из обязательной части, которую нужно выполнить на провайдере Yandex Cloud, и дополнительной части в AWS (выполняется по желанию). 
2. Все домашние задания в блоке 15 связаны друг с другом и в конце представляют пример законченной инфраструктуры.  
3. Все задания нужно выполнить с помощью Terraform. Результатом выполненного домашнего задания будет код в репозитории. 
4. Перед началом работы настройте доступ к облачным ресурсам из Terraform, используя материалы прошлых лекций и домашнее задание по теме «Облачные провайдеры и синтаксис Terraform». Заранее выберите регион (в случае AWS) и зону.

---
### Задание 1. Yandex Cloud 

**Что нужно сделать**

1. Создать пустую VPC. Выбрать зону.
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 192.168.10.0/24.
 - Создать в этой подсети NAT-инстанс, присвоив ему адрес 192.168.10.254. В качестве image_id использовать fd80mrhj8fl2oe87o4e1.
 - Создать в этой публичной подсети виртуалку с публичным IP, подключиться к ней и убедиться, что есть доступ к интернету.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 192.168.20.0/24.
 - Создать route table. Добавить статический маршрут, направляющий весь исходящий трафик private сети в NAT-инстанс.
 - Создать в этой приватной подсети виртуалку с внутренним IP, подключиться к ней через виртуалку, созданную ранее, и убедиться, что есть доступ к интернету.

Resource Terraform для Yandex Cloud:

- [VPC subnet](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_subnet).
- [Route table](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_route_table).
- [Compute Instance](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_instance).

---
    ### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

### Решение 

* Проверяем работоспособность `yc`:

```bash
yc config list
token: ******
cloud-id: b1g9ihitksj8pk4q71d5
folder-id: b1gpo5h1fdmspnqimpmm
compute-default-zone: ru-central1-b
```

* Получим IAM-токен для работы с Yandex Cloud:

```bash
yc iam create-token
******
```

* Проверим конфигурацию Terraform для созданного файла main.tf
```bash
 ~/Documents/devops-25/clopro-homeworks/15.1  main ?1 ---------------------------------------------------------------------------------------- 18:48:18 
> terraform validate

Success! The configuration is valid.
```

* Подготовим план для Terraform
![alt text](image-1.png)

* Запустим создание ресурсов с помощью Terraform:
```bash
> terraform apply --auto-approve 
```
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-10.png)
![alt text](image-11.png)

* Убедимся, что в сети с именем network-netology созданы подсети public и private (при этом private с route table):
![alt text](image-5.png) 

* Убедимся, что созданы виртуальные машины nat-instance, public-instance и private-instance:
![alt text](image-6.png)

* Подключаемся к публичной машине и проверяем наличие соединения с интернетом
![alt text](image-7.png)
![alt text](image-8.png)
* Находясь на виртуальной машине private-instance, проверим доступ в Интернет
![alt text](image-13.png)

* Виртуальная машина private-instance не имеет внешнего IP-адреса. Подключение к данной виртуальной машине выполним через виртуальную машину public-instance, скопировав на нее приватный ключ ssh:
![alt text](image-14.png)
```bash
> scp /Users/t0hab/.ssh/id_rsa ubuntu@158.160.77.183:/home/ubuntu/.ssh/id_rsa
```

* Находясь на виртуальной машине private-instance, проверим доступ в Интернет:

![alt text](image-15.png)

* Файл [terraform_file](for_netology.tf)