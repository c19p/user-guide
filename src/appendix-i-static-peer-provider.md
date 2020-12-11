# Static Peer Provider

The static peer provider allows to manually specify a list of available C19 peers.

## Configuration
```yaml
peer_provider:
  kind: Static
  peers:
    - 192.168.1.1
    - 192.168.1.2:3000
```

`peers` - A list of available peers in the form of IP or IP:Port.

If no port is specified, it is up to the connection layer to determine what port to use when connecting to peers.
The `Default` connection layer, as an example, will use its `target_port` or `port` configuration values.

## Behavior
The static peer provider will simply return the fixed peer list specified in the configuration.
