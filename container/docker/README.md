# Kafka Dockerfile

[Dockerfile](https://docs.docker.com/engine/reference/builder/) to build a Docker image with [Apache Kafka](http://kafka.apache.org).

## Getting Started

In order to build the image just clone the repo to your machine and run [docker build](https://docs.docker.com/engine/reference/commandline/build/) inside the [build](./build/) directory where the [Dockerfile](./build/Dockerfile) and [docker-entrypoint.sh](./build/docker-entrypoint.sh) are. Example:

```bash
docker build -t mykafka:1.0 .
```

### Prerequisities

In order to build the image and run this container you'll need docker installed.

This was tested using [Docker Desktop](https://www.docker.com/products/docker-desktop) for MacOS version 2.1.0.5.

An [Apache ZooKeeper](http://zookeeper.apache.org) image must be present, we are using one created from the steps on this [repo](https://github.com/ricardo-aires/zookeeper-deploy/tree/master/container/docker).

### Base Image

The base image used in this case was the [openjdk:8 Docker Official Image](https://hub.docker.com/_/openjdk). When running inside a organization we should use a base image with:

- java
- bash
- wget

### Usage

#### Start in Standalone Mode

In order to run a [single broker](https://kafka.apache.org/quickstart) we need, first to run a ZooKeeper Server, like seen [here](https://github.com/ricardo-aires/zookeeper-deploy/tree/master/container/docker#start-in-standalone-mode).

> The `ZOO_CONNECT` variable is always required to be set.

We can then:

```bash
docker container run --detach --restart always --link zk-0 --env ZOO_CONNECT=zk-0:2181 -p 9092:9092 --name kafka-0 mykafka:1.0
```

We can then test our running Kafka from our local client following [this](https://kafka.apache.org/quickstart#quickstart_createtopic) guide.

Remember to change:

- container name in the `link` option accordingly
- `ZOO_CONNECT` variable accordingly
- the image name if you choose another one when building

The image expose the next ports:

- 29092/TCP - Internal Listener port
- 9092/TCP - External Listener port, the one used to publish to your local machine

#### Start Multi Broker

In order to run in [Multi Broker](https://kafka.apache.org/quickstart#quickstart_multibroker) we need to assing an unique `KAFKA_BROKER_ID` for each broker, and if publishing the port a different `KAFKA_PORT` to be used in the external listener.

We can use this [docker-compose file](https://docs.docker.com/compose/compose-file/) found [here](./docker-compose.yml) to spin a 3-node ensemble.

It will:

- build the image and tag it as `mykafka:1.0`, if not already
- run 3 instances of ZooKeeper, exposing the AdminServer port and creating a Volume to persist data
- run 3 instances of Kafka, exposing the external listener port and creating a Volume to persist data

#### Volumes

The only volume exposed is the one for the Kafka Data directoy, at `/data`.

#### Environment Variables

Other variables to ease the [settings](https://kafka.apache.org/documentation/#configuration):

- `KAFKA_NUM_PARTITIONS` - number of partitions for the offset commit topic (default to 1)
- `KAFKA_TOPIC_REPLICATION_FACTOR` - replication factor for the offsets topic (default to 1)
- `KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR` - replication factor for the transaction topic (default to 1)
- `KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR_ISR` - Overridden `min.insync.replicas` config for the transaction topic (default to 1)
- `KAFKA_LOG_RETENTION_HOURS` - number of hours to keep a log file before deleting it (default to 168)
- `KAFKA_LOG_RETENTION_CHECK_INTERVAL_MS` - frequency in milliseconds that the log cleaner checks whether any log is eligible for deletion (default to 300000)
