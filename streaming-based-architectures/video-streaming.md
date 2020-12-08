- **RTMP** - real time messaging protocol
- **ABS** - adaptive bitrate streaming

&nbsp;

### Resources

- [How netflix handles dat streams upto 8M events/sec](https://www.youtube.com/watch?v=WuRazsX-MBY)
- [code karle -zoom](https://www.codekarle.com/system-design/Zoom-system-design.html)
- [How does netflix onboard new content](https://www.youtube.com/watch?v=x9Hrn0oNmJM)
- [Netflix system design](https://www.youtube.com/watch?v=psQzyFfsUGU)
- [Adaptive bitrate processing](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)
- [How video files are uploaded on Youtube server](https://support.google.com/youtube/answer/3070500?hl=en)
- [What happens when you press play](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html)
- [Facebook live](https://www.youtube.com/watch?v=IO4teCbHvZw)

&nbsp;

### Questions

- Store play/pause for a user
- How is video played on client
- How is video transferred to client
- adaptive bitrate processing
- video upload -> simpel upload - **FTP**, live upload - **RTMPS**

&nbsp;

## **Video upload**
- To upload videos over network to server, protocol used - **FTP**

&nbsp;

**FTP** (or SFTP - secure file transfer protocol)
- file transfer protocol
- to transfer files b/w computers and servers over a tcp/ip n/w
&nbsp;

video transferred through browser which supports FTP 

&nbsp;

## **Video Encoding**

- **Codec** - how to compress data

&nbsp;

**Video formats**

- Different formats for diff. internet connection speeds
- High quality format - very detailed
- Low quality format - more lossy compression
&nbsp;

**Resolution** requirements - mobile has low resolution req., TV has high resolution req.
&nbsp;

**Adaptive bitrate processing**

- it detects user's bandwidth and CPU capacity in real time and adjusts the quality of media stream accordingly.
- requires and encoder to enconde a single media at multiple bit rates
- client switches b/w encodings depending on available resources
- result - very little buffering, a good experience for both high-end and low-end connections
&nbsp;

**Processing**

- Create tuples of (format, resolution). F*R = number of processings you would do on a video.
- Video - takes time to process video + can fail at any point
    - So avoid restarting from beginning
    - break video into chunks
    - run each chunk through different processes to convert them into different formats
    - task = (1 format, 1 resolution, 1 chunk)
    - Parallelize processing chunks across multiple servers on the cluster
&nbsp;

**For better user experience** **(server side)**

- chunk by scene rather than timestamp
- collate smaller chunks (shots) together to form a scene
&nbsp;

**For better user experience** **(client side)**

- if user views movies not contintously, but at random points:
    - called sparse movie
    - send only required chunks than prefetching a lot of data - save bandwidth
- for dense movies, watch movie continuously
    - predictively fetch future chunks for a good user experience
&nbsp;

**Better user experience by caching**

- Localized cached data (at CDN servers) close to the country - for max hit rate
- `cache server directly streams data to client (not via web server that got the request)` 

&nbsp;

## **Netflix architecture**

Netflix does 2 tier load balancing

- **Zones** - 1st load balances to zones - logical zoning. US could have 3 zones, India could have 1.
- tier 2 - LB for actual instances behind those zones
&nbsp;

For critical endpoints, reduce service dependency (Services on which it depends) - for minimizing risk of errors. 

&nbsp;

## **Video Streaming (CDN)**

When you ask for video streaming:

- DNS looksup IP address and maps it to Netflix / youtube / FB DNS
- That DNS (based on req params) routes to nearest CDN or datacenter (**Edge server**)
- When videos are sent to client, their blob paths are already embedded there
- So for get request on a video, that path is sent with the request and used by CDN to fetch the video directly
- Hence, no req. needs to be routed to the main server of Netflix at all

&nbsp;

### cache miss

- If video not in CDN directed to, edge server there sends req. to main data center
- main server responds with S3 path for video
- edge server can fetch video from that path + store it in its cache server

&nbsp;

## **DB**

- Storing videos - Blob storage (Amazon S3)
- Storing metadata - MySQL or noSQL DB
- Transactional data - payments, billing, etc in MySQL
- Youtube uses MySQL DB. Vitess - db clustering system to horizontally scale MySQL DBs. Vitess performs:
&nbsp;

- sharding capabilities
- handles failovers and backups
- improved db performance by implementing caching
- ***Good when you cannot let go of consistency but still need to scale.***

&nbsp;

### Storage efficiency

- For storage efficiency, Netflix runs a background job that segregates data into live viewing history and compressed viewing history
- Live viewing history - recent data - used for ETLs, etc.
- compressed viewing history
  - old data - not used as much
  - stored in compressed form in separate cassandra nodes by Netflix

&nbsp;

### Replication

Master slave configuration. Forgo consistency for availability. Eventual consistency model.
