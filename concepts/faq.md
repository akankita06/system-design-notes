### How would you count ip addresses for log files?
- old
  - mapper-reducer
  - mapper - different servers on which a function is run
- but these days- Spark
    - check the implementation
&nbsp;

### Why is hashmap used to store data in-memory and why is tree used for data storage for RDBMS?
- sparse storage - hashmap stored as array. With data in DBMS, storage would be random ?
- access pattern - hashmap - random access; DBMS - range based query
&nbsp;

### Why TCP connection open and close is resource intensive
- tldr: connections in **time_wait** to close the connection take up all connection capacity and no new connection can be opened as a result.[ref](https://stackoverflow.com/questions/7844122/are-tcp-connections-resource-intensive)
- for nearby friends or chat messages:
  - keep connections open till session is active
  - close the connection once the session closes (no longer online)
&nbsp;

### Library vs service
[Library vs service](https://blogs.gartner.com/eric-knipp/2013/03/20/libraries-vs-services/)
- Library - gets compiled within your program
- Service - access service to get the data, compiled independently of your program
  ![libsvc1](https://github.com/akankita06/system-design-notes/blob/main/images/libsvc1.png)
  ![libsvc2](https://github.com/akankita06/system-design-notes/blob/main/images/libsvc1.png)
 &nbsp;
 
 ### Table vs database
- table - object inside DB
- DB has - tables, indexes, stored procedures, etc
&nbsp;

### Table vs database
- table - object inside DB
- DB has - tables, indexes, stored procedures, etc
&nbsp;

### Table vs database
- table - object inside DB
- DB has - tables, indexes, stored procedures, etc
&nbsp;

### Recursive vs iterative
- **Recursive vs Iterative**
  - [Stack overflow](https://stackoverflow.com/questions/2651112/is-recursion-ever-faster-than-looping)
&nbsp;

- **Recursion**
  - expensive compared to general
  - requires allocation of new stack frame (variables, function, everything)
  - stack/iteration - only the required values are needed
&nbsp;

- **Recursion improvement:**
  - transform tail calls into jumps instead of function calls
  - **Tail call optimization**
&nbsp;

- In some env. (eg: functional), recursion is faster
- In imperative language - iteration is faster
&nbsp;

### Thread vs process

- [Threads, processes, Python threading and processing](https://blog.floydhub.com/multiprocessing-vs-threading-in-python-what-every-data-scientist-needs-to-know/)
- [Thread vs process](https://stackoverflow.com/questions/200469/what-is-the-difference-between-a-process-and-a-thread) \- 1st 2 ans.
- [Multithread over multiprocess](https://stackoverflow.com/questions/617787/why-should-i-use-a-thread-vs-using-a-process)
- [Concurrency, threading and parallielism explained](https://www.youtube.com/watch?v=olYdb0DdGtM&list=PLp-i2HHC9rDnS_0BDNLnjT1hlm1DA1JoZ&index=4&t=591s)
- [Concurrency vs parallelism](https://medium.com/@itIsMadhavan/concurrency-vs-parallelism-a-brief-review-b337c8dac350)
- [Intro to concurrency](https://www.youtube.com/watch?v=iKtvNJQoCNw&list=PLp-i2HHC9rDnS_0BDNLnjT1hlm1DA1JoZ&index=1)
- [Python threading](https://www.youtube.com/watch?v=IEEhzQoKtQU&t=197s)
- [Python multiprocessing](https://www.youtube.com/watch?v=fKl2JW_qrso)

- **Threads**
  - run in shared memory space
  - entity within a process that can be secluded for execution
  - all threads of a process share its virtual address space and system resources
  - thread scheduling algorithm
&nbsp;

- **Processes**
  - run in separate memory spaces
  - provides all resources needed to execute a program
  - has virtual address space, executable code, env variables, etc
  - each process starts with a single thread, but can create additional threads
  - process scheduling algorithm
&nbsp;

- **Multithreading**
  - has multiple threads
  - for IO/network intensive operations
  - since all threads use same memory space, therefore multithreading needs synchronization mechanisms like locks, sepmaphores, etc for mutable data
&nbsp;

- **Multiprocessing**
  - each process has its own memory space
  - for CPU intensive operations
  - so no need for any synchronization mechanisms since data for one process won't be mutated by other process
&nbsp;

- **Advantages of multiple threads over multiple processes**
  - **inter-thread communication** (sharing data, etc.) is simpler to program than multiprocess communication
  - **context switches** between threads is faster than between processes (quicker for OS to stop a thread to run another, than stop a process to run another)
&nbsp;

- **Disadvantages of multithread**
  - no security between threads
  - one thread can mutate another thread's data
  - if one thread block, all threads in task block
&nbsp;

- **Global interpreter lock**
  - Python oddity
  - prevents 2 threads from writing to same memory location
  - only 1 threa can access Python objects at a time to execute Python bytecode
  - provides thread-safe memory management
  - **interpreter runs instructions serially**
  - but now can't utilize multiple CPU cores (for multiprocessing)
&nbsp;

### Idempotence
- **RESTful service point of view**
  - making multiple identical requests has the same effect as making a single request
  - clients can make same call repeatedly while producing the same result
  - while idempotent operations produce the same result on the server (no side effects), the response itself may not be the same (eg: a resource's state may change between requests)
  &nbsp;
  
- **wiki**
  - prop of certain operations that can be applied multiple times without changing the reuslt beyond the initial application
  - HTTP requests that are idempotent
 &nbsp;
 
- **benefits**
  - request failures happen. Provide users a safe way to retry requests.
  ```POST /BankAccount/AddFunds
       { 'value':1000, 'token':'TX123' }```
       
 
  


