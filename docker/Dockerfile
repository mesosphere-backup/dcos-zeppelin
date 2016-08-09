FROM mesosphere/mesos:0.26.0-0.2.145.ubuntu1404

RUN apt-get update && \
    apt-get install -y software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update && \
    apt-get install --yes \
    openjdk-8-jdk\
    wget \
    tar

# hadoop config
ENV HADOOP_CONF_DIR /etc/hadoop
ADD ./conf/hadoop/hdfs-site.xml /etc/hadoop/hdfs-site.xml
ADD ./conf/hadoop/core-site.xml /etc/hadoop/core-site.xml
ADD ./conf/hadoop/mesos-site.xml /etc/hadoop/mesos-site.xml

# java
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
RUN update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

# zeppelin
RUN wget http://mirror.serversupportforum.de/apache/zeppelin/zeppelin-0.6.0/zeppelin-0.6.0-bin-all.tgz
RUN tar xzvf zeppelin-0.6.0-bin-all.tgz
WORKDIR /zeppelin-0.6.0-bin-all
ADD zeppelin-env.sh conf/zeppelin-env.sh

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r zeppelin && useradd -r -m -g zeppelin zeppelin

RUN \
	chown -R zeppelin:zeppelin /zeppelin-0.6.0-bin-all && \
    chown -R zeppelin:zeppelin /etc/hadoop

USER zeppelin

CMD ["bin/zeppelin.sh", "start"]
