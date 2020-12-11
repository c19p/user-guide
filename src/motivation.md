# Motivation

Running microservices architecture has many benefits, but as you might already found out, it brings a lot of pain you must address. Services don't operate in an isolate world,
services depend on each other to complete an operation. You have to answer questions like: what if one service becomes unavailable or loaded? What should be the strategy for the
dependent service? Should it retry? Should it return an error or maybe fallback to a different result?

You then find yourself employing caching, auto scaling, retry logic, rate limiters and load balancing. Maybe you deploy Istio or Linkerd to deal with many of the issues.
While these are great solutions, they solve for the general case and don't care about the data. But the data across your system is not equal. Some data can be stale, some of it 
must be available in real time. Is it worth-while loading your system for data that doesn't change much often? In fact, if you look closely at your data across your system,
you might find out that some of it, if not most of it, can be consumed in a stale manner.

C19 doesn't come to replace the great solutions mentioned above, but it offers a different solution that would ease up the load on your system, would increase its availability,
and would do so in a simplified way. 

## Failure Rate
![Service SLA][figure-1]
##### Figure 1.0 - Service SLA

You may feel content that you have 99% service SLA. But looking at a typical topology like depicted in figure 1.0 above, you
can see that there are usually more services involved in a single client request. If you count down the SLA for each one of the services, 
you can quickly see that the total service SLA drops down to 97%.

If you could bring the data to be available locally for any of the services above, you can immediately bring up your SLA.


## Load
While most services can be horizontally scaled, some cannot. If you load the DB with a slow running query, you will immediately bring down the whole chain 
of services. If you handle your rate limits and circuit breakers well then you will end up with a degraded service at the best case scenario.

Even for other services that can be horizontally scaled, it doesn't come for free. You pay for resources.

## Single Point of Failure
When talking about data, there is usually a single source of truth. Probably a database. Even if you scale your API services up to support the 
load on your system, you still have to get the data from your source of truth - the DB.

## All data is NOT equal
Your data is not one thing. Your data differs in different ways across your system. Some of your data must be available in real-time while other data can 
be consumed while slightly stale. If this is the case, then why loading your system with redundant requests when the data can be available locally.

## One Solution to Rule Them All!
Of course all of the above can be solved in different ways: load balancers, auto scaling, circuit breakers, DB replication, caching, etc...
Each solution would require its own dedicated system.

C19 offers one, generic way, of solving many issues altogether. In the cases described above, a c19 group can share the state across the system while decoupled 
from the load of the system.

[figure-1]: figure-1-service-sla.png "Service SLA"
