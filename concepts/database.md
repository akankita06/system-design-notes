## Denormalized DB
- Fast reads and writes (schema less)
- Schema less - no validation on db layer so faster. Applications take care of validating data schema.
- Identify access patterns and model data for dynamo db around that
- Entire data collection required to an access pattern together

## Document DB
- indexes can be created on any field or sub-field of the document
- lot of data
- vast variety of attributes (differing from item to item)
- elasticsearch is a special case of document DB
- key-value pair
  - retrieve one record
  - Like map or dictionary


## Columnar DB
- retrieve multiple related records
- Like a 2d hash table, value of reach hash is a b tree
- Analogy - library - u forest define which section/ collection u want the book from ( say fiction). Within that collection the values are sorted based on an index. U can then fetch the record or take off records u want.
![Columnar DB](https://github.com/akankita06/system-design-notes/blob/main/images/columnarDB.png)


## HBase
- **resources**
    - [guru 99 blog](https://www.guru99.com/hbase-architecture-data-flow-usecases.html)
    - [hbase read and write path](https://data-flair.training/blogs/hbase-operations/)
    - [hbase read write path - better explanation](https://acadgild.com/blog/read-write-operations-hbase)
   
- **resources**
    - [guru 99 blog](https://www.guru99.com/hbase-architecture-data-flow-usecases.html)
    - [hbase read and write path](https://data-flair.training/blogs/hbase-operations/)
    - [hbase read write path - better explanation](https://acadgild.com/blog/read-write-operations-hbase)
    
- **resources**
    - [guru 99 blog](https://www.guru99.com/hbase-architecture-data-flow-usecases.html)
    - [hbase read and write path](https://data-flair.training/blogs/hbase-operations/)
    - [hbase read write path - better explanation](https://acadgild.com/blog/read-write-operations-hbase)
    
- **components**
  ![HBase Components](https://github.com/akankita06/system-design-notes/blob/main/images/hdfs1.png)
  - HMaster
  - HRegionServer
  - HRegions
  - Zookeeper

- **architecture**
- master-slave architecture

- **read-write path**
  ![HBase read write path](https://github.com/akankita06/system-design-notes/blob/main/images/hdfs2.png)

- **write path**
    - write imp. logs to write ahead log (for fault tolerance). Later if any error occurs, HBase looks into WAL
    - data to be written is forwarded to memstore (RAM of the data node) as soon as log entry is done.
        - all data is written in memstore - faster than RDBMS
    - data is dumped into HFile, where actual data is stored in HDFS. If memcache is full, data is stored in HFile directly.
    - ack is sent to client
    
- **read path**
    - zookeeper has location for META table which is present in HRegion server. When client requests zookeeper, it gives the address for the table
    - from META table of HRegionServer, client gets region address of table where data is present
    - in HRegion, check BlockCache for data from prev read
    - if data not found, search MemStore (since data would have been written to HFile sometime back)
    - if data not found, read from HFile + write to BlockCache for future reads
