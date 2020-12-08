- Twitter resource - code karle
- [Snapchat stories](https://www.youtube.com/watch?v=WUleQzu9l_8&list=WL&index=2&t=759s&ab_channel=AmazonWebServices)

&nbsp;

# Instagram, FB

## **Reqs**

- image uploads/views/downloads
- search images
- follow - users follow other users
- newsfeed generation
- data reliability - no data loss
- availability (consistency can take a hit, favor eventual consistency over strict)
- low latency for uploads, reads and newsfeed

&nbsp;

## **Storage**

- cassandra for relationships - user-follows, user-photos
- dynamodb (kv store) - for user and image metadata
- Images - on a file system - HDFS or S3

&nbsp;

## **Design**

**Image uploads**

- Why split image uploads/writes and reads to separate services:
    - uploads are done on disk - so much slower
    - reads can be done from cache - much faster
    - web servers have connection limits, and we don't want all those connections to be used up by write requests

&nbsp;

## **Scalability**

- Redundancy/replication

**Sharding**

- based on user id:
    - hot user with lots of photos - issue - overloads server
- based on photo id:
    - new photoID = epoch time + auto incrementing key
&nbsp;

how to generate unique photoIDs (global key generation)how to avoid key DB to be a single point of failure

&nbsp;

## **News feed generation**

- feed generation + feed publishing

**feed generation**

- from database tables - periodic queries -> generate feed and store it in cache as LinkedhashMap with last generated date
    - next time, start getting data from last generated date
- new posts -
- keep newsfeed on cache for users who are frequently online (no need for it for everyone) - depends on login pattern
- keep last 3 days worth of tweets in cache (80-20 principle, newsfeed from latest data)
- if not sufficient to geenrate newsfeed then get more data from the database
&nbsp;

**feed publishing**

- when client comes online, serve pre-generated feeds
- for new posts, **push** - to followers if less followers
- if millions of followers then **pull** instead or **push to notify and pull to serve** -> push new posts only to online followers and for offline either drop it since they will get it through pre-generated feeds anyway. Otherwise send them a push notification, but you can handle some more time delay here which can be taken care of through notification service
- on mobile - use pull to save on bandwidth for users
&nbsp;

**sharding**

- creation time in epoch + tweetid -> since primary key itself has creation time in time, no need for additional col (and index on it) for timestamp = so faster writes (no additional index)
- since primary key already has timestamp component, already indexed based on timestamp - quick retrieval.
