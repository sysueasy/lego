# Elasticsearch

## Getting Started

Elasticsearch/Logstash/Kibana stack [https://www.elastic.co/downloads](https://www.elastic.co/downloads)

随机生成 JSON 的工具：[https://www.json-generator.com/](https://www.json-generator.com/)

## Basic Concepts

Near Realtime, NRT: Elasticsearch provides data manipulation and search capabilities in near real time. By default, you can expect a one second delay \(refresh interval\) from the time you index/update/delete your data until the time that it appears in your search results.

Cluster: by default "elasticsearch", named e.g. logging-dev, logging-stage, logging-prod, it is valid and perfectly fine to have a cluster with only a single node in it.

Node: starting a single node will by default form a new single-node cluster named elasticsearch.

Index: An index is a collection of documents that have somewhat similar characteristics.

Document: A document is a basic unit of information that can be indexed. **This document is expressed in JSON**.

Shards: An index can potentially store a large amount of data that can exceed the hardware limits of a single node. Elasticsearch provides the ability to subdivide your index into multiple pieces called shards.

Replicas: It is very useful and highly recommended to have a failover mechanism in case a shard/node somehow goes offline or disappears for whatever reason. An index can be replicated zero \(meaning no replicas\) or more times. Once replicated, each index will have primary shards \(the original shards that were replicated from\) and replica shards \(the copies of the primary shards\).

By default, each index in Elasticsearch is allocated 5 primary shards and 1 replica which means that if you have at least two nodes in your cluster, your index will have 5 primary shards and another 5 replica shards \(1 complete replica\) for a total of 10 shards per index.

It is important to understand that once you get your search results back, Elasticsearch is completely done with the request and does not maintain any kind of server-side resources or open cursors into your results. This is in stark contrast to many other platforms such as SQL wherein you may initially get a partial subset of your query results up-front and then you have to continuously go back to the server if you want to fetch \(or page through\) the rest of the results using some kind of stateful server-side cursor.

## Add, Replace, Update

Whenever we do an update, Elasticsearch deletes the old document and then indexes a new document with the update applied to it in one shot.

The pretty parameter, again, just tells Elasticsearch to return pretty-printed JSON results.

That pattern can be summarized as follows:

&lt;HTTP Verb&gt; /&lt;Index&gt;/&lt;Type&gt;/&lt;ID&gt;

