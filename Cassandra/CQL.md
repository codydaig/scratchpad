CQL
===

## Using Keyspaces

#### Creating a Keyspace
```
CREATE KEYSPACE example WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
```
You must define the replication strategy and factor. For single node clusters, the above will work just fine.

#### Create a Keyspace (only if it doesn't already exist)
```
CREATE KEYSPACE IF NOT EXISTS example WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
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

##### INSERT IS ALWAYS UPSERT!!
An insert with an existing primary key becomes an update
  - Cassandra will just write the new column value(s) provided
  - Each column insered will supersede any older values
  - For concurrent writes with the same primary key, the last write wins
  - Note that an insert doesn't necessarily have to insert all columns

#### Expiring Columns / TTL
Column data can have an optional expiration date
  - Time to Live (TTL)
  - Useful for automatically managing data that has a "shelf life"
  - Specify TTL when inserting a column
    - Specify the time in seconds
    - after the time has expired, columns will be marked as deleted
  - You can't directly change TTL for a column
    - You can, however, reinsert the data with a new TTL
    - Read the old data, then insert/upsert with a new TTL

#### SELECT - Querying for Data
```
SELECT select_expression
  FROM keyspace_name.table_name
  WHERE relation AND relation ...
  ORDER BY ( clustering_column (ASC | DESC) ... )
  LIMIT n
  ALLOW FILTERING;
```
- The WHERE clause can only be an equality (equals or IN)
- Can't query on non-primary-key columns that aren't indexed

#### CQL Data Types
- Numbers: int, bigint, float, double, decimal(variable-precision decimal), varint (variable-precision integer)
- Text: text, ascii, varchar (text and varchar are synonyms)
- Time: timestamp, timeuuid
- Collections: list, map, set
- Others: uuid, boolean, blob, counter, inet

#### Deleting Data
###### Syntax:
```
DELETE column_name, ... | ( column_name term )
  FROM keyspace_name.table_name
  USING TIMESTAMP integer
  WHERE row_specification
  ( IF ( EXISTS | ( condition( AND condition ) ... ) ) );
```

###### Deleting an Entire Row
```
DELETE FROM user WHERE display_name = 'DataStax MVP';
```

###### Deleting a Column from a Row
```
DELETE first_name FROM user WHERE display_name = 'President 44';
```

###### Not Really Deleted?
In distributed systems, replicas may recieve delete requests at different times. So depending on the consistency level and replication factor, some replicas may have a delete, while others do not. Also, data is stored in immutable SSTables, and a given partition key may have values that span multiple SSTables, so an SSTable may contain data that has been deleted. 

By initiating a delete, cassandra actually indicates that a delete occured, and when it happened. Then it uses the standard write mechanism to propagata the delete to other replicas. 

