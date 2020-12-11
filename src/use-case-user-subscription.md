# User Subscription Data

You have a service that depends on another service for user subscription data. To make this interesting, let's say you have two service types 
that depend on a third service for the user subscription data: One that returns a list of available movies and another one that returns user information, 
including subscription data.

![User subscription use-case][figure-2]

Interesting to note here is that the movie service and the user profile service are two different services.
Each one of the services holds a C19 agent and they all share the same c19 group with the subscription service.

With this solution, instead of bombarding the subscription service with requests for data, hoping that the service will 
be available and respond in time, the c19 makes sure to bring the subscription data locally to each service.

Once the data is local, each service can request for data directly from the c19 agent instead of the subscription service.

[figure-2]: figure-2-use-case-user-subscription.png "User subscription use-case"
