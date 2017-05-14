FROM debian:jessie

RUN apt-get update \
  && apt-get install -y --no-install-recommends locales \
  && dpkg-reconfigure -f noninteractive locales \
  && locale-gen C.UTF-8 \
  && /usr/sbin/update-locale LANG=C.UTF-8 \
  && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && locale-gen \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
  && apt-get install -yq --no-install-recommends \
     wget \
     ca-certificates \
     bzip2 \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# Python/Conda
ENV CONDA_DIR /opt/conda
ENV PATH ${CONDA_DIR}/bin:$PATH
RUN cd /tmp \
  && mkdir -p ${CONDA_DIR} \
  && wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.3.11-Linux-x86_64.sh \
  && echo "b9fe70ce7b6fa8df05abfb56995959b897d0365299f5046063bc236843474fb8 *Miniconda3-4.3.11-Linux-x86_64.sh" | sha256sum -c - \
  && /bin/bash Miniconda3-4.3.11-Linux-x86_64.sh -f -b -p ${CONDA_DIR} \
  && rm Miniconda3-4.3.11-Linux-x86_64.sh \
  && ${CONDA_DIR}/bin/conda install --quiet --yes conda==4.3.11 \
  && ${CONDA_DIR}/bin/conda config --system --add channels conda-forge \
  && ${CONDA_DIR}/bin/conda config --system --set auto_update_conda false \
  && conda clean -tipsy

# Java
RUN echo "deb http://http.debian.net/debian jessie-backports main" >> /etc/apt/sources.list
RUN apt-get -y update \
  && apt-get install -y -t jessie-backports --no-install-recommends openjdk-8-jre-headless \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
RUN update-java-alternatives -s java-1.8.0-openjdk-amd64

# Spark with Hadoop
ARG APACHE_SPARK_VERSION=2.1.1
ENV APACHE_SPARK_VERSION ${APACHE_SPARK_VERSION}
ARG HADOOP_VERSION=2.7
ENV HADOOP_VERSION ${HADOOP_VERSION}
RUN cd /tmp \
  && wget -q http://d3kbcqa49mib13.cloudfront.net/spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
  && echo "372ac4f73221c07696793101007a4f19e31566d1f0d9bd0e5205b6fb5b45bfc2 *spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" | sha256sum -c - \
  && tar xzf spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz -C /usr/local \
  && rm spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz
RUN cd /usr/local && ln -s spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark
ENV SPARK_HOME /usr/local/spark
ENV SPARK_OPTS --driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info
ENV PYTHONPATH ${SPARK_HOME}/python:${SPARK_HOME}/python/lib/py4j-0.10.4-src.zip
# http://blog.stuart.axelbrooke.com/python-3-on-spark-return-of-the-pythonhashseed
ENV PYTHONHASHSEED 0

WORKDIR $SPARK_HOME
