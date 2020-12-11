# Peer Providers

When a connection layer needs to select peers to connect to it uses a `Peer Provider` to get the full list of available peers to choose from.
This is of course implementation dependent and you should first confirm that the connection layer you choose is working with peer providers.

The `Default` connection layer is using a peer provider to get the full list of available peers. As with other components in the C19 agent, 
there are different peer providers to choose from.

Please refer to [Appendix I / Available Peer Providers] for a full list of peer providers to choose from.

Let's look at two peer providers as an example:

## 1. The K8s Peer Provider
```yaml
peer_provider:
  kind: K8s
  selector:
    c19: getting-started
  namespace: default
```

The K8s peer provider targets `Kubernetes` and will query for pod endpoints using a label selector. You can configure the K8s peer provider to query 
across all namespaces or a single one.

## 2. The Static Peer Provider
```yaml
peer_provider:
  kind: Static
  peers:
    - 192.168.1.2
    - 192.168.86.204
```

The static peer provider allows setting a fixed, predefined list of peers. You can configure it to include as many peers as you wish and can also 
include a different port for each peer. If no port is specified it will fall back to the `target_port` of the connection layer or the `port` configuration of 
the connection layer.

[Appendix I / Available Peer Providers]: appendix-i-peer-providers.md
