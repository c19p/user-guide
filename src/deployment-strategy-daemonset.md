# Deploying as a Daemonset

![Deployment Strategy / Daemonset][figure]

Similar to the standalone deployment strategy and if you are deploying to a Kubernetes cluster, you can have the C19 agent 
deployed as a daemonset and have it deployed to every node or a subset of nodes in your Kubernetes cluster.

You can then create a service to act as a gateway to your C19 cluster or use a node port where you will be able to 
access your C19 cluster directly on each node.

This would have your cluster-wide shared state available on every node.

[figure]: deployment-strategy-daemonset.png "Deployment Strategy / Daemonset"
