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


