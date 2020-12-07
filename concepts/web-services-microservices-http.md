
### Microservices

- [defog tech youtube channel](https://www.youtube.com/playlist?list=PLhfHPmPYPPRk5WxsLhQIOEznHEeFJAoVM)
- [tech dummies youtube channel](https://www.youtube.com/playlist?list=PLkQkbY7JNJuDqCFncFdTzGm6cRYCF-kZO)


### Microservices design patterns

- [eudreka youtube](https://www.youtube.com/watch?v=xuH81XGWeGQ&ab_channel=edureka%21)
- [cloud design patterns - microsoft docs](https://docs.microsoft.com/en-us/azure/architecture/patterns/)


- aggregator
- API gateway
- chained or chain of responsibility (synchronous HTTP req/response for messaging)
- asynchronous messaging
- database or shared database
- event sourcing (create events regarding changes in application state)
- branch (simultaneously process req and resp from 2 or more independent microservices)
- command query responsibility seggregator (Segregate operations that read data from operations that update data by using separate interfaces.)
- circuit breaker
- decomposition

### Microservices deployment strategies
[microservices deployment strategies - tech dummies](https://www.youtube.com/watch?v=XJS_GwcLfHc&list=PLkQkbY7JNJuDqCFncFdTzGm6cRYCF-kZO&index=10&ab_channel=TechDummies)
- multiple services per host
- 1 svc per host / VM / container
- serverless

### Application deployment strategies
[deployment-strategies](https://thenewstack.io/deployment-strategies/)
- recreate
- ramped
- blue/green
- canary
- a/b testing
- shadow

### FAAS and event-driven architecture
[defog tech youtube](https://www.youtube.com/watch?v=h-vD_hycQjk&ab_channel=DefogTech)

**FAAS**
- split service into multiple functions
- deployed on some AWS storage
- once the code in the function is run, the function instance is destroyed
- billed only for amount of time that function is used
- job scheduler - schedule an event to be generated by AWS  at the specific times → associate event with your task


**HTTP is a synchronous protocol, even though an HTTP client may use asynchronous I/O when it sends a request.**


### HTTP verbs

**POST**: 
- neither safe nor idempotent
- to create a resource
- Making two identical POST requests will result in two resources containing the same information.

**GET:**
- idempotent
- to read data (not change it)
- do not expose unsafe operations via GET (it should never modify any resources on the server)

**PUT**
- idempotency depends
- to udpate the resource
- if it increases the counter, everytime it is called - not idempotent
- if it only inserts a record if it was not already present, else just modifies it - idempotent

**PATCH**
- only contains changes to the record, not the entire record
- neither safe nor idempotent
- however, a PATCH req can be issued in a way that it is idempotent
- collisions between multiple PATCH req are more dangerous than PUT req.

**DELETE**
- delete a resource
- idempotent - same as PUT


[API design - azure](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/api-design)


### Protocols
[REST vs GraphQL vs gRPC vs webhooks](https://nordicapis.com/when-to-use-what-rest-graphql-webhooks-grpc/)
[Medium article - REST, gRPC, GraphQL](https://medium.com/@saboteurkid/apis-solution-debate-rest-vs-grpc-vs-graphql-d9c25e44d6)

**REST**

- stateless
- defines its interactions through terms standardized in its requests

**gRPC**

- nimble and lightweight system for requesting data
- execute a procedure on a remote server
- functions upon an idea of contracts, in which the negotiation is defined and constricted by the client-server relationship rather than the architecture itself
- popular for IOT devices, etc that require custom contracted communications for low power devices
- to serialize data - protobufs

**GraphQL**

- client determines what data they want, how they want it, and in what format
- reversal of traditional client-server relationship (client kind of takes control)
- benefit - delivers smallest possible req. REST delivers everything at once by default (most complete request)
- good when low data package is preferred

**Webhooks**

- graphQL - option to extend an API; gRPC - retooling a classical approach; Webhook - entirely diff approach to provision resource
- an HTTP POST that is triggered when an event occurs
- data updates are served automatically rather than requested


### Proxy and reverse proxy

**Proxy server** - intermediary server for requests from client. Masks true origin of the request to the resource server

**Reverse proxy server** 

[https://en.wikipedia.org/wiki/Proxy_server#Reverse_proxies](https://en.wikipedia.org/wiki/Proxy_server#Reverse_proxies)

- appears as ordinary server to client
- forward reqs to one or more ordinary servers to handle the request
- response from the proxy server is returned as if it came directly from the original server, leaving the client with no knowledge of the original server
- can be used for load balancing, caching, security, logging, compression, encryotion/SSL, etc.


### Good API Design
- Consistency
- Stability - not changing a lot + extensible from the start+ backwards compatible
- Responses correct - handling all use cases
- Not a lot of unnecessary http requests - eg use batch endpoint when needed
- Idempotent
- Concise
- Easy to use and understand