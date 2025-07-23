# Apache Kafka - Topic, Partition e Segment.
Apache Kafka Fundamentals from an Admin Perspective.

The video was created with the intention of sharing the knowledge I have gained working with Kafka with colleagues and a few friends.
I edited the video myself, without much experience in video editing tools.


I am currently using a container. I recommend installing Docker or Docker Desktop – available at https://www.docker.com/products/docker-desktop/.

https://www.docker.com/products/docker-desktop/

# Start Kafka
```bash
docker run --rm -it --name kafka01 --hostname kafka01 -e PATH="/opt/kafka/bin/:$PATH" -p 9092:9092 apache/kafka:4.0.0 /bin/bash
```

Generate a Cluster UUID
```bash
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```

Format Log Directories
```bash
bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
```

Start the Kafka Server
```bash
bin/kafka-server-start.sh config/server.properties
```

Create Topico
```bash
$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic topico2 --partitions 4
```

List Topicos
```bash
$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

Describe
```bash
$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic topico2
```

Producer
```bash
$ bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic topico1
```

Consumer 
```bash
docker 
```

Executar o consumer
```bash
$ cd /opt/kafka_2.13-3.4.0
$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topico1
```