CQL
===

## Using Keyspaces

Creating a Keyspace
```
CREATE KEYSPACE example;
```

Create a Keyspace (only if it doesn't already exist)
```
CREATE KEYSPACE IF NOT EXISTS example;
```

Using a KeySpace
```
USE example;
```

Find out about All Keyspaces
```
DESCRIBE KEYSPACES;
```

Find out about a specific Keyspace
```
DESCRIBE KEYSPACE example;
```

Drop a Keyspace
```
DROP KEYSPACE example;
```

Drop a Keyspace (only if it exists)
```
DROP KEYSPACE IF EXISTS example;
```

