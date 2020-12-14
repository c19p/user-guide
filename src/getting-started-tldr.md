# Deploying to Kubernetes / TL;DR

To attach a C19 agent to your nginx deployment, you will need two files:
1. A configmap that will hold your C19 configuration as we saw in [Configuring the Agent] chapter.
2. A Kubernetes deployment file.

# 1. Configmap
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
        timeout: 1000
        peer_provider:
          kind: K8s
          selector:
            c19: getting-started
          namespace: default
```

Save this as `configmap.yaml` file and apply it to the cluster like so:
```shell
kubectl apply -f configmap.yaml
```

[Configuring the Agent]: getting-started-configuration.md

# 2. Deployment File
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

Save this as `deployment.yaml` file and apply it to the cluster like so:
```shell
kubectl apply -f deployment.yaml
```

**That's it! Your C19 powered Nginx is now deployed to Kubernetes and is ready to be used.**

In the next section we will go through what we just did with a bit more details. Don't worry, it will still feel like a TL;DR.
If you feel comfortable with what we did above, you can skip the step-by-step walkthrough and jump right into [Testing the Deployment].

[Testing the Deployment]: getting-started-test-deployment.md
