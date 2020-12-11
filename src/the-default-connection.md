# The Default Connection

The `Default` connection layer implementation picks up random set of peers on each specified interval and uses HTTP calls to exchange its own state with 
the peer's state.

There are different subtleties that affect its behavior and can be configured. Let's explore them through the available configuration options.

## Configuration and Behavior
```yaml
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

`port` - The port to bind to and accept connections from other peers.

`push_interval` - The rate (in milliseconds) in which the connection layer will connect and push its state to other peers. To optimize state exchanges, the connection layer will only send the changes since 
last publish time. See the `pull_interval` for when the full state will be exchanged with other peers. The lower this number is, the faster the reaction 
to new state changes, but at the expense of CPU time. Depending on the size of your data, preparing the state changes to be published might be time consuming.

`pull_interval` - The rate (in milliseconds) in which the connection layer will connect and pull the state of other peers. It will specify the version of its own state and only if the versions do 
no match then the other peer will respond with its full state.

`r0` - The number of peers to connect to on each `publish_interval`. The peers are being selected randomly. The bigger this number is, the faster the data will propagate across the system, but at the expense of time 
it takes for each exchange. The implementation tries to parallelize the connections to other peers, but it still has its limits. If the rate of changes to the state is high and you 
choose a big `r0`, it comes at the expense of publishing new values to the system.

`timeout` - The connection timeout (in milliseconds). If this connection layer cannot connect to other peer within this timeout then the connection will be aborted.

`peer_provider` - The connection uses a peer provider to get the full list of available peers to connect to. It will then choose a random set of peers based on the `r0` configuration value. The peer provider 
can be anything on the available peer providers list. Please refer to the [Appendix I / Available Peer Providers] to browse through. In the configuration example above, we are using the `K8s` peer provider. This 
peer provider targets Kubernetes and allows you to choose the label selector of the pods that are part of your C19 group.

## Exchanging the State
The `Default` connection layer selects a random set of peers to connect to and exchange the state with.

It runs two threads: push and pull.
When pushing, only the changes from last publish time are being sent to other peers.

When pulling, the connection layer will specify its own state version and only if it does not match the peer's state version, will the full state be returned by the peer.

The state version is calculated from the keys and timestamps for each keys.

[Appendix I / Available Peer Providers]: appendix-i-peer-providers.md
