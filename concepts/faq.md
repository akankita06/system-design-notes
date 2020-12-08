**How would you count ip addresses for log files?**
- old
  - mapper-reducer
  - mapper - different servers on which a function is run
- but these days- Spark
    - check the implementation
&nbsp;

**Why is hashmap used to store data in-memory and why is tree used for data storage for RDBMS?**
- sparse storage - hashmap stored as array. With data in DBMS, storage would be random ?
- access pattern - hashmap - random access; DBMS - range based query
&nbsp;

**Why TCP connection open and close is resource intensive**
- tldr: connections in **time_wait** to close the connection take up all connection capacity and no new connection can be opened as a result.[ref](https://stackoverflow.com/questions/7844122/are-tcp-connections-resource-intensive)
- for nearby friends or chat messages:
  - keep connections open till session is active
  - close the connection once the session closes (no longer online)
&nbsp;

**Library vs service**
[Library vs service](https://blogs.gartner.com/eric-knipp/2013/03/20/libraries-vs-services/)
- Library - gets compiled within your program
- Service - access service to get the data, compiled independently of your program
