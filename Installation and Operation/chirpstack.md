# ChirpStack

## Installing

### How to install

1. Install **Mosquitto**.

   ```bash
   $ sudo yum install mosquito
   ```

2. Install **Redis.**

   ```bash
   $ sudo yum install redis
   ```

3. Install **PostgreSQL 12.**

   ```bash
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-libs-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-server-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-contrib-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
   ```

4. Install **ChirpStack Gateway Bridge**.

   ```bash
   $ sudo yum install chirpstack-gateway-bridge_3.10.0_linux_386.rpm
   ```

5. Install **ChirpStack Network Server**.

   ```bash
   $ sudo yum install chirpstack-network-server_3.12.3_linux_386.rpm
   ```

6. Install **ChirpStack Application Server**.

   ```bash
   $ sudo yum install chirpstack-application-server_3.14.0_linux_386.rpm
   ```

### Note

* **ChirpStack Gateway Bridge** RPM package can be found at [here](https://www.chirpstack.io/gateway-bridge/downloads/).
* **ChirpStack Network Server** RPM package can be found at [here](https://www.chirpstack.io/network-server/downloads/).
* **ChirpStack Application Server** RPM package can be found at [here](https://www.chirpstack.io/application-server/downloads/).

## Configuration

### How to configure

1. Configure **PostgreSQL 12**.

   ```bash
   # Update /var/lib/pgsql/12/data/pg_hba.conf
   $ sudo vi /var/lib/pgsql/12/data/pg_hba.conf
   host all all 172.0.0.1/32 password
   host all all ::1/128 password
   
   # Configure PostgreSQL 12 for ChirpStack
   $ sudo su - postgres
   -bash-4-2$ psql
   create role chirpstack_ns with login password 'chirpstack_ns';
   create database chirpstack_ns with owner chirpstack_ns;
   create role chirpstack_as with login password 'chirpstack_as';
   create database chirpstack_as with owner chirpstack_as;
   \c chirpstack_as
   create extension pg_trgm;
   create extension hstore;
   ```

2. Configure **ChirpStack Network Server**.

   ```bash
   $ sudo vi /etc/chirpstack-network-server/chirpstack-network-server.toml
   dns="postgres://chirpstack_ns:chirpstack_ns@localhost/chirpstack_ns?sslmode=disable"
   ```

3. Configure **ChirpStack Application Server**

   ```bash
   $ sudo vi /etc/chirpstack-application-server/chirpstack-application-server.toml
   dns="postgres://chirpstack_as:chirpstack_as@localhost/chirpstack_as?sslmode=disable"
   jwt_secret="verysecret"
   ```

### Note

## Running

### How to run

#### start.sh

```bash
sudo systemctl start mosquitto postgresql-12 redis chirpstack-gateway-bridge chirpstack-network-server chirpstack-application-server
```

#### restart.sh

```bash
sudo systemctl restart chirpstack-application-server chirpstack-network-server chirpstack-gateway-bridge redis postgresql-12 mosquitto
```

#### log.sh

```bash
sudo journalctl -u chirpstack-application-server -u chirpstack-network-server -u chirpstack-gateway-bridge -u mosquitto -f
```

#### save-log.sh

```bash
#!/bin/bash
sudo tail -4000 /var/log/messages | grep -E 'chirpstack*|mosquitto' > ~/tmp/chirpstack-log/$1.log
```

#### stop.sh

```bash
sudo systemctl stop chirpstack-application-server chirpstack-network-server chirpstack-gateway-bridge redis postgresql-12 mosquitto
```

### [How to connect a gateway](https://www.chirpstack.io/project/guides/connect-gateway/)

### [How to connect a device](https://www.chirpstack.io/project/guides/connect-device/)

### [How to integrate MQTT with ChirpStack](https://www.chirpstack.io/application-server/integrations/mqtt/)

### [How to integrate PostgreSQL with ChirpStack](https://www.chirpstack.io/application-server/integrations/postgresql/)

### How to integrate Kafka with ChirpStack

1. Create a topic to store **ChirpStack Application Server** events on **Kafka**.

   ```bash
   $ kafka-topics.sh --create --topic chirpstack_as --bootstrap-server localhost:9092
   ```

2. Update **ChirpStack Application Server** [configuration file](https://www.chirpstack.io/application-server/install/config/).

   ```bash
   $ sudo vi /etc/chirpstack-application-server/chirpstack-application-server.toml
   # Enabled integrations.
   #
   # Enabled integrations are enabled for all applications. Multiple
   # integrations can be configured.
   # Do not forget to configure the related configuration section below for
   # the enabled integrations. Integrations that can be enabled are:
   # * mqtt              - MQTT broker
   # * amqp              - AMQP / RabbitMQ
   # * aws_sns           - AWS Simple Notification Service (SNS)
   # * azure_service_bus - Azure Service-Bus
   # * gcp_pub_sub       - Google Cloud Pub/Sub
   # * kafka             - Kafka distributed streaming platform
   # * postgresql        - PostgreSQL database
   enabled=["mqtt", "kafka"]
   
   # Kafka integration.
   [application_server.integration.kafka]
   # Brokers, e.g.: localhost:9092.
   brokers=["localhost:9092"]
   
   # TLS.
   #
   # Set this to true when the Kafka client must connect using TLS to the Broker.
   tls=false
   
   # Topic for events.
   topic="chirpstack_as"
   
   # Template for keys included in Kafka messages. If empty, no key is included.
   # Kafka uses the key for distributing messages over partitions. You can use
   # this to ensure some subset of messages end up in the same partition, so
   # they can be consumed in-order. And Kafka can use the key for data retention
   # decisions.  A header "event" with the event type is included in each
   # message. There is no need to parse it from the key.
   event_key_template="application.{{ .ApplicationID }}.device.{{ .DevEUI }}.event.{{ .EventType }}"
   
   # Username (optional).
   username=""
   
   # Password (optional).
   password=""
   
   # One of plain or scram
   mechanism="plain"
   
   # Only used if mechanism == scram.
   # SHA-256 or SHA-512 
   algorithm="SHA-512"
   ```

3. Verify whether events are published to the related topic.

   ```bash
   $ kafka-console-consumer.sh --topic chirpstack_as --from-beginning --bootstrap-server localhost:9092
   ```

### Note

## References

[ChirpStack, open-source LoRaWAN® Network Server stack](https://www.chirpstack.io/)