![[APACHE KAFKA (1).png]]

# Apache Kafka

### Работа с Kafka в Docker

```bash
# Запуск без внешнего порта
docker run --detach --rm --name kafka confluentinc/confluent-local
```

```bash
# с внешним портом
docker run --detach --rm --name kafka --publish 9092:9092 confluentinc/confluent-local
```

```bash
# Подключение к контейнеру
docker exec -it kafka bash
```

```bash
# Создание топика с помощью скрипта kafka-topics
/bin/kafka-topics --bootstrap-server localhost:9092 --create --topic example-topic
```

```bash
# Отправка сообщений с помощью kafka-console-producer
/bin/kafka-console-producer --bootstrap-server localhost:9092 --topic example-topic
```

```bash
# Чтение сообщений с помощью kafka-console-consumer
/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic example-topic --from-beginning
```

```bash
# Остановка Kafka-контейнера с помощью docker stop kafka
docker stop kafka
```

```bash
# Удаление контейнера
docker rm kafka
```

```bash
# Настройка топика с помощью консольного клиента
$ kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic example-topic
```

