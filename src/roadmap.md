# Roadmap

Different factors may affect the roadmap of the project. While we do our best to follow the roadmap we set, there could be changes.

Make sure to revisit this page from time to time to see what we are up to.

## Implementing Different Layers
The power of the C19 protocol is with its layers. Different layer configurations and combinations determine its behavior and the use cases it serves.
The more layer implementations, the more use cases it will answer.

### A Few Ideas
`Git State` - A state layer that acts as a git repository. Combined with a matching connection layer, it will act as a distributed git repository. The State will act as a 
key/value store based on Git and will allow versioning and conflict resolution as Git does.

`The Query Paradigm` - With the current layer implementations, the governing paradigm is one that's having every C19 agent hold a copy of the state. 
The `Query` paradigm will allow an application to query its local C19 agent which will, in turn, send the query to other C19 agents until an answer is found.
This paradigm allows holding a very large data set and optimizing where the data is being propagated to based on demand. When an application queries its local C19 
agent for data and it does not currently hold that data, it will query other C19 agents and will cache the results.

`Redis-Backed State` - Having a State layer where the backend is Redis. The Agent will be used as a HTTP layer for passing commands to Redis. This can be implemented for 
different kinds of DBs.

## Adding Capabilities to the `Default` Agent
The `Default` agent exposes two endpoints for setting and getting values from the state. Adding APIs that will allow an application to register for state changes would 
allow it to be notified when a key has changed.

## Adding Capabilities to Layer Integrations
Enriching the internal APIs between the layers would allow extending the different kinds of layers that can be implemented.

## Metrics
Implementing metrics API for all layers that allows layers to expose their internal stats. 

Implementing different exporters for metrics like: Prometheus and Graphite.
