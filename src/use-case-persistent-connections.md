# Persistent Connections

You have a cluster of services that accept WebSocket connections from users (Mobile apps, Web, etc...).
The cluster of services is autoscaled and you need to route messages between two connected users.

Users connect to your app to form a "room" to chat it. Since the service cluster is behind a load balancer, each user can 
be connected to a different service instance.

