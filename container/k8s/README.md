# Kafka in Kubernetes

The image created [here](../docker/README.md) may be used to spin a Kafka Multi-Broker in [Kubernetes](https://kubernetes.io) using the [k8s-kafka-deploy.yml](./k8s-kafka-deploy.yml) file.

In the example we will use the [Apache ZooKeeper](http://zookeeper.apache.org) image built from this [repo](https://github.com/ricardo-aires/zookeeper-deploy/tree/master/container/docker).

We are not going to approach the Kubernetes bits for the ZooKeeper, we can check them [here](https://github.com/ricardo-aires/zookeeper-deploy/tree/master/container/k8s).

## Getting started

After building the required images just run, from this directory:

```bash
kubectl apply -f ./k8s-kafka-deploy.yml
```

It will create, in terms of Kakfa:

- A [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) for our test
- An [headless service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services) to expose the Internal Listners inside the cluster
- A [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

## Considerations

### Variables

Unlike running in [docker](../docker/README.md#Start-Multi-Broker), we shouldn't define `KAFKA_BROKER_ID` and `KAFKA_PORT`.

Instead just set `KAFKA_K8S` and all will be automatically setup.

All variables regarding [settings](../docker/README.md#Environment-Variables) may be used.

You still need to place the `ZOO_CONNECT` one.

### Ports Expose

We are only setting an [headless service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services) to expose the Internal Listners inside the cluster.

We opt not to have External Listner to ease the deployment and because the goal is to have Producers and Consumers inside.

> We encourage to read through [this](https://rmoff.net/2018/08/02/kafka-listeners-explained/) if you want to use external.

### Scale

The Kafka Broker can be easily scale, no change of variables needed.
