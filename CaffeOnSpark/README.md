# CaffeOnSpark (unofficial)

## Supported tags and respective Dockerfile links

1. CPU-only [cpu](cpu-mode)
2. GPU [cuda](gpu-mode)
3. intel-mkl [intel](intel-mode)

## Overview

### What's CaffeOnSpark?

CaffeOnSpark brings deep learning to Hadoop and Spark clusters. By combining salient features from deep learning framework [Caffe](https://github.com/BVLC/caffe) and big-data frameworks [Apache Spark](http://spark.apache.org/) and [Apache Hadoop](http://hadoop.apache.org/), CaffeOnSpark enables distributed deep learning on a cluster of GPU and CPU servers.

As a distributed extension of Caffe, CaffeOnSpark supports neural network model training, testing, and feature extraction. Caffe users can now perform distributed learning using their existing LMDB data files and minorly adjusted network configuration (as [illustrated](../master/data/lenet_memory_train_test.prototxt#L10-L12)).

CaffeOnSpark is a Spark package for deep learning. It is complementary to non-deep learning libraries MLlib and Spark SQL. CaffeOnSpark's Scala API provides Spark applications with an easy mechanism to invoke deep learning (see [sample](../master/caffe-grid/src/main/scala/com/yahoo/ml/caffe/examples/MyMLPipeline.scala)) over distributed datasets.

CaffeOnSpark was developed by Yahoo for [large-scale distributed deep learning on our Hadoop clusters](http://yahoohadoop.tumblr.com/post/129872361846/large-scale-distributed-deep-learning-on-hadoop) in Yahoo's private cloud. It's been in use by Yahoo for image search, content classification and several other use cases.

### Why CaffeOnSpark?

CaffeOnSpark provides some important benefits (see [this blog](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep)) over alternative deep learning solutions.

- It enables model training, test and feature extraction directly on Hadoop datasets stored in HDFS on Hadoop clusters.
- It turns your Hadoop or Spark cluster(s) into a powerful platform for deep learning, without the need to set up a new dedicated cluster for deep learning separately.
- Server-to-server direct communication (Ethernet or InfiniBand) achieves faster learning and eliminates scalability bottleneck.
- Caffe users' existing datasets (e.g. LMDB) and configurations could be applied for distributed learning without any conversion needed.
- High-level API empowers Spark applications to easily conduct deep learning.
- Incremental learning is supported to leverage previously trained models or snapshots.
- Additional data formats and network interfaces could be easily added.
- It can be easily deployed on public cloud (ex. AWS EC2) or a private cloud.

## How to use this Docker image

### Installation

First pull this image

```
docker pull cimenx/caffeonspark:{mode}
```

### Running

To run a single Solr server:

```
$ docker run --name my_caffeonspark -d -p 8983:8983 -t cimenx/caffeonspark
$ docker exec my_caffeonspark start-master.sh
```
