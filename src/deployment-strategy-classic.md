# Deploying Alongside a Service

![Sharing state use case][figure]

As seen in the [Sharing State] use case, this deployment strategy has the c19 agent attached to your application 
by deploying it in the same pod as your application. It is then available to your application on the localhost.

You configure the C19 to be part of a group of other C19 agents that are attached to your other services and share the state 
between them. Note that the other C19 agent can be attached to different kinds of services. 

Please refer to [Getting Started / Deploying to Kubernetes] chapter for a walkthrough on how to do that.

[figure]: figure-4-sharing-state.png "Sharing state use case"
[Getting Started / Deploying to Kubernetes]: getting-started-deploying-to-kubernetes.md
[Sharing State]: use-case-sharing-state.md
