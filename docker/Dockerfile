FROM mesosphere/mesos:0.26.0-0.2.145.ubuntu1404

RUN apt-get update \
  && apt-get install -y software-properties-common \
  && add-apt-repository ppa:openjdk-r/ppa \
  && apt-get update \
  && apt-get install --yes openjdk-8-jdk wget tar

# hadoop config
ENV HADOOP_CONF_DIR /etc/hadoop
ADD ./conf/hadoop/hdfs-site.xml /etc/hadoop/hdfs-site.xml
ADD ./conf/hadoop/core-site.xml /etc/hadoop/core-site.xml
ADD ./conf/hadoop/mesos-site.xml /etc/hadoop/mesos-site.xml

# java
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
RUN update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

# zeppelin
RUN wget -q https://s3.amazonaws.com/downloads.mesosphere.io/zeppelin/assets/zeppelin-0.6.0-incubating-SNAPSHOT.tar.gz \
  && tar xf zeppelin-*.tar.gz && rm zeppelin-*.tar.gz
WORKDIR /zeppelin-0.6.0-incubating-SNAPSHOT
ADD zeppelin-env.sh conf/zeppelin-env.sh

CMD ["bin/zeppelin.sh", "start"]
