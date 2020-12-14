# Non-Kubernetes Deployment

If you are not running a Kubernetes cluster or you want to test C19 locally without a Kubernetes cluster, you can easily do so by using the Static Peer Provider.

As you've learned in previous chapters, the Connection layer uses a Peer Provider to query for available peers to choose from. The peer provider we've been using so far was 
K8s since we deployed and used C19 inside a Kubernetes cluster. But that doesn't mean we can't use it outside of a Kuernetes cluster.

# The Static Peer Provider
The static peer provider allows us to manually and statically specify the peers that are available in our C19 group.

```yaml
  connection:
    kind: Default
    push_interval: 1000
    pull_interval: 60000
    port: 4097
    target_port: 4097
    r0: 3
    timeout: 1000
    peer_provider:
      kind: Static
      peers:
        - 192.168.1.2
        - 192.168.86.204
```

The connection configuration above uses the static peer provider with 2 available peers in the group. You can specify how many you want.
The port that will be used by the connection layer is determined as follows, by this order:
1. The static peer provider includes the port with a peer. For example: `192.168.1.2:5097`
2. `target_port` is specified
3. If non of the above exist then `port` will be used as the target port.

# Docker Compose
There are different ways you can run C19 on your local machine. For this demonstration and for the sake of simplicity, we will use docker-compose.

If you'd like to deploy without Docker then you'd have to pull the source code from the C19 Github 
repository and compile it using Rust. If you are familiar with Rust then you should find the procedure for compiling straight forward. Just 
use `cargo build --release` and you're done.

After you have `c19` binary ready, you can move forward with the following steps and make the proper adjustments when things are specifically relevant 
for Docker deployment.

# 1. Configuring for Local Deployment
So now that we know all about the static peer provider, let's configure it so we can then deploy a C19 cluster on our local machine.

We will use the following configuration for our C19 agents:

```yaml
version: 0.1
spec:
  agent:
    kind: Default
    port: 3097
  state:
    kind: Default
    ttl: null
    purge_interval: 10000
  connection:
    kind: Default
    push_interval: 1000
    pull_interval: 60000
    port: 4097
    r0: 3
    timeout: 1000
    peer_provider:
      kind: Static
      peers:
        - 192.168.0.2
        - 192.168.0.3
        - 192.168.0.4
```

Let's save this configuration as `config.yaml`

# 2. Configuring `docker-compose`
We will configure our docker-compose to use a network which will allow us to set a static IP address for each C19 agent.

```yaml
version: "3.8"
x-c19: &c19
  image: c19p/c19:0.1.0
  command: ["/usr/local/bin/c19", "--config", "/etc/c19/config.yaml"]
  environment:
    RUST_LOG: "c19=info"
  deploy:
    replicas: 1
  volumes:
    - "./config.yaml:/etc/c19/config.yaml:ro"

services:
  c191:
    <<: *c19
    networks:
      cluster:
        ipv4_address: 192.168.0.2
    ports:
      - "3097:3097"
      - "4097:4097"

  c192:
    <<: *c19
    networks:
      cluster:
        ipv4_address: 192.168.0.3
    ports:
      - "3098:3097"
      - "4098:4097"

  c193:
    <<: *c19
    networks:
      cluster:
        ipv4_address: 192.168.0.4
    ports:
      - "3099:3097"
      - "4099:4097"

networks:
  cluster:
    ipam:
      driver: default
      config:
        - subnet: "192.168.0.0/24"
```

Save this as `docker-compose.yaml`

As you can see, we configured the `cluster` network and used the same `c19` service configuration except for the static IP address 
and the port mapping.

# 3. Running the C19 Agents

```shell
docker-compose up
```

# 4. Inspecting Our Deployment
Let's inspect our deployment:

```shell
$ docker-compose ps
      Name                    Command               State                       Ports                     
----------------------------------------------------------------------------------------------------------
resources_c191_1   /usr/local/bin/c19 --config /etc/c19/co ...   Up      0.0.0.0:3097->3097/tcp, 0.0.0.0:4097->4097/tcp
resources_c192_1   /usr/local/bin/c19 --config /etc/c19/co ...   Up      0.0.0.0:3098->3097/tcp, 0.0.0.0:4098->4097/tcp
resources_c193_1   /usr/local/bin/c19 --config /etc/c19/co ...   Up      0.0.0.0:3099->3097/tcp, 0.0.0.0:4099->4097/tcp
```

We have three C19 agents as expected, exposing ports to our local host for our inspection.

You can now follow up with [Testing the Deployment] chapter to play around with your deployment.

[Testing the Deployment]: getting-started-test-deployment.md
