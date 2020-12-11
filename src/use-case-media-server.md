# Media Service / Pulling from Origin

You have a cluster of media servers where users are live streaming to the cluster while other users consume the streams.
When a user publishes a stream their stream will be published to one of the media server instances. When another user wishes to consume 
that stream they might end up connecting to a different media server instance.

![Media server use case][figure-3]

The media server, serving the consumer, must pull the stream from the origin media server. How does it know where the stream origin is?

When the publisher first connects to the origin media server, the media server will set a key-value to it's local C19 agent. The key would be the identifier of the stream and
the value would be the IP address of the origin media server. Since all the media servers are part of the same C19 group, the data will be shared across all instances.

When the consumer connects to a random media server, that media server can query its local C19 agent for the IP address of the origin and pull the stream from it. Simple as that!

[figure-3]: figure-3-use-case-media-server.png "Media server use case"

