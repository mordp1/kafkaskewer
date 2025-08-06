# Apache Kafka - Log, Stream e Broker
Apache Kafka Fundamentals from an Admin Perspective.

The video was created with the intention of sharing the knowledge I have gained working with Kafka with colleagues and a few friends.

## Commands:
I am currently running a container and recommend installing Docker or Docker Desktop.

Start 
```bash
docker-compose -f zk-multiple-kafka-multiple.yml up -d
```

Connecting to the shell of the kafka1 container
```bash
docker exec -it kafka1 /bin/bash
```

Creating three topics inside the container
```bash
kafka-topics --bootstrap-server kafka1:19092 --topic topic1 --create --partitions 3 --replication-factor 1

kafka-topics --bootstrap-server kafka1:19092 --topic topic2 --create --partitions 3 --replication-factor 2

kafka-topics --bootstrap-server kafka1:19092 --topic topic3 --create --partitions 3 --replication-factor 3
```
To run the commands outside the container, you need to have the Kafka binaries:

```bash
wget -c https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz -O - | tar -xz
cd kafka_2.13-3.9.1
```
 
Commands for creating topics outside the container
```bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --topic topic1 --create --partitions 3 --replication-factor 1

bin/kafka-topics.sh --bootstrap-server localhost:9092 --topic topic2 --create --partitions 3 --replication-factor 2

bin/kafka-topics.sh --bootstrap-server localhost:9092 --topic topic3 --create --partitions 3 --replication-factor 3
```