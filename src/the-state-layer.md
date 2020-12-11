# The State Layer

The state layer is where your data is being held. The State is responsible for the way it holds your data and represents the data structure.
A State can be a key/value store, a blob of data or anything else that is either structured or unstructured.

The State exposes ways to set and get data to and from it and is being used by the Agent and Connection layers.

Let's inspect the `Default` state layer implementation. We'll use that as an example of how a state behaves and what configuration is available.
When you feel comfortable with what a `State` is, you can refer to [Appendix I / Available States] to browse through available states and choose the 
one that fits your use case. Make sure to check that section often as we expect this list of available states to grow.

[Appendix I / Available States]: appendix-i-states.md
