FROM openjdk:8

ARG MIRROR=http://apache-mirror.rbc.ru/pub/apache/kafka
ARG SCALA_VERSION=2.12
ARG KAFKA_VERSION=2.4.0
ARG DISTRO_NAME=kafka_$SCALA_VERSION-$KAFKA_VERSION

ENV KAFKA_HOME=/$DISTRO_NAME \
    KAFKA_LOG_DIR=/tmp/kafka-logs \
    KAFKA_TOPIC_REPLICATION_DIR=1 \
    KAFKA_HEAP_OPTS="-Xmx2G -Xms2G" \
    PATH=$PATH:/$DISTRO_NAME/bin  \
    KAFKA_K8S=false \
    KAFKA_PORT=9092

RUN set -eux && \
    groupadd -r kafka --gid=1000 && \
    useradd -r -g kafka --uid=1000 kafka 

RUN set -eux && \
    wget "$MIRROR/$KAFKA_VERSION/$DISTRO_NAME.tgz" -O "$DISTRO_NAME.tgz" && \
    tar xvzf "$DISTRO_NAME.tgz" && \
    rm -rf  "$DISTRO_NAME.tgz" && \
    mkdir -p /etc/kafka && \
    mkdir -p $KAFKA_LOG_DIR && \
    chown -R kafka:kafka "$KAFKA_HOME" $KAFKA_LOG_DIR /etc/kafka
    

WORKDIR $DISTRO_NAME
VOLUME ["$KAFKA_LOG_DIR"]

EXPOSE 29092 9092

USER kafka
COPY --chown=kafka:kafka docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kafka-server-start.sh", "/etc/kafka/server.properties"]