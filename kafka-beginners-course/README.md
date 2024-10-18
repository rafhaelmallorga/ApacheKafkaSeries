## Start Kafka Server

Generate a Cluster UUID
```bash
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```
Format Log Directories

```bash
bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
```

Start the Kafka Server

```bash
bin/kafka-server-start.sh config/kraft/server.properties
```

## Topic Creation
```bash
bin/kafka-topics.sh --create --topic demo_java --partitions 3 --bootstrap-server localhost:9092
```

## Describe Topic

```bash
bin/kafka-topics.sh --describe --topic demo_java --bootstrap-server localhost:9092
```

## Start Consumer

```bash
bin/kafka-console-consumer.sh --topic demo_java --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --property print.partition=true --from-beginning --bootstrap-server localhost:9092
```