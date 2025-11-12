# Домашнее задание к занятию «Организация сети» - Лепишин Алексей

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

### Решение

1. После применения командой ```terraform apply``` создаются VPC, route table, и 3 VM (public-vm, private-vm, nat-instance):

![1.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/1.1.png?raw=true)

![1.2.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/1.2.png?raw=true)

![1.3.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/1.3.png?raw=true)

![1.4.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/1.4.png?raw=true)

2. Проверяем подключение к public-vm ```ssh ubuntu@89.169.155.206``` и проверяем доступ в интернет:

![2.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/2.1.png?raw=true)

3. Для того чтобы с public-vm подключиться к private-vm по ее локальному IP используем ProxyJump, подключаемся к privat-vm напрямую с моего PC (ключ -J) командой ```ssh -J ubuntu@89.169.155.206 ubuntu@192.168.20.24``` и проверяем с нее доступ в интернет:

![3.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/3.1.png?raw=true)

4. Также можно убедиться по трассировке, что трафик идет через NAT-инстанс с IP 192.168.10.254:

![4.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/4.1.png?raw=true)

5. Проверяем, что у private-vm внешний IP соответствует NAT-инстансу:

![5.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/5.1.png?raw=true)

6. Файлы *.tf закоммичены, кроме terraform.tfvars, где содержатся переменные для подключения cloud_id, folder_id и ssh_pub_key.

![6.1.png](https://github.com/Liberaty/rp_hw_01/blob/main/img/6.1.png?raw=true)

### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.