FROM debian:10

LABEL author=thanhhoang@hcmut.edu.vn

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    openjdk-11-jdk \
    net-tools \
    curl \
    netcat \
    gnupg \
    libsnappy-dev \
    python3 python3-pip\
    python3-setuptools \ 
    python python-pip \
    && rm -rf /var/lib/apt/lists/*

#RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
 #   libglib2.0-0 libxext6 libsm6 libxrender1 \
  #  git mercurial subversion

#RUN curl -fSL https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh -o ~/anaconda.sh && \
#    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
#    rm ~/anaconda.sh && \
#    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
#    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
#    echo "conda activate base" >> ~/.bashrc

ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/

RUN curl -O https://dist.apache.org/repos/dist/release/hadoop/common/KEYS

RUN gpg --import KEYS

ENV HADOOP_VERSION 3.2.2

ENV SPARK_VERSION=3.1.2
ENV SPARK_HADOOP_VERSION=3.2

ENV HADOOP_URL https://www.apache.org/dist/hadoop/common/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz

RUN set -x \
    && curl -fSL "$HADOOP_URL" -o /tmp/hadoop.tar.gz \
    && curl -fSL "$HADOOP_URL.asc" -o /tmp/hadoop.tar.gz.asc \
    && gpg --verify /tmp/hadoop.tar.gz.asc \
    && tar -xvf /tmp/hadoop.tar.gz -C /opt/ \
    && rm /tmp/hadoop.tar.gz*

RUN ln -s /opt/hadoop-$HADOOP_VERSION/etc/hadoop /etc/hadoop

RUN mkdir /opt/hadoop-$HADOOP_VERSION/logs

RUN mkdir /hadoop-data

ENV HADOOP_HOME=/opt/hadoop-$HADOOP_VERSION
ENV HADOOP_CONF_DIR=/etc/hadoop
ENV YARN_CONF_DIR=/etc/hadoop/
ENV PATH=$HADOOP_HOME/bin/:$PATH

ENV MULTIHOMED_NETWORK=1

RUN curl -fSL https://mirror.downloadvn.com/apache/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${SPARK_HADOOP_VERSION}.tgz -o spark-${SPARK_VERSION}-bin-hadoop${SPARK_HADOOP_VERSION}.tgz\
    && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${SPARK_HADOOP_VERSION}.tgz \
    && mv spark-${SPARK_VERSION}-bin-hadoop${SPARK_HADOOP_VERSION} /spark \
    && rm spark-${SPARK_VERSION}-bin-hadoop${SPARK_HADOOP_VERSION}.tgz \
    && cd /

ENV PATH=spark/bin:$PATH
# Fix the value of PYTHONHASHSEED. This is needed when you use Python 3.3 or greater
ENV PYTHONHASHSEED 1

ENV USER=root

#RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

#RUN python3 get-pip.py

RUN apt-get update && apt-get install build-essential python-dev python3-dev -y

RUN python3 -m pip install --upgrade pip

RUN pip3 install wheel && python3 -m pip install --upgrade Pillow

RUN yes | pip3 install Cython numpy matplotlib

RUN pip3 install pyspark

RUN pip3 install kafka-python

ENV PYSPARK_PYTHON=python3
ENV PYSPARK_DRIVER_PYTHON=python3


ENV SCALA_VERSION=2.12.12
RUN curl -fSL www.scala-lang.org/files/archive/scala-${SCALA_VERSION}.deb -o /tmp/scala.deb \
                && dpkg -i /tmp/scala.deb \
		&& rm /tmp/scala.deb

ADD entrypoint.sh /entrypoint.sh

RUN chmod a+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
