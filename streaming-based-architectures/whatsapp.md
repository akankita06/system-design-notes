### resources

- code karle
- whatsapp encryption

&nbsp;

![whatsapp 1](https://github.com/akankita06/system-design-notes/blob/main/images/streaming1.png)
![whatsapp 2](https://github.com/akankita06/system-design-notes/blob/main/images/streaming2.png)

### Data storage

- HBase or cassandra - columnar
- why / reqs?
    - support a very high rate of small changes (huge number of small messages need to be inserted)
    - fetch small range of records quickly (a user is mostly interested in sequentially accessing the messages)

&nbsp;

**mysql or other nosql DBs**

- have to read and write each row/record to DB every time a user received or writes a message
- hence, high latency

&nbsp;

### **benefits of HBase**

- groups data together to store in memory buffer **(fault tolerance?)**
- once buffer is full, it sumps them to the disk
- writes - helps writing small messages quickly
- reads - quickly fetch data by scanning range of rows or by key
- also store variable sized data

&nbsp;

**how will clients efficiently fetch data from the server**

- paginate the data
- page size depends on viewport of client

&nbsp;

**store connections**

- kv store
    - key: user id, value: connection object
- load balancer in front
    - for config - which server has connection object for which user id
- connection protocols:
    - web socket or long polling

&nbsp;

**manage status**

- cannot broadcast every status change to every relevant person
- solution:
    - when **client opens app**, pull status of all friends
    - when **user sends msg** to user who went offline
        - apply retry mechanism (if connection was temporarily broken)
        - else, send failure msg to user and update status
    - when **user comes online**, broadcast msg after a few moments to ensure user does not go offline immediately
    - **pull status of user's on viewport** (not too frequently)
    - when **client starts chat** with another user

&nbsp;

**partitioning**

- based on user id
    - will be quick to fetch user's entire message history together
    - store multiple partitions on a server and then as they grow, get them onto separate servers
    - uneven distribution - will require repartitioning
- based on message id
    - will take a long time - we will need to query every server to get the message
    - DONOT USE this

&nbsp;

**cache**

- for user who messages a lot and whose history needs to be fetched a lot
- 80-20 rule

&nbsp;

**replication**

- store multiple copies of user data
- can use compression, etc to store the data

&nbsp;

**fault tolerance**

- when a chat server fails, it is extremely hard to tranfer the TCP connections to another server, so instead let the clients reconnect

&nbsp;

**push notifications**

- every manufacturer or device has push servers
- we need to setup a notificaiton server - it will take messages for offline users and send them to push servers of the manufacturers
- consideration - we have more time to send these compared to the online one

&nbsp;

**group chat**

- maintain group chat id with (member id, message id)
- seq. - when message reaches server
- same seq. for everyone

&nbsp;

**clarifying Qs:**

- message retention?
- upper limit of users in a group chat
- are all users in a group chat considered identical?

&nbsp;

**considerations for group chat**

- If it's the same participant group, we don't create a new thread. So given a group of users, we need to find the thread. => get_thread([user_ids])
    - to do this - hash the participant ids
- data model:

- `messages (message_id, thread_id, user_id, message)`
- `threads (thread_id, user_id(creator), created_at, last_modified_at, flags, metadata_columns)`
- `user_threads (thread_id, user_id(participant), last_message_at, flags)`


creator user id for threads/groups - so that the thread does not become an orphan if everyone is removed

- it is enough to have a thread id in message table itself, since a message cannot be a part of multiple threads.
- **if it is 1-many relationship - let it be in the same table**
- `threads` table just maintains metadata for the group
- `user-threads` is required for the chat sequence, recency of messages for inbox, etc.
- indexes required

- `users: - key(id), other identifiers (like email)`
- `threads: - key(id)`
- `messages: - key(id), - key(thread_id), - key(thread_id, created_at) to load thread`
- `user_threads: - key(id) - key(thread_id, user_id): to load all the participants of a thread, and update the thread flagsc - key(user_id, last_message_at): to load the index in order`
- `thread_attachments: - key(thread_id): to find all attachments of a thread - key(attachment_type, attachment_id): to find thread by attachments`
- `thread_participant_hashes: - participant_hash: to find thread by participants`

&nbsp;

**message sync**

- push the updates to clients when they come online
- Options A - pull mode

    With the schema the candidate has, they can save a last_updated_at on the client side, when the client come back online, client should request the diff with last_updated_at. To "render" the diff, server side can get the threads using the user_threads table with user_id, updated_at index. However to get the messages with proper pagination can be expensive, the candidate should consider add a thread_id, timestamp index on messages table.

- Options B - push mode

    Similar to RSS reader, this can be done with a "push" mode as well. With table user_messages, all the messages for their receiver will have a row in this table, and the diff is simply the rows after last_updated_at or max_message_id on the client side.

&nbsp;

**Tradeoff**

- Option A
    - Pros: fast write
    - Cons: More computationally intensive during read
- Option B
    - Pros: more granular control on messages (hide the other user's message in a group chat temporarily for privacy, ghosting reasons), lower read latency
    - Cons: more writes

To achieve realtime push, server side should have a mapping of client (device) - last_updated_at for each device. When there is an update, server should push the diff and update the mapping when client acknowledges it. To push the diff, we can use WebSocket or other push technology ([https://en.wikipedia.org/wiki/Push_technology](https://en.wikipedia.org/wiki/Push_technology)). It's probably not a good sign if they don't know any.

&nbsp;

**sharding**

- users by user_id
- threads by thread_id
- messages by thread_id
- user_threads is tricky, it's unidirectional, shard by user_id then it's harder to fetch all participants for a thread, shard by thread_id then it's harder to get inbox for a user. A possible solution is shard the table by user_id, and duplicate the mapping on participant_hashes table with a JSON field something like that.
- thread_attachments by thread_id
- participant_hashes by thread_id

&nbsp;

**caching for groups**

system read/write should be better balanced (compare to Feed/I18n etc), so caching on thread level might not be helpful, especially with client side storage. Caching user_threads for inbox rendering or diff rendering for active users might be a good idea. 

&nbsp;

**encryptions**

[https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/](https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/)
