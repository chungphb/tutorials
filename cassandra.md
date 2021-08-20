# Cassandra

## Tutorial

### Introduction

#### Apache Cassandra

* A column-oriented database.
* Scalable, fault-tolerant, and consistent.
* Being used by some of the biggest companies such as Facebook, Twitter, Cisco, Netflix, etc.

#### Features

* Elastic scalability
* Always-on architecture
* Fast linear-scale performance
* Flexible data storage
* Easy data distribution
* Fast writes

### Architecture

#### Design

* Cassandra has ***peer-to-peer distributed system*** across its nodes.
  * All the nodes in a cluster plays the same role.
  * Each node is independent but interconnected to other nodes.
* Data is distributed among all the nodes in a cluster.
  * ***Each node in a cluster can accept read and write requests***, regardless of where the data is actually located.
  * When a node goes down, read/write requests can be served from other nodes in the network.

#### Data Replication 

* In Cassandra, one or more nodes in a cluster act as replicas for a given piece of data.
  * If some of the nodes responded with an out-of-date value, Cassandra will return the most recent value to the client.
  * After returning this value, Cassandra performs a ***read repair*** in the background to update the stale values.
* Cassandra uses the ***Gossip protocol*** in the background to allow the nodes to communicate with each other and detect any faulty nodes in the cluster.

#### Components

<table>
    <thead>
        <tr>
            <th>Component</th>
            <th>Usage</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Node</td>
            <td>The place where data is stored.</td>
        </tr>
        <tr>
            <td>Data center</td>
            <td>A collection of related nodes.</td>
        </tr>
        <tr>
            <td>Cluster</td>
            <td>A component that contains one or more data centers.</td>
        </tr>
        <tr>
            <td>Commit log</td>
            <td>
                A crash-recovery mechanism in Cassandra.<br/>
                <i><b>Every write operation is written to the commit log.</b></i>
            </td>
        </tr>
        <tr>
            <td>Memtable</td>
            <td>
                A memory-resident data structure.<br/>
                After commit log, the data will be written to the memtable.
            </td>
        </tr>
        <tr>
            <td>SSTable</td>
            <td>A disk file to which the data is flushed from the memtable when its contents reach a threshold value.</td>
        </tr>
        <tr>
            <td>Bloom filter</td>
            <td>Quick and nondeterministic algorithms for testing whether an element is a member of a set.</td>
        </tr>
    </tbody>
</table>

#### CQL

* Users can access Cassandra through its nodes using ***Cassandra Query Language (CQL)***.

  * CQL treats the database (Keyspace) as a container of tables.
  * Programmers use ***cqlsh***: a prompt to work with CQL or separate application language drivers.

* Clients approach any of the nodes for their read-write operations. That node (***coordinator***) plays a proxy between the client and the nodes holding the data.

* Write operations

  * Every write is captured by the commit log.
  * Later, data will be captured and stored in the memtable.
  * When the memtable is full, data will be written into the SSTable data file.
  * All writes are automatically partitioned and replicated.
  * Unnecessary data in the SSTables will be discarded periodically.

* Read operations

  Cassandra will find the values in the memtable before checking the Bloom filter to find the appropriate SSTable that holds the required data.

### Data Model

#### Keyspace

* The outermost container for data in Cassandra.

* Attributes:

  <table>
      <thead>
          <tr>
              <th>Attribute</th>
              <th>Meaning</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>Replication factor</td>
              <td>The number of machines in the cluster that will receive copies of the same data.</td>
          </tr>
          <tr>
              <td>Replica placement strategy</td>
              <td>
                  The strategy to place replicas in the ring.
                  <ul>
                      <li>Simple strategy</li>
                      <li>Network topology strategy</li>
                  </ul>
              </td>
          </tr>
          <tr>
              <td>Column family</td>
              <td>
                  <ul>
                      <li>A container of a collection of rows.</li>
                      <li>Each row contains ordered columns.</li>
                  </ul>
              </td>
          </tr>
      </tbody>
  </table>

#### Column Family

* A container for an ordered collection of rows.

* Differences between a column family and a table of relational databases:

  <table>
      <thead>
          <tr>
              <th>Relational Table</th>
              <th>Cassandra Column Family</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>
                  <ul>
                      <li>A schema in a relational model is fixed.</li>
                      <li>Once columns are defined, while inserting data, in every row <i><b>all the columns must be filled</b></i> at least with a null value.</li>
                  </ul>
              </td>
              <td>ALthough the column families are defined, the columns are not.</td>
          </tr>
          <tr>
              <td>Relational tables define only columns.</td>
              <td>A table contains columns, or can be defined as a super column family.</td>
          </tr>
      </tbody>
  </table>

* Attributes:

  <table>
      <thead>
          <tr>
              <th>Attribute</th>
              <th>Meaning</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td><i>keys_cached</i></td>
              <td>The number of locations to keep cached per SSTable.</td>
          </tr>
          <tr>
              <td><i>rows_cached</i></td>
              <td>The number of rows whose entire contents will be cached in memory.</td>
          </tr>
          <tr>
              <td><i>preload_row_cache</i></td>
              <td>Whether you want to pre-populate the row cache.</td>
          </tr>
      </tbody>
  </table>

#### Column

* The basic data structure of Cassandra.
* Includes three values:
  * Name
  * Value
  * Timestamp

####  Super Column

* A special column with a key-value pair, stores a map of sub-columns.
* To optimize performance, it is important to keep columns that you are likely to query together in the same column family, and using a super column can be helpful.

#### Data Models of Cassandra and RDBMS

<table>
    <thead>
        <tr>
            <th>RDBMS</th>
            <th>Cassandra</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Deals with structured data.</td>
            <td>Deals with unstructured data.</td>
        </tr>
        <tr>
            <td>Has a fixed schema.</td>
            <td>Has a flexible schema.</td>
        </tr>
        <tr>
            <td>A table is an array of arrays. (ROW x COLUMN)</td>
            <td>A table is a list of "nested key-value pairs". (ROW x COLUMN key x COLUMN value)</td>
        </tr>
        <tr>
            <td>Database is the outermost container.</td>
            <td>Keyspace is the outermost container.</td>
        </tr>
        <tr>
            <td>Tables are the entities of a database.</td>
            <td>Tables or column families are the entity of a keyspace.</td>
        </tr>
        <tr>
            <td>Row is an individual record.</td>
            <td>Row is a <i><b>unit of replication.</b></i></td>
        </tr>
        <tr>
            <td>Column represents the attributes.</td>
            <td>Column is a <i><b>unit of storage.</b></i></td>
        </tr>
        <tr>
            <td>Supports the concepts of foreign keys, joins etc.</td>
            <td>Relationships are represented using <i><b>collections.</b></i></td>
        </tr>
    </tbody>
</table>


## Keyspace Operations

### Create

#### Syntax

```cassandra
CREATE KEYSPACE <identifier> WITH <properties>;

// Detail:
CREATE KEYSPACE 'keyspace name'
WITH replication = {'class': 'strategy name', 'replication_factor': 'number of replicas'}
AND durable_writes = 'boolean value';
```

* `replication`

  * *Usage:* Specifies the Replica Placement Strategy and the number of replicas wanted.

    <table>
        <thead>
            <tr>
                <th>Strategy</th>
                <th>Description</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Simple Strategy</td>
                <td>Specifies a simple replication factor for the cluster.</td>
            </tr>
            <tr>
                <td>Network Topology Strategy</td>
                <td>Sets the replication factor for each data-center independently..</td>
            </tr>
            <tr>
                <td>Old Network Topology Strategy</td>
                <td>A legacy replication strategy.</td>
            </tr>
        </tbody>
    </table>

  * *Example:*

    ```cassandra
    CREATE KEYSPACE sampleKeyspace
    WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3}
    ```

  * *Verification:*

    ```cassandra
    DESCRIBE KEYSPACES;
    ```

* `durable_writes`

  * *Usage:* Allows you to instruct Cassandra whether to use commit log for updates on the current keyspace.

  * *Example:*

    ```cassandra
    CREATE KEYSPACE sampleKeyspace
    WITH replication = {'class': 'NetworkTopologyStrategy, 'data_center': 3}
    AND durable_writes = false;
    ```

  * *Verification:*

    ```cassandra
    SELECT * FROM system_schema.keyspaces;
    ```

* Using a Keyspace

  * *Syntax:*

    ```cassandra
    USE <identifier>;
    ```

  * *Example:*

    ```cassandra
    USE sampleKeyspace;
    ```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "CREATE KEYSPACE sampleKeyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};";
session.execute(query);
```

### Alter

#### Syntax

```cassandra
ALTER KEYSPACE <identifier> WITH <properties>;

// Detail:
ALTER KEYSPACE 'keyspace name'
WITH replication = {'class': 'strategy name', 'replication_factor': 'number of replicas'}
AND durable_writes = 'boolean value';
```

#### Example

```cassandra
ALTER KEYSPACE sampleKeyspace
WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};
```

#### Verification

```cassandra
DESCRIBE KEYSPACES;
SELECT * FROM system_schema.keyspaces;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "ALTER KEYSPACE sampleKeyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3};";
session.execute(query);
```

### Drop

#### Syntax

```cassandra
DROP KEYSPACE <identifier>;

// Detail:
DROP KEYSPACE 'keyspace name';
```

#### Example

```cassandra
DROP KEYSPACE sampleKeyspace;
```

#### Verification

```cassandra
DESCRIBE KEYSPACES;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = DROP KEYSPACE sampleKeyspace;";
session.execute(query);
```

## Table Operations

### Create

#### Syntax

```cassandra
CREATE (TABLE | COLUMNFAMILY) 'table name'
('column definition', 'column definition', ...)
(WITH <option> AND <option>);

// Column definition:
'column name' 'data type',
'column name' 'data type',
...

// Primary key:
CREATE TABLE 'table name' (
    'column definition' PRIMARY KEY,
    'column definition',
    ...
);

CREATE TABLE 'table name' (
    'column definition',
    'column definition',
    ...,
    PRIMARY KEY ('column name', ...)
);
```

#### Example

```cassandra
USE sampleKeyspace;
CREATE TABLE sampleTable (
    sampleID int PRIMARY KEY,
    sampleName text,
    sampleInfo text
);
```

#### Verification

```cassandra
SELECT * FROM sampleTable;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "CREATE TABLE sampleTable ("
    + "sampleID int PRIMARY KEY,"
    + "sampleName text,"
    + "sampleInfo text"
    + ");";
session.execute(query);
```

### Alter

#### Syntax

```cassandra
ALTER (TABLE | COLUMNFAMILY) 'table name' 'instruction';

// Adding a column:
ALTER TABLE 'table name'
ADD 'new column' 'data type';

// Dropping a column:
ALTER TABLE 'table name'
DROP 'column name';
```

#### Example

```cassandra
// Adding a column:
ALTER TABLE sampleTable
ADD sampleMoreInfo text;

// Dropping a column:
ALTER TABLE sampleTable
DROP sampleMoreInfo;
```

#### Verification

```cassandra
SELECT * FROM sampleTable;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
// Adding a column:
String query = "ALTER TABLE sampleTable ADD sampleMoreInfo text;";

// Dropping a column:
String query = "ALTER TABLE sampleTable DROP sampleMoreInfo;";

session.execute(query);
```

### Drop

#### Syntax

```cassandra
DROP (TABLE | COLUMNFAMILY) 'table name';
```

#### Example

```cassandra
DROP TABLE sampleTable;
```

#### Verification

```cassandra
DESCRIBE (TABLES | COLUMNFAMILIES);
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "DROP TABLE sampleTable;";
session.execute(query);
```

### Truncate

#### Syntax

```cassandra
TRUNCATE [TABLE] 'table name';
```

#### Example

```cassandra
TRUNCATE sampleTable;
```

#### Verification

```cassandra
SELECT * FROM sampleTable;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "TRUNCATE sampleTable;";
session.execute(query);
```

### Create Index

#### Syntax

```cassandra
CREATE INDEX 'index name' ON 'table name';
```

#### Example

```cassandra
CREATE INDEX sampleIndex ON sampleTable(sampleName);
```

#### Verification

```cassandra
DESCRIBE TABLE sampleTable;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "CREATE INDEX sampleIndex ON sampleTable(sampleName);";
session.execute(query);
```

### Drop Index

#### Syntax

```cassandra
DROP INDEX [IF EXISTS] 'index name';
```

#### Example

```cassandra
DROP INDEX IF EXISTS sampleIndex;
```

#### Verification

```cassandra
DESCRIBE TABLE sampleTable;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "DROP INDEX IF EXISTS sampleIndex;";
session.execute(query);
```

### Batch Statements

#### Syntax

```cassandra
BEGIN BATCH
'<Insert statement>', or
'<Update statement>', or
'<Delete statement>'
APPLY BATCH;
```

#### Example

```cassandra
BEGIN BATCH
INSERT INTO sampleTable(sampleID, sampleName, sampleInfo) VALUES (3, 'Chung Pham', 'Vietnam');
UPDATE sampleTable SET sampleName = 'Bernie Sanders' WHERE sampleID = 1;
DELETE sampleInfo FROM sampleTable WHERE sampleID = 2;
APPLY BATCH;
```

#### Java API

Step 1. *Create a Cluster object*

```java
Cluster cluster = Cluster.builder().addContactPoint("127.0.0.1").build();
```

Step 2. *Create a Session object*

```java
Session session = cluster.connect("sampleKeyspace");
```

Step 3. *Execute the query*

```java
String query = "BEGIN BATCH"
    + "INSERT INTO sampleTable(sampleID, sampleName, sampleInfo) VALUES (3, 'Chung Pham', 'Vietnam');"
    + "UPDATE sampleTable SET sampleName = 'Bernie Sanders' WHERE sampleID = 1;"
    + "DELETE sampleInfo FROM sampleTable WHERE sampleID = 2;"
    + "APPLY BATCH;";
session.execute(query);
```