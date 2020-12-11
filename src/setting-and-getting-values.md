# Setting and Getting Values

Setting and getting values from a state is done by the Agent layer. This is the entry point to the c19 agent and where your app comes into play.
Your application uses the endpoints exposed by the Agent layer of the C19 agent to get and set values to the state.

Communicating with the Agent layer depends on the Agent layer chosen by you in the C19 configuration. If you've followed the Getting Started guide so far, 
then you have noticed we were using the `Default` agent layer which exposes two endpoints: GET and PUT for getting and setting values.

## Setting a Value While Using the Default Agent Layer
```shell
curl -XPUT localhost:3098/ -d '{"cat": {"value":"garfield", "ttl": 20000}}'
```

The `Default` agent layer implementation expects a PUT request and will pass the value as-is to the state layer implementation. The agent layer 
is agnostic to the content of the PUT request, but the `Default` state layer, which we are using for our examples, dictates that the body will be a JSON object with the following form:
```json
{
  "key": {
    "value": "any valid json value, including a json object",
    "ttl": "optional ttl value for this key",
    "ts": "optional timestamp for the key"
  }
}
```

## Getting a Value While Using the Default Agent Layer
```shell
$ curl localhost:3097/cat
{"value":"garfield","ts":1603548753122,"ttl":null}
```

The `Default` agent layer expects a GET request while the path is the `key` to retrieve from the state.
Since we are using the `Default` state layer implementation, the answer is a JSON body similar to the one used for setting a value.
The agent itself is agnostic to the body returned by the state layer.

# Different Agent Layers
The above is an example for using the `Default` agent layer. As mentioned, different agent layers will have 
different implementations. Please refer to [Appendix I] to get specific information of the different Agent layers. 

# Compatibility
Important to note that not all layers are compatible with one-another. One Agent layer might not be compatible with 
a certain State layer. Please refer to the documentation of the layers you choose to make sure everything can work together.

[Appendix I]: appendix-i.md
