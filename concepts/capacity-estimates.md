## **Search (Twitter)**

- users = 1.5B, DAU = 800M
- tweets = 400M / day, size = 300Bytes (140 chars + metadata)
- searches = 500M / day

**Storage**

- Metadata
    - new tweets = 120 GB/day = 200TB for 5 yrs
    - fault tolerance copies + 80% full = 500TB
    - 1 server storage = 4TB, total servers = 125
- Index
    - keys
        - 300k (dictionary words) + 200k (famous nouns) = 500k
    - tweet IDs
        - index in memory for tweets of last 2 years
        - tweets in 2 yrs = ~300B
        - tweetIDs = 300B * 5bytes = 1460GB
        - word/tweet = 15 (excl. the, etc) - 1 tweet stored 15 times
    - storage = 1460GB * 15 = 21TB
    - 144GB memory/server -> 152 servers
- Individual fields:
    - name - 20 bytes
    - latitude-langitude (for GPS location 24 bits for lat/long. is fine), but use - 4 bytes each
    - dates - 4 bytes
    - Id (nearly million unique IDs) = 8 bytes (to account for growth)
    - image path - 256 bytes
- time estimations
    - for storage - for next 10 years
    - account for growth
- number of connections per server - 65,5356 (can be expanded by request per thread instead of connection per thread)
