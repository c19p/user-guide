# Sharing State

You have a cluster of services and you want to share state across them.

![Sharing state use case][figure]


This is a classic case for C19. Services that share the same C19 group will automatically share the state between them, effectively having the state 
available locally for each service. i.e distributed in-pod cache.

Note that different services can share the same C19 group. You are not limited to a single service type.
For example, consider an API gateway service, Web backend and a third service type for user subscription information.

[figure]: figure-4-sharing-state.png "Sharing state use case"

