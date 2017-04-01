FROM python:3.6-slim

MAINTAINER Justin Tyberg <justin.tyberg@gmail.com>

RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential \
      ca-certificates \
      git \
      libatlas-base-dev \
      libopenblas-dev \
      libopencv-dev \
      && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 install --no-cache-dir \
      numpy

ENV MXNET_VERSION 0.9.3
RUN cd /root && git clone --recursive https://github.com/dmlc/mxnet.git && \
    cd mxnet && \
    git checkout tags/v$MXNET_VERSION -b $MXNET_VERSION && \
    cp make/config.mk config.mk && \
    echo "USE_BLAS=openblas" >> config.mk && \
    echo "ADD_CFLAGS += -I/usr/include/openblas" >> config.mk && \
    echo "ADD_LDFLAGS += -lopencv_core -lopencv_imgproc" >> config.mk && \
    make -j$(nproc)

RUN cd /root/mxnet/python && python3 setup.py install
RUN echo "export PYTHONPATH=/root/mxnet/python:$PYTHONPATH" >> /root/.bashrc
