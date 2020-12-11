# The C19 Protocol

*by Chen Fisher*

The C19 protocol is a variant of the [Gossip protocol](https://en.wikipedia.org/wiki/Gossip_protocol). It allows a group of services to agree on a service-wide state.

The state is shared across a distributed set of services which in effect means that each service has the data available locally.

![Sharing state use case][figure]

C19 decouples the process of fetching the data from using it. Consider a service that depends on another service to get subscription data for a user. 
The dependent service would have to handle fetching the data before being able to use it. But fetching the data is not its main focus and should not be it's main concern.
By decoupling fetching the data from using it, c19 makes sure the data is available locally to the service. 

If you are running a microservices architecture you will soon, if not already, find yourself having to deal with dependencies across services, handling unavailability, scale, 
redundancy, etc. Microservices architecture brings with it a lot of complexities that are not at the core of your service. Solutions like Istio exist to help and solve this problem,
but nothing really beats having the data available locally.

C19 is a simple, powerful and extensible system. It's low on resources and is easy to reason about.

Although c19 can be run anywhere and has no dependencies, this user-guide targets Kubernetes as the main platform to run the c19 system. One of the goals of this project is to 
allow simplicity and have it "just work" and Kubernetes as an orchestrator supports that goal.

[figure]: figure-4-sharing-state.png "Sharing state use case"
