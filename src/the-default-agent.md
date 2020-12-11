# The Default Agent

The `Default` agent layer allows an application to get and set values using HTTP requests.
It does not assume anything about the `State` being used, hence the body of the request can be anything.

## Setting Values
The agent exposes a `PUT` request endpoint which accepts a body of any type that will be passed to the state.
As mentioned, the `Default` agent does not assume anything about the `State` being used. This means that the body of the 
message can represent any type. When using the `Defaul` state layer, the body of the message is expected to be a JSON object 
representing the value being set:

```shell
curl -XPUT localhost:3097/ -d '{"cat": {"value":"garfield"}}'
```


## Getting Values
The agent exposes a `GET` request endpoint where the path represents the `key` to be retrieved from the state. The response is dependent on the `State` 
being used. In the following example we are using the `Default` state layer which outputs a JSON object with the value 
and extra details:

```shell
$ curl localhost:3097/cat
{"value":"garfield","ts":1603548753122,"ttl":null}
```

## Configuration
```yaml
agent:
  kind: Default
  port: 3097
```

The only relevant configuration option for the `Default` agent layer is the port to bind to when accepting connections from an application.
The default port value is `3097`.
