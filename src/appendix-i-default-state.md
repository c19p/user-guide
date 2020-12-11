# The Default State
The `Default` state layer implementation offers a key/value store where the key is a string and the value is anything JSON, including a JSON object.

When setting values to the store, the state expects a JSON object with a `value` and other optional fields (see below). It will return a JSON object with 
the value and all optional fields when retrieving values from the store.

This state also supports [Data Seeders].

## Configuration
```yaml
state:
  kind: Default
  ttl: null
  purge_interval: 60000
  data_seeder: 
    kind: File
    filename: data.json
```

`ttl` - The default TTL (in milliseconds) to use for new keys that are not explicitly set with their own TTL. Default value is `null` (no default ttl).

`purge_interval` - The Default state will always filter out expired keys, but will still hold them in memory until purged. This setting controls the 
interval (in milliseconds) in which expired keys will be purged. Default value is 1 minute.

`data_seeder` - The `Data Seeder` to use when the state is first loaded. Default value is `null` (no default data seeder).


## Behavior
The state holds a map of key to value where the value is anything JSON.
It offers an optional TTL per key and will expire those keys at a certain interval. 

### Setting Values
The `State` expects a JSON object in the following form:
```json
{
  "key": {
    "value": <any valid json value>,
    "ttl": <optional. ttl in milliseconds>,
    "ts": <optional. manually setting the timestamp (in milliseconds since epoch) of this key
  },
  ...
}
```

### Getting Values
The state will return a JSON as mentioned above.

### TTL and Purging Keys
The state will not return expired keys and will purge them every `purge_interval` milliseconds.

### Data Seeders
The state supports data seeders and will load a data seeder as the first step of initialization.

[Data Seeders]: data-seeders.md
