# Testing the Deployment

Now that we have our C19 powered Nginx deployment, let's test it.

We'll use Kubernetes port-forwarding to have two of the pods available to us locally. We will then use `curl` to get and set values to the C19 agent.

# 1. Port-Forwarding
```shell
$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-6bc49755fb-6h4l8   2/2     Running   0          32m
nginx-deployment-6bc49755fb-v4nx9   2/2     Running   0          32m
nginx-deployment-6bc49755fb-vw7q9   2/2     Running   0          32m
```

```shell
kubectl port-forward nginx-deployment-6bc49755fb-6h4l8 3097:3097
```

```shell
kubectl port-forward nginx-deployment-6bc49755fb-v4nx9 3098:3097
```

As you can see, we port-forwarded local port 3097 to 3097 of one pod and 3098 to 3097 of a second pod.

We will now use `curl` to set values to one of the C19 agents and see that the data is available on the second C19 agent as well.

# 2. Setting and Getting Values
```shell
curl -XPUT localhost:3097/ -d '{"cat": {"value":"garfield"}}'
```

We just put the key/value pair `cat` and `garfield` to the first C19 agent.

Let's get the value from this pod first:
```shell
$ curl localhost:3097/cat
{"value":"garfield","ts":1603548753122,"ttl":null}
```

You can see that the value returned to us is a JSON that includes the value, the timestamp and the ttl, which is set to `null` in this example.

Now let's get the value from the second C19 pod:
```shell
$ curl localhost:3098/cat
{"value":"garfield","ts":1603548753122,"ttl":null}
```

We used `3098` this time to make the call to the second C19 agent. We got the same value! True magic ;)

# 3. Setting a TTL
Let's set a ttl for the `cat` key. It doesn't matter if we use the first or the second C19 agent for that, as the data will propagate between them.
For the sake of this example, we will use the second C19 agent:
```shell
curl -XPUT localhost:3098/ -d '{"cat": {"value":"garfield", "ttl": 20000}}'
```

We just set a TTL of 20 seconds to the`cat` key. Let's quickly get it from the first pod before it goes away:
```shell
$ curl localhost:3097/cat
{"value":"garfield","ts":1603549250457,"ttl":20000}
```

Ok, we got it back. Now if we wait long enough and try to get the value again:
```shell
$ curl localhost:3097/cat
not found
```

We got `not found` (404) as a response. The value is long-gone...

# 4. Getting the Whole State
The `Default` connection layer implementation exposes port 4097 (by default) as the port for exchanging data between peers.
You can use that port to query the whole state! Let's see how we can do this:

```shell
$ kubectl port-forward nginx-deployment-6bc49755fb-vw7q9 4097:4097

$ curl -XPUT localhost:3097/ -d '{"cat": {"value":"garfield"}}'
$ curl -XPUT localhost:3097/ -d '{"dog": {"value":"Snoopy"}}'

$ curl -s localhost:4097 | jq
{
  "dog": {
    "value": "Snoopy",
    "ts": 1603549623758,
    "ttl": null
  },
  "cat": {
    "value": "garfield",
    "ts": 1603549562977,
    "ttl": null
  }
}
```

Amazing! 

(We used `jq` for pretty printing the JSON value)

# 5. Keep playing
I hope you got the gist of it. Feel free to keep playing and experimenting with the C19 cluster.
