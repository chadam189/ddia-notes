## Chapter 1

### Overview

1. Introduces terminology we'll see throughout the rest of the book
1. What are the "Big Three":
   1. Reliability
   1. Scalability
   1. Maintainability

### Terms/Definitions

* database = store data so they (or another application) can find it later
* cache = remember result of expensive operation, to speed up reads
* search indexes = allow users to filter data (e.g. search results by keyword)
* stream processing = send message to another process, to be handled asynchronously
* batch processing = periodically crunching a large amount of accumulated data
* Reliability
  - work CORRECTLY (correct results at desired performance level) despite ADVERSITY (errors caused by hardware/software/humans)
* Scalability
  - as system GROWS (more data, traffic, or complexity), there are reasonable ways to handle the growth to continue being RELIABLE
* Maintainability
  - future engineers can PRODUCTIVELY support the system, to maintain current behaviour and adapt it for new use cases

### Phrases I Should Use

* "access patterns" => db, cache, message queue are all data systems, technically...but they have very different access patterns (aka user requirements) which means different performance characteristics and therefore different implementation strategies
* "telemetry" => db, cache, message queue are all data systems, technically...but they have very different access patterns (aka user requirements) which means different performance characteristics and therefore different implementation strategies
* "degradation" => how many users can we add before we see our performance degrade?
* "fan-out" => servicing one incoming requests involves making many other requests (aka fan-out)
* "latency" => incoming request has been received, but it's in a queue waiting to be "seen" (aka no action is being taken)
* "response time" => from the USER'S persepctive, how long does it take to receive a response
* "median response time" => p50, or if I took every response time from the last 5 minutes and sorted them, what would the middle one be (aka 50% are faster than this, 50% are slower)?
* "p50, p99, p999" => same exercise as above: what is the response time for the fastest X percengate of requests? ("The top 99% of requests see a response time of 1.5 seconds")
* "tail latencies" => the latencies for the very highest percentiles of response times
* "Service Level Objectives" => individual goals/targets for a specific service (aka the median request needs to be under 200 milliseconds)
* "Service Level Agreements" => the complete list of SLO's that define the agreement between said service and all of its consumers
* "Tail Latency Amplification" => aka "weakest link in the chain" - if 5 parallel requests have to be made, the overall response time will be based on the slowest (not fastest, or average) response
* "Extensibility" => make it easy for future engineers to expand/modify this system to service new user requirements by focusing on extensible code and design choices
 
### Reliability

"Works CORRECTLY (correct results at desired performance level) despite ADVERSITY (errors caused by hardware/software/humans)"

Write code that handles errors!
Robust testing pyramid, including manual QA
APIs that are flexible and not too restrictive (therefore incentivizing devs to work around them), but also have built-in protections
security - preventing access by people who shouldn't have it, and limiting actions of people who do have it
various non-production environments to test in
automated processes for both deployments and rollbacks
METRICS! track performance metrics, error rates, logs all (relevant) requests and DB queries into an easily queryable storage search index (Elasticache)


### Scalability

"As a system GROWS (more data, traffic, or complexity), there are reasonable ways to handle the growth to continue being RELIABLE"

Need to be able to define "load"

Load parameter examples:
* requests per second (web server)
* concurrent users (application)
* read/write ratio (database)
* hit rate (cache) 

Two ways to analyze change:

L = load parameter
R = resources
P = performance

f(L, R) => P

1. f(2L, R) => ??P 

By doubling load (concurrent users, tweets in Tweets table, QPS) with SAME resources, what happens to performance?

1. f(2L, ??) => P

By doubling load AND wanting exact same performance, how many more resources (CPU, pods, diskspace) do we need to achieve that?


Total Response Time = netowrking time => latency (request is queued up but no action is being taken yet) => processing time => networking time

**Response Time = 2x Networking + Latency + Processing**

End-user request needs 5 parallel BE requests:
1. 77ms
2. 83ms
3. 129ms
4. 507ms
5. 99ms

The WHOLE end-user response is held up by slow-ass BE #4! 

Scaling up = vertical scaling (more powerful machines)
Scaling out = horizontal scaling (more copies of the same machine)

Best option is a combo of both
Scaling UP = better for stateful systems (DBs)
Scaling OUT = better for stateless systems (web servers)

* Two systems, each with 6GB/minute in requests:

1. 100K requests per second, each request is \~1KB in size (1KB * 1000 = 1MB, * 100 = 100MB, * 60 = 6000MB => 6GB)
1. 3 requests per minute, each request is \~2GB in size 

These are going to be designed WILDLY different from each other

### Maintainability

"Future engineers can PRODUCTIVELY support the system, to maintain current behaviour and adapt it for new use cases"

List of maintenance tasks:
* add new features to adapt to new use cases
* pay down tech debt
* investigate failures (and then fix the bugs that caused them)
* adapting it to new platforms / updating modules 
* keeping it operational (ex: migrating from Node v6 to Node v12)

1. Operability 

**"Make it easy to keep system running smoothly"**

Make life easier for devops!

Provide helpful documentation ("if I do X, then Y will happen")
Provide good default behaviours (with ability to insert overrides)
Provide robust error handling
Provide appropriate logging
Provide connections to monitoring so perf metrics can be tracked

1. Simplicity:

**"Make it easy for new engineers to understand system"**

1. Evolvability:

**"Make it easy for future engineers to change the system"**



