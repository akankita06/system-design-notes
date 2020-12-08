# Scaling

## Load balancing

- Between clients & application servers
- Between application servers & backend servers
- Round robin - simple but does not consider load on each server. For that, use a smart load balancer
- Functions:
    - Fault tolerance - identify + take out dead server from rotation
    - Balancing load

&nbsp;

## Performance

**Search**

- early stop of posting list scanning
- sharding to reduce length of posting list
- keep index in-memory on index servers
- number of search results depending on viewport (pagination)
&nbsp;

**Image/video uploads**

- Separate the services for uploads/write requests and read requests

&nbsp;

## Fault tolerance

- Replication
- Load balancer can help
- Reverse index server
    - For search (Twitter Search)
- How to keep replicas in-sync (Elastic search)

&nbsp;

## DB Sharding

**Issues**

- hot partition
- uneven storage distribution

**Specific cases**

- for search index - [Twitter Search](joplin://d92408e785134758bc584cbf0567aacc)
