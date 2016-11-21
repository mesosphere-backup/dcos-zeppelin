FROM mesosphere/mesos:1.0.11.0.1-2.0.93.ubuntu1404

RUN apt-get update && \
    apt-get install -y software-properties-common
RUN apt-get update && \
    apt-get install -y \
    wget \
    tar

# hadoop config
ENV HADOOP_CONF_DIR /etc/hadoop
ADD ./conf/hadoop/hdfs-site.xml /etc/hadoop/hdfs-site.xml
ADD ./conf/hadoop/core-site.xml /etc/hadoop/core-site.xml
ADD ./conf/hadoop/mesos-site.xml /etc/hadoop/mesos-site.xml

# Oracle JDK8
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
RUN update-alternatives --set java /usr/lib/jvm/java-8-oracle/jre/bin/java

# Zeppelin
RUN wget http://apache.claz.org/zeppelin/zeppelin-0.6.2/zeppelin-0.6.2-bin-all.tgz
RUN tar xzvf zeppelin-0.6.2-bin-all.tgz
WORKDIR /zeppelin-0.6.2-bin-all
ADD zeppelin-env.sh conf/zeppelin-env.sh

# R : https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04
RUN sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list'
RUN gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
RUN gpg -a --export E084DAB9 | sudo apt-key add -

RUN apt-get update && \
    apt-get install -y \
    r-base \
    ed \
    liblzma-dev \
    libssl-dev \
    curl \
    libcurl4-openssl-dev

RUN R CMD javareconf
RUN R -e "install.packages('devtools', repos = 'http://cran.us.r-project.org')"
RUN R -e "install.packages('knitr', repos = 'http://cran.us.r-project.org')"
RUN R -e "install.packages('ggplot2', repos = 'http://cran.us.r-project.org')"
RUN R -e "install.packages('RWeka', repos = 'http://cran.us.r-project.org')"
RUN R -e "install.packages(c('devtools','mplot', 'googleVis'), repos = 'http://cran.us.r-project.org'); require(devtools); install_github('ramnathv/rCharts')"
RUN R -e "install.packages(c('glmnet', 'pROC', 'data.table', 'caret', 'sqldf', 'wordcloud'), repos = 'http://cran.us.r-project.org')"
RUN cd ./interpreter/spark/R/lib/SparkR && R -e "devtools::install('.')"

# Python
RUN apt-get update && \
    apt-get install -y \
    python \
    python-dev \
    python-pip \
    python-virtualenv \
    python-matplotlib \
    python-pandas

RUN pip install \
    py4j \
    numpy \
    matplotlib \
    scipy \
    pandas

# Slimming down Docker container
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["bin/zeppelin.sh", "start"]
