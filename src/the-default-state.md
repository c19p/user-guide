# The Default State

In this section we will walk through the `Default` implementation of the `State` layer. Please don't be mistaken by 
thinking `Default` means just a placeholder for something more powerful or that this implementation is not for production usage.

This `Default` implementation is powerful enough to answer most use cases. It is default just because we had to start with something...

# Behavior of the Default State
The `Default` state layer is a key/value store, where the key is expected to be a string and the value anything JSON compatible, including a JSON 
object.

The `Default` state layer supports setting up a TTL per key or a default TTL to be applied to all keys that don't have a TTL set when 
putting a value into the state.

Also supported is a data seeder. You will learn more about data seeders in the [Data Seeders] chapter. For now it is enough 
to know that a data seeder will load your data into the C19 agent as it first launches.

# State Configuration
```yaml
state:
  kind: Default
  ttl: null
  purge_interval: 60000
  data_seeder: 
    kind: File
    filename: data.json
```

`kind` - Here we mention we would like to use the `Default` state layer.

`ttl` - The TTL to use (in milliseconds). If left out then there will be no default TTL set for keys. In any case, you can always set a TTL for a 
key when setting a value to the state. Default value is no ttl.

`purge_interval` - The Default state will always filter out expired keys, but will still hold them in memory until purged. This setting controls the 
interval (in milliseconds) in which expired keys will be purged. Default value is 1 minute.

`data_seeder` - Optional and can be omitted. The Data Seeder to use when launching the C19 agent. In the above example we are using a data seeder that loads 
the data from a file named `data.json`

[Data Seeders]: data-seeders.md
