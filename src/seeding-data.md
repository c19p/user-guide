# Seeding Data

The C19 Protocol can be considered as a distributed cache layer. As with all other cache implementations, one must consider a case where they would have to seed 
data into the cache once it first loads.

When a C19 agent first launches it connects to other C19 agents and exchanges data with them. This means there's no need for seeding data as it will be in par with 
other C19 agents soon after it loads. 

We can also look back at the [Media Service] use case and see that in this case, even with a fresh new cluster we do not need to seed data as the data is being set 
with every new publisher who starts publishing to the cluster.

But there are still cases where you'd want to seed external data into your C19 cluster and for that we can consider two approaches:


## Two Approaches to Seeding Data

### 1. Using a Data Seeder
This is a simple and easy way to seed data into your C19 cluster. When a C19 agent first loads it will use a `Data Seeder` to load data 
to its `State` and will then continue as normal with exchanging this data with other C19 agents.

Please refer to [Architecture / Data Seeders] for a quick example of the `File` data seeder and to [Appendix I / Available Data Seeders] for 
a list of available data seeders.

### 2. On-Demand Data Seeding
Consider how `ARP` (Address Resolution Protocol) works: It holds a table of MAC to IP tuple and when a higher application level asks 
the ARP service for the MAC of a specific IP address it will first check its table and if it doesn't have this information it will send a request across 
the wire "who has IP address..." and will then cache the response to its table. 

We can do the same with C19. Your application can set missing values to your local 
C19 agent on-demand. If a value is missing your application will retrieve it in another way and will set the response to the local C19 agent which will then spread this 
information across other C19 agents. Since all C19 agents share the same state, your data will soon be available across your cluster, based on your demand.

[Architecture / Data Seeders]: data-seeders.md
[Appendix I / Available Data Seeders]: appendix-i-data-seeders.md
[Media Service]: use-case-media-server.md
