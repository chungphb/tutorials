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
            <td>Keyspace is the outermost container.</td>
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

