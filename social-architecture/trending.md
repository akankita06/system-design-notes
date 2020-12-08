### Resources
- [count min sketch](https://www.youtube.com/watch?v=ibxXO-b14j4&ab_channel=TechDummies)
- [top k system design](https://www.youtube.com/watch?v=kx-XDoPjoHw&ab_channel=SystemDesignInterview)
- [building a new twitter trends experience](https://blog.twitter.com/engineering/en_us/a/2015/building-a-new-trends-experience.html)

&nbsp;

![trending](https://github.com/akankita06/system-design-notes/blob/main/images/trending.png)
&nbsp;

**for geo based or feature based trends**

- index on that feature in database
- update cahche key from tweet hashtag to - eg:
    - geo : {tweet hashtag: count}
- for stream processing, same - you can have geo based processing where counter is not just for individual hashtag but also for geo

&nbsp;

**for last 24 hours**

- even that is just 1440 1 min windows, which can be easily stored in cache
- so you can still ETL events (raw events into a database for backup), but you can just combine the 1-min windows from cache after stream processor as well
