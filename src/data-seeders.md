# Data Seeders

When a C19 agent launches it connects to other C19 peers and exchanges data with them. This means that as soon as it launches it will be in par with other C19 agents and 
there will be no need for seeding data.

But there are cases where you'd want to seed external data into your C19 agent. We will talk about a few of these cases in the [Seeding Data] chapter.

`Data Seeders` allow you to seed external data into your C19 agent as soon as it launches. You can refer to [Appendix I / Available Data Seeders] for a list of available 
data seeders.


Let's explore the `File` data seeder.

## File Data Seeder
This data seeder will load data from a file. The data in the file must be compatible with the `State` layer you are using.
For our example, we assume the `Default` state layer implementation.

```json
{
  "cat": {
    "value": "garfield",
    "ttl": 3600000
  },
  "dog": {
    "value": "snoopy"
  }
}
```

When the C19 agent first launches it will load this data into its state and will continue with exchanging this state with other peers as normal.

[Seeding Data]: seeding-data.md
[Appendix I / Available Data Seeders]: appendix-i-data-seeders.md
