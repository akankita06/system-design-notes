### Resources
- [https://github.com/JMPerez/wpo-book](https://github.com/JMPerez/wpo-book)
- [https://www.sciencedirect.com/topics/computer-science/performance-optimization](https://www.sciencedirect.com/topics/computer-science/performance-optimization)

### Front end
![frontend](https://github.com/akankita06/system-design-notes/blob/main/images/webperformance.jpeg)

- Minimize http request - can all the data at once instead of sending a request for every data needed
- Minifying a file involves removing unnecessary formatting, whitespace, and code.
- Since every unnecessary piece of code adds to the size of your page, it’s important that you eliminate extra spaces, line breaks, and indentation. This ensures that your pages are as lean as possible.
- When it comes to your website, leaner is better. The fewer elements on a page, the fewer HTTP requests a browser will need to make the page render — and the faster it will load.
- Replace cookies with Local storage since cookies will have to be sent with every request

### Middle
- Precomputation
- Caching (with high hit rates)
- Minimize number of requests
- Asynchronous requests
- High speed protocols eg - Udp instead of TCP where it makes sense
- Avoid lot of 301 redirects
- Early flushing (allow users to see main content first generating in parallel the content of different side areas, that are served by flushing in the very moment they are ready.
- Pagination

### Service
- Multithreading when tasks can be done in parallel or for higher throughout of requests
- Asynchronous tasks for IO waits
- Non blocking algorithms for locks, etc
- Differential data transfers or updates

### Database
- Query optimization
- Optimized locks instead of persistent ones
- Different read write paths where needed
- Indexing
- Optimum database choice (for a lot of updates Cassandra, for read with lots of joins - nosql db)
- Snapshots/materialized views

### Others
- Profiling with **profilers**
- Code optimization - faster algorithms
- Configuration optimization - tuning the knobs of components you use eg kafka
- Caching strategy
- Load balancing
- Parallelizing work - distributed computing
