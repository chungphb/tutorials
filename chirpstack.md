# ChirpStack

## Installing

### How to install

1. Install Mosquitto

   ```bash
   $ sudo yum install mosquito
   ```

2. Install Redis

   ```bash
   $ sudo yum install redis
   ```

3. Install PostgreSQL 12

   ```bash
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-libs-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-server-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo yum install https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64/postgresql12-contrib-12.2-1PGDG.rhel7.x86_64.rpm
   $ sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
   ```

4. Install ChirpStack Gateway Bridge

   ```bash
   $ sudo yum install chirpstack-gateway-bridge_3.10.0_linux_386.rpm
   ```

5. Install ChirpStack Network Server

   ```bash
   $ sudo yum install chirpstack-network-server_3.12.3_linux_386.rpm
   ```

6. Install ChirpStack Application Server

   ```bash
   $ sudo yum install chirpstack-application-server_3.14.0_linux_386.rpm
   ```

### Note

* ChirpStack Gateway Bridge RPM package can be downloaded at [here](https://www.chirpstack.io/gateway-bridge/downloads/).
* ChirpStack Network Server RPM package can be downloaded at [here](https://www.chirpstack.io/network-server/downloads/).
* ChirpStack Application Server RPM package can be downloaded at [here](https://www.chirpstack.io/application-server/downloads/).

## Configuration

### How to configure

1. Configure PostgreSQL 12

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

2. Configure ChirpStack Network Server

   ```bash
   $ sudo vi /etc/chirpstack-network-server/chirpstack-network-server.toml
   dns="postgres://chirpstack_ns:chirpstack_ns@localhost/chirpstack_ns?sslmode=disable"
   ```

3. Configure ChirpStack Application Server

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

```
#!/bin/bash
sudo tail -4000 /var/log/messages | grep -E 'chirpstack*|mosquitto' > ~/chungphb/debug/$1.log
```

#### stop.sh

```bash
sudo systemctl stop chirpstack-application-server chirpstack-network-server chirpstack-gateway-bridge redis postgresql-12 mosquitto
```

### Note

## References

[ChirpStack, open-source LoRaWAN® Network Server stack](https://www.chirpstack.io/)