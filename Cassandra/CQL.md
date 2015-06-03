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

#### Inserting Data
```
INSERT INTO User (first_name, last_name, display_name) VALUES ('Jonathan', 'Ellis', 'CassandraChairman;);
```
You don't have to put data into all fields.
```
INSERT INTO User (first_name, display_name) VALUES ('Billy', 'DataStax CEO');
```
For the above statement, if you were to get that row from cassandra, it will display null for last_name. That's just how CQL decides to interpret the fact that there is no value there. Cassandra does not store null.

###### Optional clauses for Inserting Data
```
INSERT INTO keyspace_name.table_name 
  ( column_name, column_name, ...)
  VALUES ( value, value, ...) 
  IF NOT EXISTS
  USING option AND option;
  ```
