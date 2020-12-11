# Introduction

Welcome to the C19 protocol documentation! In this user guide you will find everything needed to understand and run the C19 protocol.

At its core, the C19 protocol goal is to share a state across a distributed set of services, handle fetching the data and make it available locally to be consumed by a service.
Due to its distributed nature and low resource footprint, you gain redundancy, robustness and performance at a low cost.

Some of the ideas employed by this implementation are:
- Periodic interaction between agents that is decoupled from the change in state. This allows robustness even on high load systems where the state changes at a high rate.
- Bi-directional exchange of states to speed up state propagation across the system.
- Inherent replication of data which gives you redundancy and reliability.

Most importantly, the system is designed to be extensible and as you will see in the following chapters, you have full control and flexibility to choose the best strategy to 
match your data and your needs.
