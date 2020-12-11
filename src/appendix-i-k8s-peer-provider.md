# K8s Peer Provider

The K8s peer provider targets Kubernetes and will query for pod endpoints using a specified pod label selector.

## Configuration
```yaml
peer_provider:
  kind: K8s
  selector:
    c19: getting-started
  namespace: default
```

`selector` - The pod selector to use when querying Kubernetes API server for a list of pod's endpoints.

`namespace` - The namespace to use. Defaults to `default` namespace. Accepts `:all` (mind the `:`) to allow querying across all namespaces.

In the configuration example above, one would specify `c19: getting-started` on their pods label.

## Behavior
The K8s peer provider uses the specified selector and namespace to watch the matching pod list for changes. It holds a most up-to-date list of the available pods and 
will return that list whenever queried for by a connection layer.
