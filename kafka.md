# Kafka

## Installing

### How to install

1. Install the default available version of **Java**.
    ```bash
    $ yum -y install java-1.8.0-openjdk
    $ java -version
    ```

2. Download and extract [the most recent stable version](https://kafka.apache.org/downloads) of **Apache Kafka**.

    ```bash
    $ tar -xzf kafka_2.13-2.8.0.tgz
    ```

3. Add **Kafka** environment to the `.bash_profile` file and initialize it.

    ```bash
    $ ln -s kafka_2.13-2.8.0 kafka
    $ echo "export PATH=$PATH:/root/kafka_2.13-2.8.0/bin" >> ~/.bash_profile
    $ source ~/.bash_profile
    ```

### Note

## Running

### How to start

1. Start **Zookeeper** (with default properties) and check its accessibility.

   ```bash
   $ zookeeper-server-start.sh -daemon /root/kafka/config/zookeeper.properties
   $ telnet localhost 2181
   ```

2. Update the following fields in the `server.properties` configuration file.

   ```toml
   listeners=PLAINTEXT://localhost:9092
   advertised.listeners=PLAINTEXT://localhost:9092
   ```
   
3. Start **Kafka** (with default properties) and check its accessibility.

   ```bash
   $ kafka-server-start.sh -daemon /root/kafka/config/server.properties
   $ telnet localhost 9092
   ```

### How to run

#### Create a topic

```bash
$ kafka-topics.sh --create --topic sample-topic --bootstrap-server localhost:9092
```

#### Describe a topic

```bash
$ kafka-topics.sh --describe --topic sample-topic --bootstrap-server localhost:9092
```

#### Write events to a topic

```bash
$ kafka-console-producer.sh --topic sample-topic --bootstrap-server localhost:9092
```

#### Read events from a topic

```bash
$ kafka-console-consumer.sh --topic sample-topic --from-beginning --bootstrap-server localhost:9092
```

#### [Delete a topic](https://stackoverflow.com/questions/33537950/how-to-delete-a-topic-in-apache-kafka)

#### List all topics

```bash
$ kafka-topics.sh --list --bootstrap-server localhost:9092
```

### How to stop

1. Stop **Zookeeper** and check its accessibility.

   ```bash
   $ zookeeper-server-stop.sh
   $ telnet localhost 2181
   ```
2. Stop **Kafka** and check its accessibility.

   ```bash
   $ kafka-server-stop.sh
   $ telnet localhost 9092
   ```

### Note

