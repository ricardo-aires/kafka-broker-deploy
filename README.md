# Apache Kafka

[Apache Kafka](https://kafka.apache.org) is a distributed streaming platform, a publish/subscribe messaging system.

![Apache Kafka](./img/kafka-logo.png)

The aim of this project is to provide different deployment methods to setup the Kafka Broker:

- [Ansible](ansible/roles/kafka-broker/README.md)
- [Container](container/README.md)

> We are not going to use the [Confluent](https://www.confluent.io) distribution, but take it into consideration. Also we are opting-out of external access by design.

## Considerations

The solutions provided were designed for Proof of Concepts. Hence, are not to be treated as production ready, especially because of the lack of Security settings.

### System Requirements

The latest Kafka version at this time is [2.4.0](http://mirror.linux-ia64.org/apache/kafka/).

The Kafka core is written in Scala, and Kafka Streams and KSQL are written in Java. A Kafka server can run in several operating systems: Unix, Linux, macOS, and even Windows.

We will use the latest [OpenJDK 8](https://openjdk.java.net) available in each OS package management.

A [ZooKeeper Server](http://zookeeper.apache.org) is required, we can use our [zookeeper-deploy](http://github.com/ricardo-aires/zookeeper-deploy) or [use the one that comes with kafka](https://kafka.apache.org/quickstart) to get one up and running, or even check the [ZooKeeper Docker Official Image](https://hub.docker.com/_/zookeeper).

### Architecture

Kafka is run as a cluster on one or more servers, which stores streams of records in categories called topics.

Topics are additionally broken down into a partitioned log.

![Anatomy of a Topic](./img/log_anatomy.png)

Each partition is an ordered, immutable sequence of records that is continually appended to—a structured commit log. The records in the partitions are each assigned a sequential id number called the offset that uniquely identifies each record within the partition.

The Kafka cluster durably persists all published records—whether or not they have been consumed—using a configurable retention period. For example, if the retention policy is set to two days, then for the two days after a record is published, it is available for consumption, after which it will be discarded to free up space. Kafka's performance is effectively constant with respect to data size so storing data for a long time is not a problem.

Going back to the “commit log” description, a partition is a single log. Messages are written to it in an append-only fashion, and are read in order from beginning to end. Note that as a topic typically has multiple partitions, there is no guarantee of message time-ordering across the entire topic, just within a single partition. Figure 1-5 shows a topic with four partitions, with writes being appended to the end of each one. Partitions are also the way that Kafka provides redundancy and scalability. Each partition can be hosted on a different server, which means that a single topic can be scaled horizontally across multiple servers to provide performance far beyond the ability of a single server.

![Kakfa APIs](./img/kafka-apis.png)

Kafka has four core APIs:

- [Producer API](https://kafka.apache.org/documentation.html#producerapi) - allows an application to publish a stream of records to one or more Kafka topics.
- [Consumer API](https://kafka.apache.org/documentation.html#consumerapi) - allows an application to subscribe to one or more topics and process the stream of records produced to them.
- [Streams API](https://kafka.apache.org/documentation/streams) - allows an application to act as a stream processor, consuming an input stream from one or more topics and producing an output stream to one or more output topics, effectively transforming the input streams to output streams.
- [Connector API](https://kafka.apache.org/documentation.html#connect) allows building and running reusable producers or consumers that connect Kafka topics to existing applications or data systems.
