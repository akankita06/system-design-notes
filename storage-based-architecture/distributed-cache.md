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
