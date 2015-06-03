CQL
===

## Using Keyspaces

#### Creating a Keyspace
```
CREATE KEYSPACE example;
```

#### Create a Keyspace (only if it doesn't already exist)
```
CREATE KEYSPACE IF NOT EXISTS example;
```

#### Using a KeySpace
```
USE example;
```

#### Find out about All Keyspaces
```
DESCRIBE KEYSPACES;
```

#### Find out about a specific Keyspace
```
DESCRIBE KEYSPACE example;
```

#### Drop a Keyspace
```
DROP KEYSPACE example;
```

#### Drop a Keyspace (only if it exists)
```
DROP KEYSPACE IF EXISTS example;
```

## Tables

#### Create a Table
```
CREATE TABLE User ( 
  first_name VARCHAR,
  last_name VARCHAR,
  display_name VARCHAR,
  PRIMARY KEY (display_name)
);
```
alternatively, if there is only one primary key:
```
CREATE TABLE User ( 
  first_name VARCHAR,
  last_name VARCHAR,
  display_name VARCHAR PRIMARY KEY
);
```
