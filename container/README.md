# Containerised Kafka Broker

This project provides a way to run [Apache Kafka](http://kafka.apache.org) in containers, in standalone or multi-broker.

> The solutions provided were designed for Proof of Concepts. Hence, are not to be treated as production ready, especially because of the lack of Security settings.

## Requirements

An [Apache ZooKeeper](https://kafka.apache.org/quickstart) needs to be in-place, we are using the one that can be built following our other [repo](https://github.com/ricardo-aires/zookeeper-deploy/blob/master/container/docker/).

## Dockerfiles

A [Dockerfile](https://docs.docker.com/engine/reference/builder/) to create the image is provided, more information can be found [here](./docker/README.md).

## Docker Compose

A [docker-compose file](https://docs.docker.com/compose/compose-file/) will be provided to spin Kafka in [multi-broker](./docker/docker-compose.yml).

We can use them to build the required image also, from this directory just run:

```bash
docker-compose -f ./docker/docker-compose.yml build
```

To spin up:

```bash
docker-compose -f ./docker/docker-compose.yml build
```

More information [here](./docker/README.md#Start-Multi-Broker)

## Kubernetes

After creating the images we will be able to run it in [Kubernetes](https://kubernetes.io) using [this](./k8s/k8s-kafka-deploy.yml).

To spin up, run:

```bash
kubectl apply -f ./k8s/k8s-kafka-deploy.yml
```

More information [here](./k8s/README.md)
