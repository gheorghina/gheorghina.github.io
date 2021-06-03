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

- Disable swapping and enable mlockall for ensuring performance and node stability. This will prevent operating systems from swapping the allocated memory for ES.

As per ES documentation: "Most operating systems try to use as much memory as possible for file system caches and eagerly swap out unused application memory. This can result in parts of the JVM heap or even its executable pages being swapped out to disk."

- Choosing between one node and 10 shards or 10 nodes each with a shard: 

From Lucene shard level, there is no difference between the two options. 

From the domain perspective, if the information can be split logically and evenly, then maybe splitting in multiple indices makes more sense.

If deletion is required, it is easier and faster to delete an entire index.

For searching, aliases can be used for increasing the response performance(ex. by entity_id).

From ES the recommendation of a shard size is in 10s of gigabytes. This means if data is less than that, then having a single index shall be preferred.

- Sharding(WIP)
 

### Some Resources

[Disable swapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/setup-configuration-memory.html)


