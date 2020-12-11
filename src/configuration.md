# Configuration

The C19 agent is configured using a YAML file. When running the `c19` binary you specify the configuration 
file using the `--config` flag.

The configuration file is divided into 3 main parts, one for every layer: Agent, State and Connection.
Since the configuration is specific to the layer chosen, your configuration will vary depending on the layers 
you choose.

For example, the following configuration uses the `Default` Agent, State and Connection layers.
For the Connection layer the `k8s` peer provider is chosen and for the `Default` state the file 
data seeder is being used.

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
    data_seeder: 
      kind: File
      filename: data.json
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

Please refer to [Appendix I] for specific details on each available layer.

[Appendix I]: appendix-i.md
