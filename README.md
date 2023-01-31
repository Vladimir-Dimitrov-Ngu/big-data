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

#### Далее подключаемся к Proxy машине

1. Берем его публичный IP адрес
2. Заходим в термина, там вводим следующую команду: ssh yc-user@__ip__
3. 
