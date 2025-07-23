# Apache Kafka - Architecture, Zookeeper and KRaft
Apache Kafka Fundamentals from an Admin Perspective.

The video was created to share the knowledge I’ve gained working with Kafka with colleagues and a few friends.
I personally edited the video, although I don’t have much experience with video editing tools.

 
I am currently using a container. I recommend installing Docker or Docker Desktop – available at https://www.docker.com/products/docker-desktop/.

### Create a Directory on Your Local Machine and Run All Commands From There
``` bash
mkdir kafka
cd kafka
```

Download the Ubuntu Image
``` bash
docker pull ubuntu
```

### Running the Container: Examples for Different Operating Systems
On Windows CMD
``` bash
docker run --rm -it --name kafka01 --hostname kafka01 -v %cd%:/opt -p 9092:9092 ubuntu:latest
```

On Windows PowerShell
``` bash
docker run --rm -it --name kafka01 --hostname kafka01 -v  ${PWD}:/opt -p 9092:9092 ubuntu:latest
```

On Linux
``` bash
docker run --rm -it --name kafka01 --hostname kafka01 -v $(pwd):/opt -p 9092:9092 ubuntu:latest
``` 

Using Podman on Linux
``` bash
sudo podman run --privileged --rm -it --name kafka01 --hostname kafka01 -v $(pwd):/opt -p 9092:9092 ubuntu:latest
``` 

Installing Packages
``` bash
apt update && apt install default-jdk wget vim jq kafkacat -y
``` 

# Zookeeper + Kafka

Downloading Kafka
```bash 
wget -c https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz -O - | tar -xz
cd kafka_2.13-3.9.1
``` 

Starting Zookeeper
``` bash
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
``` 

Starting Kafka
``` bash
bin/kafka-server-start.sh -daemon config/server.properties
``` 

Creating a Topic
``` bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic topico1
``` 

Listing Topics
``` bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
``` 

Describe
``` bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic topico1
``` 

# Kraft + Kafka

```bash 
wget -c https://dlcdn.apache.org/kafka/4.0.0/kafka_2.13-4.0.0.tgz -O - | tar -xz
cd kafka_2.13-4.0.0
``` 

## Get from Docker image:
```bash
docker pull apache/kafka:4.0.0
```

Start the Kafka Docker container:
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