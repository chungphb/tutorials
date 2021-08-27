# Hadoop

## Big Data

* Nothing but lots of data consisting of varieties of data.
* Plays a pivotal role in the business environment today.
* Advantages:
  * Errors are known immediately.
  * Higher conversion rate.
  * The plan of action of your opposition can be seen promptly.
  * Extortion can be recognized right away and legitimate measures can be taken to restrict the harm.
* Disadvantages:
  * The data collected is raw and inconsistent.
  * Big Data is still struggling with security aspects.
  * Most of the data is hidden and can only be accessed by having the technical knowledge and expertise.
* Reasons for failure:
  * The way Big Data is perceived by the masses.
  * Lack of skilled data scientists.
  * Lack of budget.
  * Poor strategy.

## 5V

###  Volume

* The amount of data.
* Plays a crucial role to determine its value.

### Velocity

* The high speed of accumulation of data.
* Can be dealt with by sampling data.

### Variety

* The type of data.
  * Structured
  * Semi-structured
  * Unstructured
* Also the sources of data.

### Veracity

* The inconsistency and uncertainty in data.
* Affects the quality of the Big Data solution.

### Value

* The value of data.
* The most important V.

## Hadoop

### Overview

* Traditional approach
  * Data is stored on local machines.
  * Data is then processed.
  * Enterprise approach:
    * As data started increasing, data was started to be stored on remote servers.
    * Data has to be fetched from the servers and then processed upon.
    * This is very complex and expensive.
* Hadoop approach
  * Instead of fetching the data on local machines, the query is sent to the data.
  * Moreover, at the server, the query is divided into several parts.
    * These parts process the data simultaneously.
    * This is called *parallel execution* and only possible because of MapReduce.
  * Not only there is no need to fetch the data, but also the processing takes lesser time.

### RDBMS vs. Hadoop

<table>
    <thead>
        <tr>
            <th>RDBMS</th>
            <th>Hadoop</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Used for data storage, manipulation and retrieval.</td>
            <td>Used for storing data and running applications or processes concurrently.</td>
        </tr>
        <tr>
            <td>Structured data is mostly processed.</td>
            <td>Both structured and unstructured data is processed.</td>
        </tr>
        <tr>
            <td>Best suited for OLTP environment.</td>
            <td>Best suited for BIG data.</td>
        </tr>
        <tr>
            <td>Less scalable than Hadoop.</td>
            <td>Highly scalable.</td>
        </tr>
        <tr>
            <td>Data normalization is required.</td>
            <td>Data normalization is not required.</td>
        </tr>
        <tr>
            <td>Stores transformed and aggregated data.</td>
            <td>Stores huge volume of data.</td>
        </tr>
        <tr>
            <td>Has no latency in response.</td>
            <td>Has some latency in response.</td>
        </tr>
        <tr>
            <td>The data schema of RDBMS is static.</td>
            <td>The data schema of Hadoop is dynamic.</td>
        </tr>
        <tr>
            <td>High data integrity available.</td>
            <td>Low data integrity available than RDBMS.</td>
        </tr>
        <tr>
            <td>Cost is applicable for licensed software.</td>
            <td>Free of cost, as it is an open source software.</td>
        </tr>
    </tbody>
</table>

### Architecture

#### MapReduce

* An algorithm/a data structure based on the YARN framework.

* Performs the distributed processing in parallel.

* Has mainly 2 tasks:

  * Map task

    <table>
        <thead>
            <tr>
                <th>Component</th>
                <th>Usage</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>RecordReader</td>
                <td>
                    <ul>
                        <li>Breaks the records.</li>
                        <li>Provides key-value pairs for the Map() function.
                            <ul>
                                <li><i>Key:</i> The record's locational information </li>
                                <li><i>Value:</i> The data associated with the record.</li>
                            </ul>
                        </li>
                    </ul>
                </td>
            </tr>
            <tr>
                <td>Map</td>
                <td>
                    <ul>
                        <li>A user-defined function whose work is to process the pairs obtained from the RecordReader.</li>
                        <li>Either:
                            <ul>
                                <li>Does not generate any key-value pair, or</li>
                                <li>Generate multiple key-value pairs</li>
                            </ul>
                        </li>
                    </ul>
                </td>
            </tr>
            <tr>
                <td>Combiner</td>
                <td>
                    <ul>
                        <li>Groups the data in the Map workflow.</li>
                        <li>Similar to a local Reducer.</li>
                    </ul>
                </td>
            </tr>
            <tr>
                <td>Partitioner</td>
                <td>
                    <ul>
                        <li>Fetches key-value pairs generated.</li>
                        <li>Generates the shards corresponding to each Reducer.</li>
                    </ul>
                </td>
            </tr>
        </tbody>
    </table>

  * Reduce task

    <table>
        <thead>
            <tr>
                <th>Component</th>
                <th>Usage</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Shuffle and Sort</td>
                <td>
                    <ul>
                        <li>Sorts the data using its key.</li>
                        <li>Does not wait for the completion of the task performed by the Mapper</li>
                    </ul>
                </td>
            </tr>
            <tr>
                <td>Reduce</td>
                <td>
                    <ul>
                        <li>Gathers the pairs generated from the Mapper.</li>
                        <li>Performs some sorting/aggregation process on these pairs.</li>
                    </ul>
                </td>
            </tr>
            <tr>
                <td>OutpuyFormat</td>
                <td>
                    <ul>
                        <li>The key-value pairs are written into a file with the help of the record writer.
                            <ul>
                                <li>Each record is written in a new line.</li>
                                <li>The key and the value is written in a space-separated manner.</li>
                            </ul>               
                        </li>
                    </ul>
                </td>
            </tr>
        </tbody>
    </table>

#### HDFS

* Utilized for storage permission in a Hadoop cluster.
* Mainly designed for working on commodity hardware devices (inexpensive devices) working on a distributed file system design.
* Stores a large chunk of blocks better than small data blocks.
* Provides ***fault-tolerance*** and ***high availability***.
* In HDFS:
  * NameNode (Master)
    * Used for storing the Metadata.
      * The transaction logs.
      * The name of the file, size, and the information about the location of DataNode that Namenode stores to find the closest DataNode for faster communication.
  * DataNode (Slave)
    * Mainly utilized for storing the data in a Hadoop cluster.
    * The number of DataNode can vary from 1 to 500 or even more than that.
* High level architecture of Hadoop
* File block
  * Data in HDFS is always stored in terms of blocks.
  * A single block of data is divided into multiple blocks of size 128MB by default.
* Replication
  * The commodity hardware that Hadoop run on can be crashed at any time.
  * Replication ensures the availability of the data.
  * The Replication Factor for Hadoop is set to 3 by default.
* Rack awareness
  * A rack is a physical collection of Nodes in a Hadoop cluster.
  * With Racks information, Namenode can choose the closest DataNode to achieve the maximum performance. 

#### YARN

 * Performs 2 operations:
    * Job scheduling
      	* Divides a big task into small jobs so that they can be assigned to various slaves in a Hadoop cluster to maximize processing time.
      	* Keeps track of which job is important, which job has more priority, dependencies between the jobs and all the other information.
    * Resource management
      	* Manages all the resources that are made available for running a Hadoop cluster.
 * Features:
   	* Multi-tenancy
   	* Scalability
   	* Cluster-utilization
   	* Compatibility

#### Common

* Java libraries/files/scripts needed for all the other components present in a Hadoop cluster.
* Used by HDFS, YARN, and MapReduce for running the cluster.

### Versions

#### Version 1

* The first and most basic version of Hadoop.
* Includes Hadoop Common, Hadoop Distributed File System (HDFS), and Map Reduce. 

####  Version 2

* Adds YARN (Yet Another Resource Negotiator).

#### Version 3

* The most recent version of Hadoop.
* Resolves the issue of single point failure by having multiple name nodes.
* Features:
  * Economically feasible
  * Easy to use
  * Open source
  * Fault tolerance
  * Scalability
  * Distributed processing
  * Locality of data

