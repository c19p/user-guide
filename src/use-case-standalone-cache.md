# Standalone Distributed Cache

Due to it's nature, the C19 protocol can be deployed as a standalone distributed cache and can be easily horizontally scaled.

As you've seen with the previous use-cases, the C19 agent is deployed within a pod, alongside the service it serves. As you will learn 
in [Deployment Strategies] chapter, the C19 protocol can be deployed as a standalone 
distributed cache or even as a daemonset to be present on every node.

![Media server use case](use-case-standalone.png)
##### Standalone C19 cluster

[Deployment Strategies]: deployment-strategies.md
