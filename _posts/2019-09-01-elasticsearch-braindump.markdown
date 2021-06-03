---
layout: post
title:  "Elasticsearch Braindump"
date:   2019-09-01 00:18:23 +0700
categories: [search, elasticsearch]
---

### Context

This small blog post is meant to just enumerate a list of tips for future me. 

I am making the information public for the case someone else might find them useful.

### Tips and Guidelines (WIP)

- Disable swapping and enable bootstrap.memory_lock for ensuring performance and node stability. This will prevent operating systems from swapping the allocated memory for ES.

As per ES documentation: "Most operating systems try to use as much memory as possible for file system caches and eagerly swap out unused application memory. This can result in parts of the JVM heap or even its executable pages being swapped out to disk."

- Choosing between one node and 10 shards or 10 nodes each with a shard: 

From Lucene shard level, there is no difference between the two options. 

From the domain perspective, if the information can be split logically and evenly, then maybe splitting in multiple indices makes more sense.

If deletion is required, it is easier and faster to delete an entire index.

For searching, aliases can be used for increasing the response performance(ex. by entity_id).

From ES the recommendation of a shard size is in 10s of gigabytes. This means if data is less than that, then having a single index shall be preferred.

- Sharding(WIP)

- AWS Best Practices

The recommended AWS instance type is i3.2xlarge because it has locally attached 1900 GiB NVMe SSDs, which means the disk access will increase the overall ES performance.

Another good practice is to distribute the nodes across multiAZ, and use shard allocation awareness to ensure that each shard has copies in more than one availability zone.
 

### Resources

[Disable Swapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration-memory.html)

[AWS Instances Comparison](https://instances.vantage.sh/)

[How Many Shards to Have](https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster)

[AWS Best Practices](https://www.elastic.co/guide/en/elasticsearch/plugins/current/cloud-aws-best-practices.html)

[Allocation Awareness](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/allocation-awareness.html)


