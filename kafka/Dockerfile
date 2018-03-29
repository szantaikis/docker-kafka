# Kafka and Zookeeper

FROM  docker.flcn.io/base/java:8

ENV DEBIAN_FRONTEND noninteractive
ENV SCALA_VERSION 2.11
ENV KAFKA_VERSION 0.11.0.2
ENV KAFKA_HOME /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"
ENV ZOOKEEPER_VERSION 3.4.11
ENV ZOOKEEPER_HOME /opt/zookeeper-"$ZOOKEEPER_VERSION"

# Install Kafka, Zookeeper and other needed things
RUN apt-get update

RUN apt-get install -y wget
RUN apt-get install -y supervisor
# RUN apt-get install -y dnsutils
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean

RUN wget -q http://apache.mirrors.spacedump.net/zookeeper/zookeeper-"$ZOOKEEPER_VERSION"/zookeeper-"$ZOOKEEPER_VERSION".tar.gz -O /tmp/zookeeper-"$ZOOKEEPER_VERSION".tar.gz
RUN tar xfz /tmp/zookeeper-"$ZOOKEEPER_VERSION".tar.gz -C /opt
RUN rm /tmp/zookeeper-"$ZOOKEEPER_VERSION".tar.gz

RUN wget -q http://apache.mirrors.spacedump.net/kafka/"$KAFKA_VERSION"/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -O /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz
RUN tar xfz /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -C /opt
RUN rm /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz
RUN ln -s "$ZOOKEEPER_HOME" /opt/zookeeper
RUN rm -rf "$ZOOKEEPER_HOME"/docs "$ZOOKEEPER_HOME"/src

ADD scripts/start-kafka.sh /usr/bin/start-kafka.sh

# Supervisor config
ADD supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092

CMD ["supervisord", "-n", "-c","/etc/supervisor/supervisord.conf"]
