![kafka.png](../files/kafka.png)

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

### Управление
```bash
----------Управление топиками: создание------------
# Создать топик с одним разделом и фактором репликации 1
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic my-topic --partitions 1 --replication-factor 1

# Создать топик с тремя разделами и фактором репликации 3
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic orders --partitions 3 --replication-factor 3

# Создать топик с конфигурацией (время хранения 2 дня)
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic events --partitions 1 --replication-factor 1 --config retention.ms=172800000

----------Управление топиками: просмотр------------
# Показать список всех топиков в кластере
kafka-topics.sh --bootstrap-server localhost:9092 --list

# Показать метаданные конкретного топика
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic my-topic

# Показать только топики, переопределённые в конфигурации
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topics-with-overrides

# Показать только топики с недоступными разделами
kafka-topics.sh --bootstrap-server localhost:9092 --describe --unavailable-partitions

# Показать только топики с недостаточной синхронизацией реплик
kafka-topics.sh --bootstrap-server localhost:9092 --describe --under-replicated-partitions

----------Управление топиками: изменение------------
# Увеличить количество разделов в топике
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic my-topic --partitions 6

# Удалить конфигурацию топика
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic my-topic --delete-config retention.ms

----------Управление топиками: удаление------------
# Удалить топик (требует delete.topic.enable=true)
kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic my-topic

----------Управление топиками: конфигурация------------
# Показать конфигурацию конкретного топика
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type topics --entity-name my-topic --describe

# Изменить конфигурацию топика (установить retention.ms)
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type topics --entity-name my-topic --alter --add-config retention.ms=172800000

# Показать конфигурацию по умолчанию для топиков брокера
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type topics --entity-default --describe

----------Управление топиками: удаление записей------------
# Удалить записи до определённого смещения в топике
kafka-delete-records.sh --bootstrap-server localhost:9092 --offset-json-file offsets.json

# Содержимое offsets.json:
# {
#   "partitions": [
#     {"topic": "my-topic", "partition": 0, "offset": 1000}
#   ],
#   "version": 1
# }

----------Управление брокерами: просмотр------------
# Показать версии API брокеров
kafka-broker-api-versions.sh --bootstrap-server localhost:9092

# Показать список брокеров в кластере
kafka-broker-api-versions.sh --bootstrap-server localhost:9092 | grep "id:"

----------Управление брокерами: конфигурация------------
# Показать конфигурацию конкретного брокера
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type brokers --entity-name 0 --describe

# Показать конфигурацию всех брокеров
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type brokers --entity-default --describe

# Изменить конфигурацию брокера (например, log.retention.hours)
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type brokers --entity-name 0 --alter --add-config log.retention.hours=168

----------Перераспределение разделов------------
# Сгенерировать план перераспределения для топиков на брокеры 5,6
kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --topics-to-move-json-file topics-to-move.json --broker-list "5,6" --generate

# Содержимое topics-to-move.json:
# {
#   "topics": [
#     {"topic": "foo1"},
#     {"topic": "foo2"}
#   ],
#   "version": 1
# }

# Выполнить перераспределение по сгенерированному плану
kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file expand-cluster-reassignment.json --execute

# Проверить статус перераспределения
kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file expand-cluster-reassignment.json --verify

# Выполнить кастомное перераспределение (вручную составленный план)
kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file custom-reassignment.json --execute

----------Управление потребителями: группы------------
# Показать список всех групп потребителей
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# Показать детали конкретной группы (смещения и lag)
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-group

# Показать смещения всех групп
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --offsets --describe --all-groups

# Показать участников конкретной группы
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --members --describe --group my-group

----------Управление потребителями: сброс смещений------------
# Сбросить смещения в начало (предварительный просмотр)
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --to-earliest --dry-run

# Сбросить смещения в конец
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --to-latest --execute

# Сбросить смещения на конкретное значение
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --to-offset 1000 --execute

# Сбросить смещения сдвигом на -10
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --shift-by -10 --execute

# Экспортировать текущие смещения в CSV
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --to-current --export --dry-run

# Сбросить смещения из CSV-файла
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --topic my-topic --reset-offsets --from-file offsets.csv --execute

----------Управление потребителями: удаление------------
# Удалить группу потребителей (только для пустых групп)
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --delete --group my-group

----------Производство и потребление: консольные клиенты------------
# Отправить сообщения из stdin в топик
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic

# Читать сообщения из топика с начала
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning

# Читать сообщения с указанием группы (смещения сохраняются)
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --group my-consumer-group

# Читать с указанием свойств безопасности (SASL/SSL)
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --consumer.config client.properties

----------Тестирование производительности------------
# Тест производительности продюсера
kafka-producer-perf-test.sh --topic my-topic --num-records 1000000 --record-size 100 --throughput -1 --producer-props bootstrap.servers=localhost:9092

# Тест производительности консюмера
kafka-consumer-perf-test.sh --bootstrap-server localhost:9092 --topic my-topic --messages 1000000

----------Kafka Streams: управление группами------------
# Показать список групп Streams
kafka-streams-groups.sh --bootstrap-server localhost:9092 --list

# Показать детали группы Streams
kafka-streams-groups.sh --bootstrap-server localhost:9092 --describe --group my-streams-app --state --verbose

# Показать смещения входных топиков и lag
kafka-streams-groups.sh --bootstrap-server localhost:9092 --describe --group my-streams-app --offsets

# Сбросить смещения входных топиков (предварительный просмотр)
kafka-streams-groups.sh --bootstrap-server localhost:9092 --group my-streams-app --reset-offsets --all-input-topics --to-datetime 2025-01-31T23:57:00.000 --dry-run

# Удалить группу Streams
kafka-streams-groups.sh --bootstrap-server localhost:9092 --delete --group my-streams-app

----------Настройка прав доступа (ACL)------------
# Показать все ACL
kafka-acls.sh --bootstrap-server localhost:9092 --list

# Добавить ACL для пользователя на чтение топика
kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:alice --operation Read --topic orders

# Добавить ACL для группы потребителей
kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:alice --operation Read --group order-worker

# Удалить ACL
kafka-acls.sh --bootstrap-server localhost:9092 --remove --allow-principal User:alice --operation Read --topic orders

----------Управление квотами------------
# Показать квоты для пользователя
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type users --entity-name alice --describe

# Установить квоту на производство для пользователя
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type users --entity-name alice --alter --add-config producer_byte_rate=10485760

# Установить квоту на потребление для пользователя
kafka-configs.sh --bootstrap-server localhost:9092 --entity-type users --entity-name alice --alter --add-config consumer_byte_rate=20971520

----------Управление ZooKeeper------------
# Подключиться к ZooKeeper через shell
zookeeper-shell.sh localhost:2181

# Показать конфигурацию ZooKeeper
zookeeper-shell.sh localhost:2181 get /zookeeper/config

# Выполнить 4-letter-word команду (stat)
echo stat | nc localhost 2181

----------Управление журналами------------
# Просмотреть логи брокера
tail -f /var/log/kafka/server.log

# Изменить уровень логирования для компонента (через Connect REST API)
curl -X PUT -H "Content-Type: application/json" -d '{"org.apache.kafka.connect.runtime.Worker": "DEBUG"}' http://localhost:8083/admin/loggers/org.apache.kafka.connect.runtime.Worker

# Запустить с кастомным log4j.properties
export KAFKA_LOG4J_OPTS="-Dlog4j.configuration=file:/path/to/log4j.properties"
kafka-server-start.sh config/server.properties

----------Запуск и остановка------------
# Запустить брокер в фоне
kafka-server-start.sh -daemon config/server.properties

# Остановить брокер
kafka-server-stop.sh

# Запустить ZooKeeper в фоне
zookeeper-server-start.sh -daemon config/zookeeper.properties

# Остановить ZooKeeper
zookeeper-server-stop.sh

----------Проверка работы------------
# Проверить, запущен ли процесс Kafka
jcmd | grep Kafka

# Проверить доступность брокера
kafka-broker-api-versions.sh --bootstrap-server localhost:9092 > /dev/null && echo "Broker is up"
```