---
layout: post
title:  "Using Ansible to Setup Your on Premise Solr Cloud Cluster"
date:   2018-01-06 00:18:23 +0700
categories: [ansible, solr, cloud, scaling, distribution]
---

There comes a day when a single search index with a single shard deployed on a single node is no longer enough for satisfying the performance requirements of a growing system.
In this article I will not go into the details of discovering the right configuration which has to be put in place for a given context. This would make an entire article in itself. 

All I can say, very briefly, is that there is no magic formula which can be generically applied to all the applications which are using search solutions. 
It always depends on multiple factors which have to be discovered with the right measurements and tests. 

You have no choice than to take your search data, put it in an environment where you can test various combinations of nodes, collections, sharding and replication numbers and break each setup with production like queries, until the measurements fit the required ones. 

Among the factors which will influence the configuration you will end up with are:
 - the number of concurrent reads
 - the types of the reads 
 - the number of concurrent writes
 - the types of the writes
 - the size of your data
 - what other processes happen behind the scenes (services which trigger deletions, exports, you name it)
 - and of course the defined NFR(Non Functional Requirements) for your system either if it is a 5s or 0.5s response time 

Bellow I will describe how you can setup collections splitted into two shards, replicated on 3 nodes out of which one would be the master and the other two the slaves.

For a good start, I recommend the official documentation from apache: 
    [Solr Cloud](https://lucene.apache.org/solr/guide/6_6/solrcloud.html)
    [Taking Solr to Production](https://lucene.apache.org/solr/guide/6_6/taking-solr-to-production.html)
 
Solr Cloud does not maintain the cluster configurations. Therefore, their recommendation is to use an ensemble of zookeeper services for that:  

    "For a ZooKeeper service to be active, there must be a majority of non-failing machines that can communicate with each other. To create a deployment that can tolerate the failure of F machines, you should count on deploying 2xF+1 machines. Thus, a deployment that consists of three machines can handle one failure, and a deployment of five machines can handle two failures. Note that a deployment of six machines can only handle two failures since three machines is not a majority.

    For this reason, ZooKeeper deployments are usually made up of an odd number of machines."

                                     — ZooKeeper Administrator's Guide
                                        http://zookeeper.apache.org/doc/r3.4.10/zookeeperAdmin.html

In our example we will install 3 zookeeper instances.

For balancing the load on our local configuration, we will use [HAProxy](http://www.haproxy.org/) which will be configured to elect the nodes based on the round robin algorithm.

The goal would be to achieve something like this: 

![]({{ site.url }}/static/img/solr-cloud/solr-cloud.jpg)


First step consists in configuring the necessary servers with the desired configurations:

    For example, a zookeeper instance can be set with 2 CPU and 1024 GB of memory
    
        ![]({{ site.url }}/static/img/solr-cloud/zookeeper-vagrant.png)


Loading...


 


