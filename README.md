# Big Data

## Cloud technologies for big data

To connect the server (через консоль), you need to do:

1. Установить клиент: `curl https://storage.yandexcloud.net/yandexcloud-yc/install.sh | bash`

2. Авторизоваться: `yc init`

### Настройка сети

Для создания сети:

1. Переходим в `Virtual Private Cloud` ( на сервере) и создаем сети (с подсетями)
2. Там же создаем шлюзы
3. Там же создаем таблицу маршрутизации. В нем обязательно нужно добавить маршрут (в нем выбрать шлюз).
4. В подсетях для ru-central-a привязываем таблицу маршрутизации

### Настройка хранилища (Object Storage)
Для создания хранилища:
1. Настроить нужно только опцию публичного доступа.

### Создание кластера (Data Proc)
Для создания кластера нам потребуются следующие настройки (всех их можно примененить в Data Proc)
1. Версия 2.0
2. убираем TEZ
3. добавляем LIVY для удобного обращения к Spark
4. Вставить публичный ssh ключ (....pub)
5. Выбираем сервисный аккаунт
6. Зона доступности - central1-a
7. Добавляем бакет
8. Добавляем сеть
9. Добавляем UI Proxy
10. С мощностями кластера можно выбирать опционально, исходя из задачи. Я использую следующие настройки:
* Master Node. Выбираю network-hdd, память выбираю побольше, так как Master отвечает за отправку данных, а последних может быть довольно *много*
* Data. 2 хоста, network-hdd

### Создание Proxy машины

Для того чтобы мы могли обращатсья к ВМ с помощью терминал, нам потребуется Proxy машина.

1. Заходим в Сompute Cloud и нажимаем `Создать ВМ`
2. Зона доступности 1-а
3. Выбираем Ubuntu
4. HHD с 15 ГБ
5. Выбираем платформу Intel Cascade Lake и устанавливаем 5 процентов гарантированной доли
6. RAM - 1 ГБ
7. Подсеть 1-a
8. Сервисный аккаунт 
9. Устанавлиаем логин yc-user
10. Добавляем публичный ssh ключ

### Далее подключаемся к Proxy машине

1. Берем его публичный IP адрес
2. Заходим в термина, там вводим следующую команду: `ssh yc-user@__ip__`

Проверим, действительно ли мы подключились к кластеру (с помощью вызова Master Node):
1. **Узнать какая машина главная (Master Node).** Заходим в Data Proc, заходим в кластер, далее кликаем хосты. Выбираем хост с Master Node.
2. Берем его _Внутренний FQDN _.
3. В консоли прописываем команду: `ping you_FQDN`

Быстрый способ подключиться: `ssh -A -J yc-user@IP_PROXY ubuntu@FQDN`

Проверка: `hadoop version`, должен вывести верси: 3.2.2 (на состояние 31.01.2023)

### Автоматическое подключение через Proxy

`nano /.ssh/config`

![image](https://user-images.githubusercontent.com/84929000/215666714-890d9711-e2f2-476a-9d47-91b22f9a867d.png)

Теперь можно подключаться так: `ssh lsml-head`

### Работа с Object Storage

Для подключения Object Storage: hdfs dfs -ls s3a://backet_name/

Для передачи в Object Storage:  hdfs dfs -put file.txt s3a://backet_name/file.txt

### Настраиваем Jupiter Notebook

Для более удобной работы нам потребуется Jupiter. 

1. На головной машине устанавливаем Jupiter Notebook: `pip3 install notebook`
2. Для того, чтобы во время запуска кластера запускался и jupiter, необходимо прописать следующую команду:

`sudo nano /etc/systemd/system/jupyter.service`

3. И вставляем туда следующую конфигурацию: 

![image](https://user-images.githubusercontent.com/84929000/215673655-1a7320e7-9e0a-4f02-ad3b-2685b3c69bca.png)

4. Проверка: `cat /etc/systemd/system/jupyter.service`
5. Запускаем наш  сервер:  

`sudo systemctl enable jupyter.service`

`sudo systemctl start jupyter.service`

6. Проверка: systemctl status jupyter.service
7. В конфиге для более удобного доступа, следует прописать: 

![image](https://user-images.githubusercontent.com/84929000/215674191-1f74f899-c662-4f65-96b3-d9968a797b42.png)

8. Для запуска: `ssh -N lsml-proxy`
