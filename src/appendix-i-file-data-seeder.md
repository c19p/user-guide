# File Data Seeder

The File data seeder loads data from a file.

## Configuration
```yaml
data_seeder: 
  kind: File
  filename: data.json
```

`filename` - The filename to load.

## Behavior
When a `State` layer calls this data seeder it will look for the specified file and will load the data into the state. It is up to the user 
to make sure the file is formatted as expected by the `State` layer.
