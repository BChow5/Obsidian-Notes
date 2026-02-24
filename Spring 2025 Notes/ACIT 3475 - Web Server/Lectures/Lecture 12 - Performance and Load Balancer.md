### Performance Considerations
- performance issues are usually caused by poorly optimized code in either front end, back end, or associated services
- another reason could be surrounding system config and resource bottlenecks
- besides optimizing the app, we need to understand the surrounding systems and measure and observe related performance metrics that could limit the number of users we can support

### Metrics
- performance metrics and quantifiable figure
	- allow you to measure and compare system performance
- metrics require a baseline for comparing expected performance against change in performance
- baseline should be collected when application is deployed and fully functional 

### latency
- latency is a measure of how fast a server responds to requests from the client
- measured in milliseconds (ms)
	- also called response time
- lower numbers are faster responses 
- measured on client side from the time the request is sent until the response is received 

### throughput and percentile
- how many requests the server can handle during a specific time interval
	- usually reported as requests per second
- percentiles are a way of grouping results by their percentage of the whole sample set

### identifying bottlenecks
- load testing to identify potential bottlenecks and how your app reacts under significant load
- load testing models and expected usage of a program by simulating multiple users accessing the program concurrently
- complex load testing includes test plans or scripts that send random queries to simulate logins, etc. 