# Kafka Connect - FileStream from local file.

Apache Kafka Fundamentals from an Admin Perspective.

The video was created with the intention of sharing the knowledge I have gained working with Kafka with colleagues and a few friends.
I edited the video myself, without much experience in video editing tools.

Understanding Kafka Connect in practice, configuring a FileStream connector so that it can read a local file and produce messages on the Kafka topic


## Kafkacat: 

https://github.com/edenhill/kcat
``` bash
$ apt-get install kafkacat
```
## Utilizaremos o exemplo da Conduktor.

https://www.conduktor.io/kafka/how-to-start-kafka-using-docker/

### Baixe o repositorio
```bash
$ git clone https://github.com/mordp1/espetinhodekafka
$ cd espetinhodekafka/day9
```

### Subindo Kafka Cluster com Kafa Connect

``` bash
$ docker-compose -f zk-multiple-kafka-multiple.yml up -d
``` 

### Crie um novo topico que será usado para produzirmos as mensagens.

``` bash
$ docker exec -it kafka1 kafka-topics --bootstrap-server kafka1:19092 --topic connect-test --create --partitions 3 --replication-factor 3
```

### Você pode conectar no container e criar os seguinte arquivos

```bash
$ docker exec -it kafka-connect /bin/bash
$ cd /tmp/
```

### Crie o config.properties com seguinte conteúdo:
``` bash
$ cat << EOF> config.properties 
bootstrap.servers=kafka1:19092
key.converter=org.apache.kafka.connect.json.JsonConverter
value.converter=org.apache.kafka.connect.json.JsonConverter
key.converter.schemas.enable=true
value.converter.schemas.enable=true
offset.storage.file.filename=/tmp/connect.offsets
offset.flush.interval.ms=10000
plugin.path=/usr/share/filestream-connectors
EOF
```
### Crie o connect-file-source.properties com seguinte conteúdo:
``` bash
$ cat << EOF> connect-file-source.properties
name=local-file-source
connector.class=FileStreamSource
tasks.max=1
file=/tmp/test.txt
topic=connect-test
EOF
``` 
### Agora crie um arquivo em /tmp/test.txt para que o connector possa ler e produzir a mensagem
``` bash
$ cat <<EOF> 
teste1
teste2
teste3
EOF
```

### Se você não criou os arquivos dentro do container, basta copiar os 3 arquivos.
``` bash
$ docker cp connect-file-source.properties kafka-connect:/tmp
$ docker cp config.properties kafka-connect:/tmp
$ docker cp test.txt kafka-connect:/tmp
``` 
### Abra um novo terminal e inicie o Kafka Connect:
``` bash
$ docker exec -ti kafka-connect connect-standalone /tmp/config.properties /tmp/connect-file-source.properties
``` 
### Abra um novo terminal e faça o consumo das mensagens:
``` bash
$ docker exec -it kafka1 kafka-console-consumer --bootstrap-server localhost:19092 --topic connect-test --from-beginning
``` 
### Ou pode ser utilizado o kafkacat da maquina local:
``` bash
$ kafkacat -C -b localhost:9092 -t connect-test
``` 
### Você pode adicionar novas linhas no arquivo e ver ele sendo processado

``` bash
$ docker exec -it kafka-connect /bin/bash
$ echo teste4 >> /tmp/test.txt
```

### Derrubando todos os containers:
``` bash
$ docker-compose -f zk-multiple-kafka-multiple.yml down
``` 

## Fatiando o Vídeo

https://youtu.be/hdC5oMpgRlc