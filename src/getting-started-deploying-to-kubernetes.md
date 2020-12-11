# Deploying to Kubernetes

We will deploy the C19 agent along side a pod that holds your application. The C19 agent will be available for your application 
at the localhost and the port used in our configuration (defaults to 3097).

![Sharing state use case](figure-4-sharing-state.png)
##### Deployment topology


Let's say you have the following nginx deployment:
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
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

The next section will show you a TL;DR of how to deploy C19 agent alongside the nginx deployment above.
We will then continue to a step-by-step guide with the important details.

