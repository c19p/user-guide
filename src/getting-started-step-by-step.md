# Deploying to Kubernetes / Step-by-Step

We need three things to configure:
1. Add a label to the pod to be used as the selector for the C19 group
2. Add a C19 agent container to the pod
3. Add a configmap with our C19 configuration and refer to it from the container definition

## Labeling Our Pod
Note that in this example we use a single service type: nginx, but as mentioned before, this can be cross services.
Imagine you had another service: web. You would do the same for the web pod: add a container, use the same label and refer to the same configuration.

We will add the following label selector to our pod:
`c19: getting-started` this matches our configuration from the previous section.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        c19: getting-started
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## Add a C19 Container to the Pod

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        c19: getting-started
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
      - name: c19
        image: c19p/c19:0.1.0
        args: ["/usr/local/bin/c19", "--config", "/etc/c19/config.yaml"]
        ports:
          - name: agent
            containerPort: 3097
            protocol: TCP
          - name: connection
            containerPort: 4097
            protocol: TCP
```

As you can see, we are using the ports we defined in our C19 configuration for the agent and connection layers (3097, 4097 respectively).

## Add a Configmap and Refer to it From the Pod Definition

#### Let's first create the config map (configmap.yaml):

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: c19-getting-started-config
immutable: true
data:
  config.yaml: |
    version: 0.1
    spec:
      agent:
        kind: Default
        port: 3097
      state:
        kind: Default
        ttl: null
        purge_interval: 60000
      connection:
        kind: Default
        port: 4097
        push_interval: 1000
        pull_interval: 60000
        r0: 3
        timeout: 5000
        peer_provider:
          kind: K8s
          selector:
            c19: getting-started
          namespace: default
```

Save this as `configmap.yaml` file and let's apply this to the cluster:
```shell
kubectl apply -f configmap.yaml
```

#### And finally, let's refer to our configuration from the configmap

## The Final Deployment YAML (deployment.yaml):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        c19: getting-started
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
      - name: c19
        image: c19p/c19:0.1.0
        args: ["/usr/local/bin/c19", "--config", "/etc/c19/config.yaml"]
        ports:
          - name: agent
            containerPort: 3097
            protocol: TCP
          - name: connection
            containerPort: 4097
            protocol: TCP
        volumeMounts:
          - name: c19
            mountPath: /etc/c19
      volumes:
        - name: c19
          configMap:
            name: c19-getting-started-config
```

Save this as a `deployment.yaml` file and let's apply this to our cluster:
```shell
kubectl apply -f deployment.yaml
```
