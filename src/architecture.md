# Architecture

If you've followed the `Getting Started` guide and read through [Configuring the Agent] chapter, then you had a chance to get a glimpse into the C19 agent architecture.
The main components of a C19 agents are the three layers: Agent, Connection and State. Each layer has different responsibilities within the C19 agent.

`The State` is where your data is being held. It allows the other layers to set and get values to and from the state.

`The Agent` is the entry-point to your application and where you communicate with the C19 agent. It exposes ways for your application 
to communicate with the C19 agent, set and get values to and from the state.

`The Connection` is the low-level layer that is responsible for communicating with other C19 agents and exchange the state with them.


Due to the nature of C19 extensibility, there are different implementations for each layer. You are free to choose whichever one suites your needs. Depending on 
your use case you may want to choose a `State` that acts as a key/value store or maybe one that holds blobs of data.

Your `Agent` might be one that accepts HTTP JSON requests for setting and getting values or it might be one that communicates over WebSockets.

The `Connection` layer you choose affects how your data is being exchanged with other C19 agents. You may choose one that is optimized for fast state propagation 
at the expense of network and CPU resources or maybe one that is slower to propagate data but is low on resources.


In this chapter we will talk about each layer in a bit more detail and use the `Default` layer implementations as an example.

[Configuring the Agent]: getting-started-configuration.md
