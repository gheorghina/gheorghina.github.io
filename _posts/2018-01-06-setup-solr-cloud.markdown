---
layout: post
title:  "Using Ansible to Setup Your on Premise Solr Cloud Cluster"
date:   2018-01-06 00:18:23 +0700
categories: [ansible, solr, cloud, scaling, distribution]
---

There comes a day when a single search index with a single shard deployed on a single node is no longer enough for satisfying the performance requirements of a growing system.
In this article I will not go into the details of discovering the right configuration which has to be put in place for a given context. This will make an entire article in itself. 

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

Bellow I will describe how you can setup a collection splitted into two shards, replicated on 3 nodes.

For a good start, I recommend the official documentation from apache: [Solr Cloud](https://lucene.apache.org/solr/guide/6_6/solrcloud.html)

Loading...


 


