### Similar architectures
- uber
- lyft
- friends nearby
- tinder

### Resources
- [uber architecture - microservices](https://www.codekarle.com/system-design/Uber-system-design.html)
- [uber - gsdi](https://www.educative.io/courses/grokking-the-system-design-interview/YQVkjp548NM)
- [uber geospatial data platform](https://www.youtube.com/watch?v=Dc5WYYhMOIQ&t=292s&ab_channel=DataWorksSummit)
- [uber - how is location data stored and indexed in database](https://www.youtube.com/watch?v=AzptiVdUJXg&ab_channel=UberEngineering)
- [uber - map matching](https://www.youtube.com/watch?v=ChtumoDfZXI&ab_channel=UberEngineering)
- [uber - ETA calculation](https://www.youtube.com/watch?v=FEebOd-Pdwg&ab_channel=UberEngineering)

### Uber - gsdi
- how to deal with frequent location updates from drivers and update the quad (or segment tree)
- subscribe customers to the drivers (when they request a ride) to give them most up-to-date location of the nearby drivers

### Qs
- how are shortest routes stored between point-A and point-B? 

### Notes
- for fixed locations for objects considered. eg: restaurants for Yelp - used **fixed grids**
- for dynamic locations for objects that keep changing location: eg: people for nearby locations or drivers/taxis for uber - use **dynamic grid**

- how to identify grid id (fixed grids) for a location given latitude and longitude (a quad tree with max and min longitude and latitudes?)
- dynamic nodes - how to define which out of the 4 children would a location go to (track min and max lat and long, and the min, (min+max)/2 lat and long goes to 1st quad / child and so on., a better way - use geohash. With geohash, the nearby places would be based on prefix based search of the string. But there are edge cases where 2 places might be close but then they can fall into different buckets [geohashing](https://www.youtube.com/watch?v=zsSZYHZyDnA). Also, geohash on the same level, can denote different region sizes - dynamic grid approach already in-built.But that would be in any generic approach. So may be use the doubly linked list approach for this.)
- how to connect leaf nodes into doubly linked list (what order should be maintained here?)
    - pointers to each of 8 neighbors may be - not fixed grids so no.
- using geohashes
- push based coomunication to client where clients can subscribe - plausible dpeends on how many concurrent connections a server can hold and so how many connections a server would need. With poll you constantly have to ask for updates - which can be wasteful. Also if you have to keep starting and closing connections, that would be very resource intensive. Its better to just keep the connections open with the online/active users
- number of people online and moving - 1bn DAU -> 10% active at any tim - 10mn active -> 100 mn -> 1% moving -> 1 mn * 500 friends/user = 500 mn

1 server can hold 65k connections so for 100 mn online people you'd need - 1500 servers. Although there are ways where people have increased the server capacity for concurrent connections.

You can use hybrid - where number of friends for a person is really large - like 10's of 1000s so you don't overload the system during push. You can poll for them.

Better performance
- only send updates real-time to online/active users
- Others for push notifiation when they are offline can be delayed
- Frequency can be altered for getting and pushing updates. Eg: Only send updates after every 5th update when someone is moving or when they are 0.1 mile apart from prev stop.
- also, when people get online, they can poll the data from friends nearby and for subsequent updates from the friends it would be a push update.


### Requirements
**Functional**

2 types of users - Drivers + Customers
- Drivers notify customers of their availability to pick passengers
- Passengers see all nearby available drivers
- Customer can request a ride -> nearby drivers get notified
- Once a driver accepts customer, they constantly see each other's current location
- Upon reaching destination, driver marks journey complete

**Non-functional**
- Availability (consistency can take a hit ?)
- Minimum latency (Real-time location updates)

**Capacity**
- 300M customers , 1M driver , 1M DAU , 500k DA drivers
- 1M daily ridespointer takes 8 bytes

### Algorithm
- Similar to Yelp

- Difference - **dynamic updates on quad tree for driver**'s and customer's current location

- Active drivers report their location every 3 sec - need Qaudtree udpate to reflect that - resource intensive
    - find the right grid based on drivers previous location
    - if new position not in current grid, remove driver from current grid and move/insert to a new grid
    - repartition new grid if max limit of drivers is reached (<u>grid - based on drivers number</u>)
- How to quickly propogate updates to Quadtree ?
- How to notify customes and drivers about location updates when ride is in progress ?

Solution

- Donot modify Quadtree every time driver reports new location
    - beats the purpose - Quadtree was to efficiently find nearby drivers
- Store the <u>latest postiion reported by all drivers in a distributed hash table</u> (DriverLocationHT)
    - Store current and old position of driver in hash table with driver ID
    - DriverID , old latitude , old longitude , new latitude , new longitude
    - Memory needed - 3 bytes for DriverID , 8 byte per lat./long. for 1M drivers = 35 bytes * 1M = 35MB
    - bandwidth - driver ID + new location
- Distribute hash table across multiple servers ?
    - not for memory constraints
    - for replication + future scalability - yes
- Notification system
    - DriverLocationHT servers store location data + do following
    - When server receives udpate for driver's location
        - it will broadcast it to - nearby customers
        - notify quadtree server to refresh driver's location - every 10 sec or so
- Notification system
    - Push or long polling model
    - Separate notification service - based on pub/sub model
    - Flow
        - Customer opens Uber app
        - query server to find nearby drivers (Cache it - if it is keeps updating ? )
        - On server side, before returning list of drivers , customer is subscribed to all updates from those drivers
        - maintain a list of subscribers for each driver
    - Memory to store subscriptions
        - 1driver - 5 subscriptions (3 byte driver ID)
        - 1m DA customers, 500k DA drivers (8 byte customer ID)
        - = (500K * 3) + (500K * 5 * 8) = 21MB
- How will drivers get added for a current customer
    - Customers pull current list when they open the app
    - For new drivers - track area customer is watching - complicated
    - Allow lients to pull info about nearby drivers from server
        - Clients send their current location (at a freq.)
        - Server finds nearby drivers from quadTree
        - Sendsdata to client
        - Client updates screen
        - Repartition a grid when it reaches limit ?
            - keep some buffer to not repartition very often
- Request Ride - flow
    - Customer will pull a request for a ride
    - Aggregate servers (AS) will take request + gets nearby drivers from Quadtree
    - AS collect + ranks results
    - AS will send notification to top drivers - whichever accepts first gets assigned the ride
        - other drivers get cancellationr request
    - once driver accepts request, customer gets notified
    
### Others

**Fault Tolerance**
- Replicas - notification servers, DriverLocation servers - if one dies, other takes over
- Also store data in persistent storage like SSDs that can provide fast IOs (to recover if primary/secondary if both fail)

**Ranking**
- Store popularity number in quad tree - get top 10 drivers from the quad tree ranked by popularity
