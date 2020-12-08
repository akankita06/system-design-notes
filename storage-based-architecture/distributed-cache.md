## Resources
- [redis architecture (gaurav sen)](https://www.youtube.com/watch?v=U3RkDLtS7uY&t=565s&ab_channel=GauravSen)
- [sdi - distributed cache](https://www.youtube.com/watch?v=iuqZvajTOyA&t=1s&ab_channel=SystemDesignInterview)


## Requirements

- easy to scale horizontally
- store massive amounts of data
- get and set functions


## Other requirements
- metrics to monitor
- deployment stratgey
- how to verify if data is correct


## Solution
- reads vs writes
- master slave vs p2p (pros and cons)
- consistent hashing
- gossip vs master-slave for data replication (trade-offs)
  - gossip - a lot of network traffic for p2p
  - master-slave - single point of failure
  - master-master ?
  

## Considerations
considerations:

- consistency
- failure proof
- cache data expiration (so data does not just sit there)
- eviction policy - LRU
- local vs remote cache
- security (usually, only accessed by internal servers - not exposed to internet, so implement a firewall)
- metrics to monitor - cache hits, misses, QP
&nbsp;

**Solution 1**

The hashmap + linked list are protected by a reader/writer lock. On read, a read lock is grabbed and a timestamp in the node is updated. The node is not moved. When capacity is reached the thread making the request iterates over the list picking the oldest entries to delete. It then deletes the entry from the hashmap and the list. It then grabs a write lock and deletes that node.
&nbsp;

This solution is not great because once capacity is reached, every insert is O(n).
&nbsp;

**Solution 2**

An improvement on the solution above is a clock algorithm. In this, there’s a bit that’s set every time a node is accessed. When a threshold number of entries (less than the capacity) is reached, a second thread, let’s call it the reaper thread, starts up and sets this bit to zero. When a second watermark is reached, the reaper thread wakes up again and iterates over the list again, deleting any entries that have the bit set to zero. If the low watermark is not reached, the two passes are repeated.
&nbsp;

This has the advantage that it cleans up multiple entries and tries to maintain the working set in the cache rather than wait for it to fill up before evicting. If more fidelity than a single bit is needed, multiple bits can be used as a counter. In the multi bit scenario, the reaper thread still sets the counter to zero in the initial pass, but may have to make multiple passes to reap the entries as there may not be enough entries with the counter set to 0. In that case it repeats the pass with the counter set to 1, 2, and so on.
&nbsp;

**on-disk storage data structures**
- log structured merge tree
- b+ tree


## Failure scenarios
- node failure
- n/w packet loss
- n/w partition
- effects of latency (esp. for writes)
- complete vs intermittent outages of data centers

  ### Solutions
  - consensus for writes - send acknowledgement only after some n replicas are written to successfully
  - multiple data centers for disaster scenarios (master-master replication between data centers)
    

## Other resources
- [highly scalable](https://highlyscalable.wordpress.com/2012/09/18/distributed-algorithms-in-nosql-databases/)
- [Dynamo DB paper](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf)
- [Numbers everyone should know](http://highscalability.com/numbers-everyone-should-know)
