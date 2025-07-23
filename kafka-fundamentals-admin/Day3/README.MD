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
kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
```

Start the Kafka Server
```bash
kafka-server-start.sh -daemon config/server.properties
```

Create Topic
```bash
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic topic2 --partitions 4
```

List Topic
```bash
kafka-topics.sh --bootstrap-server localhost:9092 --list
```

Describe
```bash
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic topic2
```

Producer
```bash
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic topic2
```

Consumer
```bash
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic2 --from-beginning
```