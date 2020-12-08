### Video streaming (youtube, netflix)

- [code karle - netflix](https://www.youtube.com/watch?v=lYoSd2WCJTo&feature=emb_logo&ab_channel=codeKarle)
- [grokking the system design - youtube](https://www.educative.io/courses/grokking-the-system-design-interview/xV26VjZ7yMl)
- [How does netflix onboard new content](https://www.youtube.com/watch?v=x9Hrn0oNmJM) (gaurav sen)
- [Netflix system design](https://www.youtube.com/watch?v=psQzyFfsUGU) (tech dummies)
- [What happens when you press play](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html) (highscalability.com)
- [How netflix handles dat streams upto 8M events/sec](https://www.youtube.com/watch?v=WuRazsX-MBY) (talk)
- [How video files are uploaded on Youtube server](https://support.google.com/youtube/answer/3070500?hl=en) (google documentation)
- [youtube using http for resumeable uploads](https://developers.google.com/youtube/v3/guides/using_resumable_upload_protocol)
- [RTMP protocol](https://www.youtube.com/watch?v=AoRepm5ks80&ab_channel=Heavybit) (TCP based)
- [SFTP protocol](https://www.youtube.com/watch?v=OE34VJCPMVQ&ab_channel=ExaVault) (TCP based)
- HLS protocol (HTTPS based)
- MPEG-DASH (HTTP based)

&nbsp;

### Music streaming

- [Music streaming](https://www.youtube.com/watch?v=ks-CS41AiQs&ab_channel=ReachGoals)
- [Spotify - how ML finds new music](https://medium.com/s/story/spotifys-discover-weekly-how-machine-learning-finds-your-new-music-19a41ab76efe)

&nbsp;

### FB live / video conference / zoom

- [code karle - zoom](https://www.youtube.com/watch?v=G32ThJakeHk&ab_channel=codeKarle)
- [FB live architecture](https://www.youtube.com/watch?v=IO4teCbHvZw&t=1452s&ab_channel=InfoQ)

&nbsp;

### whatsapp / chat / audio call

- [code karle - whatsapp](https://www.youtube.com/watch?v=RjQjbJ2UJDg&feature=emb_logo&ab_channel=codeKarle)
- [grokking the sytsem design - fb messenger](https://www.educative.io/courses/grokking-the-system-design-interview/B8R22v0wqJo)
- how are connections handled on a server
- [end-to-end encryption](https://www.youtube.com/watch?v=jkV1KEJGKRA&ab_channel=Computerphile)

&nbsp;

### Streaming protocols
- RTMP, HLS, MPEG-DASH
- [Adaptive bitrate processing](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)
- [Streaming protocols](https://www.dacast.com/blog/video-streaming-protocol/)
- Real time messaging protocol (RTMP)
- web real time communication protocol (WebRTC)
- [ingestion protocol comparison](https://developers.google.com/youtube/v3/live/guides/ingestion-protocol-comparison)
- [streaming protocols](https://www.dacast.com/blog/streaming-protocols/)

&nbsp;

  **which to use depends on**
  - audio, video codes supported
  - segmentation by default
  - playback support
  - http vs tcp vs udp protocols (RTMP, HLS, MPEG_DASH - all are http protocols)
  
 &nbsp;
 
 **WebRTC**
 - real time communication protocol via simple APIs
- allowing direct peer-to-peer communication, eliminating the need to install plugins or download native apps
- [https://en.wikipedia.org/wiki/WebRTC](https://en.wikipedia.org/wiki/WebRTC)


&nbsp;

### Starting the video upload from the same point

- hash the video on client (Create its ID)
- send the video theough RTMPS (which chunks the video and uploads it)
- if connection is lost, you can still maintain videoID on the client till all chunks are uploaded.
- on server, check against (videoid, userid) = if available, then it is extended part of the existing video
- have a ttl on the cache to invaldiate it when the video is fully uploaded

&nbsp;

### Video Search

- Index built on video tags or title
- Rest is similar to any other search

&nbsp;

### Architecture for streaming data at netflix

- For additional requirements like exactly once instead of atleast once - build a wrapper on top of Kafka
- If kafka goes down, what to do ? - have a buffer on the producer / client side to store events temproarily. Once that buffer is filled, events start dropping. But with netflix - their priority is not to block producers. the yare okay with event loss.
- Each event type is a topic.
- number of acks you wait for - can also be blockers. Netflix does 1 ack (instead of 2) - :question:
- 10MB size limit on event (still pretty big)
- Immutable event payload
- Batch events to reduce CPU and network
- stick to 1 partition for a while when producing for non-keyed messages
