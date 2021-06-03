---
layout: post
title:  "Elasticsearch Braindump"
date:   2019-09-01 00:18:23 +0700
categories: [search, elasticsearch]
---

### Context

This small blog post is meant to just enumerate a list of tips for future me. 

I am making the information public for the case someone else might find them useful.

### Tips and Guidelines

### Configurations

Disable swapping and enable bootstrap.memory_lock for ensuring performance and node stability. This will prevent operating systems from swapping the allocated memory for ES.

As per ES documentation: "Most operating systems try to use as much memory as possible for file system caches and eagerly swap out unused application memory. This can result in parts of the JVM heap or even its executable pages being swapped out to disk."

Enable shard allocation awareness for allowing ES know about the hardware configuration. This way it can ensure that the primary shard and its replica shards are spread across different physical servers, racks, or zones, to minimize the risk of losing all shard copies at the same time.

Reducing the number of primary shards from the default 5 to 1 (see Sharding for details)

### AWS Recommendations

The recommended AWS instance type is i3.2xlarge because it has locally attached 1900 GiB NVMe SSDs, which means the disk access will increase the overall ES performance.

Another good practice is to distribute the nodes across multiAZ, and use shard allocation awareness to ensure that each shard has copies in more than one availability zone. Do not span a cluster across regions. Elasticsearch expects that node-to-node connections within a cluster are reasonably reliable and offer high bandwidth and low latency, and these properties do not hold for connections between regions

If networking is a bottleneck, avoid instance types with networking labelled as Moderate or Low.

Consider enabling termination protection for all of your data and master-eligible nodes.

### Sharding

Data in ES is organized into indices. Each index is made up of one or more shards. Each shard is an instance of a Lucene index. 

A Lucene index is a self contained search engine that indexes and handles queries for a subset of the data in an Elasticsearch cluster.
This means each shard has an overhead in terms of memory, CPU and file handles.

As data is written to a shard, it is periodically published into new immutable Lucene segments on disk, and it is at this time it becomes available for querying.

Choosing between one node and 10 shards or 10 nodes each with a shard makes no difference at the Lucene level. But having the wrong design can lead to performance issues on the long run.
 
From the domain perspective, if the information can be split logically and evenly, then maybe splitting in multiple indices makes more sense.

If deletion is required, it is easier and faster to delete an entire index.

For searching, aliases can be used for increasing the response performance(ex. by entity_id).

From ES the recommendation of a shard size is in 10s of gigabytes. This means if data is less than that, then having a single index shall be preferred.

For ensuring a good health of the ES cluster, the recommendation is to keep the number of shards per node below 20 per GB it has configured. This means that a node with a 20GB heap should have maximum 400 shards. Better the have the number bellow this value. Too many shards can kill performance.

Use 1 primary shards for all indices and implement [rollover index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-rollover-index.html) using [Index Lifecycle Management](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html).

### IML - Index Lifecycle Management

Spin up a new index when an index reaches a certain size or number of documents

Create a new index each day, week, or month and archive previous ones

Delete stale indices to enforce data retention standards

### Resources

[Disable Swapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration-memory.html)

[AWS Instances Comparison](https://instances.vantage.sh/)

[How Many Shards to Have](https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster)

[AWS Best Practices](https://www.elastic.co/guide/en/elasticsearch/plugins/current/cloud-aws-best-practices.html)

[Allocation Awareness](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/allocation-awareness.html)


