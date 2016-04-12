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
RUN wget http://apache.arvixe.com/incubator/zeppelin/0.5.6-incubating/zeppelin-0.5.6-incubating-bin-all.tgz
RUN tar xzvf zeppelin-0.5.6-incubating-bin-all.tgz
WORKDIR /zeppelin-0.5.6-incubating-bin-all
ADD zeppelin-env.sh conf/zeppelin-env.sh

CMD ["bin/zeppelin.sh", "start"]
