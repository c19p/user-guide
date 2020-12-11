# Configuring the Agent

A C19 agent has 3 parts (layers): agent, state and connection.

We will go into details about each layer in the [Architecture] chapter, but for now all you need to know is 
that the agent layer is responsible for communicating with your app, the connection layer is responsible for communicating with other 
C19 agents and the state layer is responsible for holding your data.

A C19 configuration file is a YAML formatted file. Let's prepare ours:

```yaml
version: 0.1
spec:
  agent:
    kind: Default
    port: 3097
  state:
    kind: Default
    ttl: null
    purge_interval: 60000
  connection:
    kind: Default
    port: 4097
    push_interval: 1000
    pull_interval: 60000
    r0: 3
    timeout: 1000
    peer_provider:
      kind: K8s
      selector:
        c19: getting-started
      namespace: default
```

As you can see, we have our 3 layer configurations: agent, state and connection. Every section starts with the `kind` configuration which 
tells C19 which kind of agent/state/connection to load. As you will learn, there can be different types of layer implementations, each would 
suit different needs.

## Agent
`kind` - We will use the "Default" agent layer. For now, it's enough for you to know that the "Default" agent provides a HTTP server which 
accepts a JSON formatted calls for getting and setting values to the state.

`port` - The port to use for the agent HTTP server. The default value, if unspecified, is 3097.

## State
`kind` - Same as the Agent, we will use the "Default" state implementation. The "Default" state implementation provides a key-value store 
with an option to set a TTL for each key.

`ttl` - The TTL to use (in milliseconds). If left out there will be no default TTL set for keys. In any case, as you will learn, you can always set a TTL for a 
key when setting a value to the state. Default value is no ttl.

`purge_interval` - The Default state will always filter out expired keys, but will still hold them in memory until purged. This setting controls the 
interval (in milliseconds) in which expired keys will be purged. Default value is 1 minute.

## Connection
`kind` - We will use the "Default" connection layer implementation. The "Default" connection uses HTTP to exchange the state with other C19 peers. 
It randomly chooses peers to connect to and exchanges the state at a specified interval.

`port` - The port to use when connecting to other peers and allowing other peers to connect to this C19 agent. Default port is 4097.

`push_interval` - Controls the interval (in milliseconds) in which the connection layer will connect and push the state to a random set of C19 peers. Default value is 1 second. 
Setting this value higher will speed up propagation of the state in the system, but would come at the expense of CPU usage.

`pull_interval` - Controls the interval (in milliseconds) in which the connection layer will connect and pull the state from a random set of C19 peers. Default value is 60 seconds.
The connection layer will specify its own state version when pulling the state from other peers. If the state versions match then no state will be exchanged. This is to optimize 
redundant state exchanges. If the versions do not match then the full state will be returned by the connected peer.

`r0`: Controls how many peers should this connection layer connect to at each publish interval. Setting this value higher will speed up propagation 
of the state in the system, but would come at the expense of network load. (connecting to more peers). Default value is 3.

`timeout` - Controls the connection timeout (in milliseconds) when exchanging state with each peer. Default value is 1 second.

`peer_provider` - At each `publish_interval`, the connection layer will query the peer provider for a full list of available peers to choose from. There are 
different types of peer providers. For example: Kubernetes peer provider that queries for pod endpoints based on a label selector and a static peer provider which allows specifying a predefined 
list of peer endpoints. Since we will be deploying to a Kubernetes cluster we will be using the K8s 
peer provider.

### K8s Peer Provider
`kind` - Controls the type of peer provider to use. Default value is `K8s`

`selector` - Sets the Kubernetes pod label selector which will form a C19 group. Default value is none which means all pods will be used for the same C19 group. 
This option allows you to have many different C19 groups. Each group will have its own isolated state shared.

`namespace` - The Kubernetes namespace to use. Defaults to the `default` namespace. Can also be set to `:all` (mind the `:`) to specify that all namespaces should be used.

## Applying the Configuration
You specify the configuration file to use when running a C19 agent by using the `--config <filename>` flag.

[Architecture]: architecture.md
