# The Connection Layer

The connection layer is responsible for exchanging the state with other C19 agents. It is up to the implementation of the 
connection layer to determine how the state should be exchanged, at what rate, what would be the transport layer, etc...

Different connection layer implementations might use different techniques and optimizations when exchanging data with other peers. You 
may refer to the [Appendix I / Available Connections] section to browse through available connection layer implementations and their use-case.

In the next section we will explore the `Default` connection layer implementation and its different configurations.

[Appendix I / Available Connections]: appendix-i-connections.md

