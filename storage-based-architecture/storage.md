## Resources
- [Distributed message queue](https://www.youtube.com/watch?v=iJLL-KPqBpM&ab_channel=SystemDesignInterview)
- [S3](https://www.youtube.com/watch?v=UmWtcgC96X8&ab_channel=TechDummies)
&nbsp;

## Dropbox
- [https://www.youtube.com/watch?v=8HaXU4aNTSg&ab_channel=CodingSimplified](https://www.youtube.com/watch?v=8HaXU4aNTSg&ab_channel=CodingSimplified)
- [tech dummies youtube video](https://www.youtube.com/watch?v=U0xTu6E2CT8&ab_channel=TechDummies)
&nbsp;

## Concepts
- Consistent hashing
- Zookeeper (keep separate ZK instances for separate clusters)
- garbage collection (fragmentation, de-fragmentation)
&nbsp;

- Cassandra
- Hbase
- HDFS as a sink to dump all info for analytics
- Cassandra vs HBase 
- Data replication protocols - gossip (probabilitistic), paxos (consensus)
- Spark streaming + batching
- Graph DB
&nbsp;

**why is cassandra good for high write throughput**
- cassandra is bad for random queries - use search for it
- cassandra is bad for aggregations - use streaming for it
- it is good for patterned queries (Eg: all movies for the user)
