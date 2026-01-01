# Масштабирование приложений

## Задание 2

Содержание registry, содержащего образ hello-world

![registry-data](./screenshots/task2/screenshot1.png)

## Задание 3

Перед инициализацией registry необходимо добавить созданный сертификат в систему и выбрать опцию Trust All

![add-cert-on-mac](./screenshots/task3/screenshot1.png)

Попытка получить образ hello-world до и после прохождения аутентификации

![pull-hello-world-image](./screenshots/task3/screenshot2.png)

## Задание 4

Все ноды в состоянии Active, команда выполняется на основной ноде

![docker-node-active-1](./screenshots/task4/screensho1.png)

![docker-node-active-2](./screenshots/task4/screenshot2.png)

Перевод второй ноды в состояние Drain

![docker-node-switch-drain](./screenshots/task4/screenshot3.png)

Проверка, что состояние ноды изменилось

![docker-node-drain-1](./screenshots/task4/screenshot4.png)

![docker-node-drain-2](./screenshots/task4/screenshot5.png)

Проверка, что на выведнной из роя ноды нет запущенных контейнеров, команда выполняется данной ноде

![docker-node-drain-empty](./screenshots/task4/screenshot6.png)

После вывода из роя второй ноды, задачи, которые были на ней запущены перераспределяются между оставшимися нодами

![docker-service-status-1](./screenshots/task4/screenshot7.png)

![docker-service-status-2](./screenshots/task4/screenshot8.png)

![docker-service-status-3](./screenshots/task4/screenshot9.png)

Чтобы вернуть ноду в состояние Active, нужно изменить availability следующей командой:

```bash
docker node update --availability active <NODE_ID>
```

Все ноды вновь находятся в состоянии Active

![docker-node-switch-active-1](./screenshots/task4/screenshot10.png)

![docker-node-switch-active-2](./screenshots/task4/screenshot11.png)

При переводе ноды в состояние Drain Swarm останавливает все задачи сервисов на этой ноде перепланирует их на другие ноды. Однако при возврате ноды в состояние Active Swarm не делает обратного ребаланса и не пересоздаёт задачи без явного обновления сервиса.

Проверка, что на второй ноде не запустились остановленные задачи, команда выполняется данной ноде

![docker-node-active-empty](./screenshots/task4/screenshot12.png)

Чтобы на вновь активной ноде запускались задачи необходмо заново их перераспределить:

```bash
docker service update --force <SERVICE_NAME>
```

Если у сервиса указано, что реплик всего 1 и она уже запущена на другой ноде, изменить их колчиество:

```bash
docker service scale <SERVICE_NAME>=2
```

Запускам распределение задач между нодами и убеждаемся, что второй ноде была выделена задача

![docker-service-restart](./screenshots/task4/screenshot13.png)

![docker-node-active-running](./screenshots/task4/screenshot14.png)
