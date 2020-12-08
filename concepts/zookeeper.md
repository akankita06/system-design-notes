### Resources
- [zookeeper - Frank Kane](https://www.youtube.com/watch?v=gZj16chk0Ss)
- [zookeeper election](https://www.tutorialspoint.com/zookeeper/zookeeper_leader_election.htm)
&nbsp;

### Use cases**
- which node is master
- which task is assigned to which worker
- which workers are available
- is master is down, elect a new master
- crash detection
&nbsp;

Things to see:
- zookeeper election - through consensus
&nbsp;

### Zookeeper vs load balancer
- ZooKeeper is used for High Availability, but not as a Load Balancer exactly.
- High Availability means, you don't want to loose your single point of contact i.e. your master node. If one master goes down there should be some else who can take care and maintain the same state.
- Load Balancer is used to balance your different kinds of loads like network and application services. If there is a high demand flows for the application, an application load balancer delegates the task to it's available instances. If there is huge calls at network layer, an network load balancer plays an role to handle the situation.
