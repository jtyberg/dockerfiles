FROM python:3.6-slim

MAINTAINER Justin Tyberg <justin.tyberg@gmail.com>

# See: http://docs.opencv.org/3.1.0/d7/d9f/tutorial_linux_install.html

RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential \
      cmake \
      libavcodec-dev \
      libavformat-dev \
      libgtk2.0-dev \
      libswscale-dev \
      libtbb2 \
      libtbb-dev \
      libjpeg-dev \
      libpng-dev \
      libtiff-dev \
      libjasper-dev \
      python3-dev \
      unzip \
      wget \
      && \
    rm -rf /var/lib/apt/lists/*

RUN pip3 install --no-cache-dir \
      numpy

ENV OPENCV_VERSION 3.1.0
RUN cd /root && \
    wget https://github.com/Itseez/opencv/archive/"$OPENCV_VERSION".zip && \
    unzip "$OPENCV_VERSION".zip && \
    cd /root/opencv-"$OPENCV_VERSION" && \
    mkdir build && cd build && \
    cmake \
      -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D PYTHON3_EXECUTABLE=/usr/local/bin/python \
      -D INSTALL_C_EXAMPLES=OFF \
      -D INSTALL_PYTHON_EXAMPLES=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D WITH_FFMPEG=NO \
      -D WITH_IPP=NO \
      -D WITH_OPENEXR=NO \
      .. && \
    VERBOSE=1 make && \
    make install && \
    rm /root/"$OPENCV_VERSION".zip && \
    rm -rf /root/opencv-"$OPENCV_VERSION"
