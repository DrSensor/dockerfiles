## Reference:
## https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_local
## https://github.com/yahoo/CaffeOnSpark/blob/master/caffe-grid/src/test/python/PythonTest.sh

FROM ubuntu:14.04
MAINTAINER Fahmi Akbar Wildana <fahmi.akbar.w@mail.ugm.ac.id>

ENV pwd /root

## Install git, wget, bc and dependencies
# https://github.com/Kaixhin/dockerfiles/blob/master/caffe/deps/Dockerfile
RUN apt-get update && apt-get install -y \
  git \
  wget \
  bc \
  cmake \
  libatlas-base-dev \
  libatlas-dev \
  libboost-all-dev \
  libopencv-dev \
  libprotobuf-dev \
  libgoogle-glog-dev \
  libgflags-dev \
  protobuf-compiler \
  libhdf5-dev \
  libleveldb-dev \
  liblmdb-dev \
  libsnappy-dev \
  python-dev \
  python-pip \
  python-numpy \
  maven \
  software-properties-common \
  gfortran > /dev/null && \
  pip install --upgrade pip

# # Clone Caffe repo and move into it
# RUN cd ${pwd} && git clone https://github.com/BVLC/caffe.git && cd caffe && \
# # Install python dependencies
#   pip install -U -r python/requirements.txt
#
# # Move into Caffe repo
# RUN cd ${pwd}/caffe && \
# # Make and move into build directory
#   mkdir build && cd build && \
# # CMake
#   cmake .. && \
# # Make
#   make -j"$(nproc)" all && \
#   make install
#
# # Add to Python path
# ENV PYTHONPATH=/root/caffe/python:$PYTHONPATH

## Install Oracle Java 8
# https://github.com/dockerfile/java/blob/master/oracle-java8/Dockerfile
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# Get CaffeOnSpark
RUN cd ${pwd} && git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
ENV CAFFE_ON_SPARK ${pwd}/CaffeOnSpark

# Configure CaffeOnSpark
ADD requirements.txt ${CAFFE_ON_SPARK}/caffe-public/python/
RUN cd ${CAFFE_ON_SPARK}/caffe-public/ && \
  pip install -U -r python/requirements.txt


# Install Spark and Hadoop
ADD Makefile.config ${CAFFE_ON_SPARK}/caffe-public/
RUN bash ${CAFFE_ON_SPARK}/scripts/local-setup-hadoop.sh && \
  bash ${CAFFE_ON_SPARK}/scripts/local-setup-spark.sh \

ENV HADOOP_HOME ${pwd}/hadoop-2.6.4
ENV SPARK_HOME ${pwd}/spark-1.6.0-bin-hadoop2.6
ENV PATH ${HADOOP_HOME}/bin:${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${PATH}

# Build CaffeOnSpark
RUN cd ${CAFFE_ON_SPARK} && \
  make build

ENV LD_LIBRARY_PATH ${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib
# ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH}:/usr/local/cuda-7.0/lib64:/usr/local/mkl/lib/intel64/


# Set ~/caffe as working directory
WORKDIR ${CAFFE_ON_SPARK}
