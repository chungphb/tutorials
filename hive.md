# Hive

## Introduction

### Hadoop

* An open-source framework to ***store and process Big Data*** in a ***distributed environment***.
* Contains two modules:
  * ***MapReduce:*** A parallel programming model for processing large amounts of data on large clusters of commodity hardware.
  * ***Hadoop Distributed File System (HDFS):*** A part of Hadoop framework that provides a fault-tolerant file system to run on commodity hardware.

* The Hadoop ecosystem contains different tools that are used to support Hadoop modules.
* There are several ways to execute MapReduce operations:
  * The traditional approach using Java MapReduce program ***for structured, semi-structured, and unstructured data***.
  * The scripting approach for MapReduce to process ***structured and semi-structured data*** using Pig.
  * The Hive Query Language (HiveQL or HQL) for MapReduce to process ***structured data*** using Hive.

### Hive

* A data warehouse infrastructure tool to process ***structured data*** in Hadoop.
* Developed by Facebook, but used by different companies.
* Not:
  * A relational database
  * A design for Online Transaction Processing (OLTP)
  * A language for real-time queries and row-level updates

### Features

* Stores schema in a database and processed data in HDFS.
* Designed for Online Analytical Processing (OLAP).
* Provides SQL type language for querying.
* Familiar, fast, scalable, and extensible.

### Architecture

<table>
    <thead>
        <tr>
            <th>Unit</th>
            <th>Operation</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>User Interface</td>
            <td>
                Supports:
                <ul>
                    <li>Hive Web UI</li>
                    <li>Hive Command Line</li>
                    <li>Hive HDInsight</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Metastore</td>
            <td>Stores the schema or the metadata of databases, tables, columns in a table, their data types, and HDFS mapping.</td>
        </tr>
        <tr>
            <td>HiveQL Process Engine</td>
            <td>
                Processes HiveQL:
                <ul>
                    <li>HiveQL is one of the replacements of the traditional approach for MapReduce program.</li>
                    <li>We can write a query for MapReduce job and process it instead of writing a MapReduce program in Java.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Execution Engine</td>
            <td>The conjunction part between HiveQL Process Engine and MapReduce.</td>
        </tr>
        <tr>
            <td>HDFS/HBASE</td>
            <td>The data storage techniques to store data into file system.</td>
    </tbody>
</table>

### Workflow

1. *Execute query*

   User Interface sends the query to Driver (any database driver) to execute.

2. *Get plan*

   Driver sends the query to Compiler to check the syntax and the query plan or the requirement of the query.

3. *Get metadata*

   Compiler sends a metadata request to Metastore.

4. *Send metadata*

   Metastore sends the metadata as a response to Compiler.

5. *Send plan*

   Compiler checks the requirement and sends the plan back to Driver.

   The parsing and compiling of a query is complete.

6. *Execute plan*

   Driver sends the plan to Execution Engine.

7. *Execute job*

   7.1.

   - Internally, the process of execution job is a MapReduce job.
   - The execution engine sends the job to ***JobTracker***, which is in ***Name node*** and it assigns this job to ***TaskTracker***, which is in ***Data node***.
   - Here, the query executes MapReduce job.

   7.2.

   * Meanwhile, Execution Engine can execute metadata operations with Metastore.

8. *Fetch results*

   Execution Engine receives the results from Data nodes.

9. *Send results*

   Execution Engine sends the results to Driver.

10. *Send results*

    Driver sends the results to User Interface.

### Data Types

#### Column Types

* Integral Types

  <table>
      <thead>
          <tr>
              <th>Type</th>
              <th>Postfix</th>
              <th>Example</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>TINYINT</td>
              <td>Y</td>
              <td>0Y</td>
          </tr>
          <tr>
              <td>SMALLINT</td>
              <td>S</td>
              <td>0S</td>
          </tr>
          <tr>
              <td>INT</td>
              <td>-</td>
              <td>0</td>
          </tr>
          <tr>
              <td>BIGINT</td>
              <td>L</td>
              <td>4L</td>
          </tr>
      </tbody>
  </table>

* String Types

  <table>
      <thead>
          <tr>
              <th>Type</th>
              <th>Maximum length</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>VARCHAR</td>
              <td>1 to 65355</td>
          </tr>
          <tr>
              <td>CHAR</td>
              <td>255</td>
          </tr>
      </tbody>
  </table>

* Timestamps

  * Supports traditional UNIX timestamp with optional nanosecond precision.
  * Supported conversions:
    * Integer numeric types
    * Floating point numeric types
    * Strings
  * Timezoneless and stored as an offset from the UNIX epoch.

* Dates

  *  Describe a particular year/month/day in the form `YYYY-­MM-­DD`.
  * The range of values supported for the Date type is 0000-­01-­01 to 9999-­12-­31.

* Intervals

  <table>
      <thead>
          <tr>
              <th>Supported description</th>
              <th>Example</th>
              <th>Meaning</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>Intervals of time units: SECOND / MINUTE / DAY / MONTH / YEAR</td>
              <td>INTERVAL '1' YEAR</td>
              <td>An interval of 1 year(s)</td>
          </tr>
          <tr>
              <td>Year to month intervals: SY-M</td>
              <td>INTERVAL '1-6' YEAR TO MONTH</td>
              <td>INTEVAL '1' YEAR + INTERVAL '6' MONTH</td>
          </tr>
          <tr>
              <td>Day to second intervals: SD H:M:S.nnnnnn</td>
              <td>INTERVAL '1 4:12:23.000025' DAY</td>
              <td>INTEVAL '1' DAY + INTERVAL '4' HOUR + INTERVAL '12' MINUTE + INTERVAL '13' SECOND + INTERVAL '25' NANO</td>
          </tr>
          <tr>
              <td>Intervals with constant numbers</td>
              <td>INTERVAL 1 DAY</td>
              <td>Aids query readability</td>
          </tr>
          <tr>
              <td>Intervals with expressions</td>
              <td>INTERVAL (1 + dt) DAY</td>
              <td>Enables dynamic intervals</td>
          </tr>
          <tr>
              <td>Optional INTERVAL keyword</td>
              <td>1 YEAR</td>
              <td>INTERVAL 1 YEAR</td>
          </tr>
          <tr>
              <td>Timeunit alias: SECONDS / MINUTES / HOURS / DAYS / WEEKS / MONTHS / YEARS</td>
              <td>6 MONTHS</td>
              <td>6 MONTH</td>
          </tr>
      </tbody>
  </table>

* Decimals

  * Represents immutable arbitrary precision decimal numbers.

  * *Syntax:*

    ````hive
    DECIMAL(precision, scale)
    ````

  * *Example:*

    ```hive
    DECIMAL(10, 4)
    ```

#### Literals

* Floating Point Literals
  * Assumed to be DOUBLE.
* Decimal Literals
  * Provides precise values and greater range for floating point numbers than the DOUBLE type.

#### Complex Types

* Arrays

  * *Syntax:* `ARRAY<data_type>`

  * *Example:* `ARRAY<STRING>`
  
* Maps

  * *Syntax:* `MAP<primitive_type, data_type>`

  * *Example:* `MAP<INT, STRING>`
  
* Structs

  * *Syntax:* `STRUCT<col_name : data_type [COMMENT col_comment], ...>`

  * *Example:* `STRUCT<col1 : INT COMMENT 'Column 1', col2 : STRING COMMENT 'Column 2'>`
  
* Unions

  * *Syntax:* `UNIONTYPE<data_type, data_type, ...>`

  * *Example:* `UNIONTYPE<INT, STRING>`

## Database Operations

### Create Database

#### Syntax

```hive
CREATE [REMOTE] (DATABASE|SCHEMA) [IF NOT EXISTS] database_name
[COMMENT database_comment]
[LOCATION hdfs_path]
[MANAGEDLOCATION hdfs_path]
[WITH DBPROPERTIES (property_name=property_value, ...)];
```

#### Example

```hive
CREATE DATABASE IF NOT EXISTS sampleDatabase;
```

#### Verification

```hive
SHOW DATABASES;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();
statement.executeQuery("CREATE DATABASE IF NOT EXISTS sampleDatabase;");
```

#### Use Database

```hive
USE database_name;
```

### Alter Database

#### Syntax

```hive
// Altering the properties of the database:
ALTER (DATABASE|SCHEMA) database_name SET DBPROPERTIES (property_name = property_value, ...);

// Altering the owner of the database:
ALTER (DATABASE|SCHEMA) database_name SET OWNER [USER|ROLE] user_or_role;

// Altering the location of the database:
ALTER (DATABASE|SCHEMA) database_name SET LOCATION hdfs_path;

// Altering the managed location of the database:
ALTER (DATABASE|SCHEMA) database_name SET MANAGEDLOCATION hdfs_path;
```

#### Example

```hive
// Altering the properties of the database:
ALTER DATABASE sampleDatabase SET DBPROPERTIES ('edited-by' = 'ChungPHB');

// Altering the owner of the database:
ALTER DATABASE sampleDatabase SET OWNER chungphb;

// Altering the location of the database:
ALTER DATABASE sampleDatabase SET LOCATION '/path/to/location';

// Altering the managed location of the database:
ALTER DATABASE sampleDatabase SET MANAGEDLOCATION '/path/to/managed/location';
```

#### Verification

```hive
DESCRIBE DATABASE EXTENDED sampleDatabase;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();

// Altering the properties of the database:
statement.executeQuery("ALTER DATABASE sampleDatabase SET DBPROPERTIES ('edited-by' = 'ChungPHB');");

// Altering the owner of the database:
statement.executeQuery("ALTER DATABASE sampleDatabase SET OWNER chungphb;");

// Altering the location of the database:
statement.executeQuery("ALTER DATABASE sampleDatabase SET LOCATION '/path/to/location';");

// Altering the managed location of the database:
statement.executeQuery("ALTER DATABASE sampleDatabase SET MANAGEDLOCATION '/path/to/managed/location';");
```

### Drop Database

#### Syntax

```hive
DROP (DATABASE|SCHEMA) [IF EXISTS] database_name [RESTRICT|CASCADE];
```

#### Example

```hive
DROP DATABASE IF EXISTS sampleDatabase CASCADE;
```

#### Verification

```hive
SHOW DATABASES;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();
statement.executeQuery("DROP DATABASE IF EXISTS sampleDatabase CASCADE;");
```

## Table Operations

### Create Table

#### Syntax

```hive
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
[(col_name data_type [column_constraint_specification] [COMMENT col_comment], ... [constraint_specification])]
[COMMENT table_comment]
[PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
[CLUSTERED BY (col_name, col_name, ...) [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
[SKEWED BY (col_name, col_name, ...) ON ((col_value, col_value, ...), (col_value, col_value, ...), ...) [STORED AS DIRECTORIES]]
[
	[ROW FORMAT row_format]
	[STORED AS file_format] | STORED BY 'storage.handler.class.name' [WITH SERDEPROPERTIES (...)]
]
[LOCATION hdfs_path]
[TBLPROPERTIES (property_name=property_value, ...)]
[AS select_statement];
```

#### Example

```hive
CREATE TABLE IF NOT EXISTS sampleTable
(sampleID INT, sampleName STRING, sampleInfo STRING)
COMMENT 'Sample table'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```

#### Verification

```hive
SHOW TABLES;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();
statement.executeQuery("CREATE TABLE IF NOT EXISTS sampleTable"
+ "(sampleID INT, sampleName STRING, sampleInfo STRING)"
+ "COMMENT 'Sample table'"
+ "ROW FORMAT DELIMITED"
+ "FIELDS TERMINATED BY '\t'"
+ "LINES TERMINATED BY '\n'"
+ "STORED AS TEXTFILE;");
```

### Alter Table

#### Syntax

```hive
// Renaming a table:
ALTER TABLE name RENAME TO new_name;

// Adding new columns to a table:
ALTER TABLE name ADD COLUMNS (col_spec[, col_spec ...]);

// Dropping a column from a table: 
ALTER TABLE name DROP [COLUMN] column_name;

// Updating a column of a table:
ALTER TABLE name CHANGE column_name new_name new_type;

// Replacing columns of a table:
ALTER TABLE name REPLACE COLUMNS (col_spec[, col_spec ...]);
```

#### Example

```hive
// Renaming a table:
ALTER TABLE sampleTable RENAME TO newSampleTable;

// Adding new columns to a table:
ALTER TABLE sampleTable ADD COLUMNS (sampleMoreInfo STRING COMMENT 'More info');

// Dropping a column from a table: 
ALTER TABLE sampleTable DROP sampleMoreInfo;

// Updating a column of a table:
ALTER TABLE sampleTable CHANGE sampleInfo newSampleInfo STRING;

// Replacing columns of a table:
ALTER TABLE sampleTable REPLACE COLUMNS (newSampleID INT, newSampleName STRING, newSampleInfo STRING);
```

#### Verification

```hive
SELECT * FROM sampleTable;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();

// Renaming a table:
statement.executeQuery("ALTER TABLE sampleTable RENAME TO newSampleTable;");

// Adding new columns to a table:
statement.executeQuery("ALTER TABLE sampleTable ADD COLUMNS (sampleMoreInfo STRING COMMENT 'More info');");

// Dropping a column from a table: 
statement.executeQuery("ALTER TABLE sampleTable DROP sampleMoreInfo;");

// Updating a column of a table:
statement.executeQuery("ALTER TABLE sampleTable CHANGE sampleInfo newSampleInfo STRING;");

// Replacing columns of a table:
statement.executeQuery("ALTER TABLE sampleTable REPLACE COLUMNS (newSampleID INT, newSampleName STRING, newSampleInfo STRING);");
```

### Drop Table

#### Syntax

```hive
DROP TABLE [IF EXISTS] table_name;
```

#### Example

```hive
DROP TABLE IF EXISTS sampleTable;
```

#### Verification

```hive
SHOW TABLES;
```

#### JDBC program

Step 1. *Register driver and create driver instance*

```java
String driverName = "org.apache.hadoop.hive.jdbc.HiveDriver"
Class.forName(driverName);
```

Step 2. *Get connection*

```java
Connection connection = DriverManager.getConnection("jdbc:hive://localhost:10000/default", "", "");
```

Step 3. *Create and execute statement*

```java
Statement statement = connection.createStatement();
statement.executeQuery("DROP TABLE IF EXISTS sampleTable;");
```

## Partitioning

Hive organizes tables into ***partitions***.

* A way of dividing a table into related parts based on the values of partitioned columns.
* Easier to query a portion of the data using partitions.

Tables or partitions are subdivided into ***buckets***.

* Provides extra structure to the data that may be used for more efficient querying.
* Based on the value of the hash function of some column of the table.

### Add Partition

#### Syntax

```hive
ALTER TABLE table_name
ADD [IF NOT EXISTS] PARTITION partition_spec [LOCATION 'location'][, PARTITION partition_spec [LOCATION 'location'], ...];

partition_spec
	: (partition_column = partition_col_value, partition_column = partition_col_value, ...)
```

#### Example

```hive
ALTER TABLE sampleTable
ADD
PARTITION (sampleInfo = 'sampleValue1') LOCATION '/path/to/partition1',
PARTITION (sampleInfo = 'sampleValue2') LOCATION '/path/to/partition2';
```

#### Verification

```hive
SHOW PARTITIONS sampleTable;
```

### Alter Partition

#### Syntax

```hive
// Renaming a partititon:
ALTER TABLE table_name PARTITION partition_spec
RENAME TO PARTITION partition_spec;

// Altering table/partition file format:
ALTER TABLE table_name [PARTITION partition_spec]
SET FILEFORMAT file_format;

// Altering table/partition location:
ALTER TABLE table_name [PARTITION partition_spec]
SET LOCATION 'new location';
```

#### Example

```hive
// Renaming a partititon:
ALTER TABLE sampleTable PARTITION (sampleInfo = 'sampleValue1')
RENAME TO PARTITION (sampleName = 'sampleValue1');

// Altering table/partition file format:
ALTER TABLE sampleTable PARTITION (sampleInfo = 'sampleValue1')
SET FILEFORMAT ORC;

// Altering table/partition location:
ALTER TABLE sampleTable PARTITION (sampleInfo = 'sampleValue1')
SET LOCATION '/new/path/to/partition1';
```

#### Verification

```hive
SHOW PARTITIONS sampleTable PARTITION (sampleInfo = 'sampleValue1');
```

### Drop Partition

#### Syntax

```hive
ALTER TABLE table_name
DROP [IF EXISTS] PARTITION partition_spec[, PARTITION partition_spec, ...];
```

#### Example

```hive
ALTER TABLE sampleTable
DROP IF EXISTS PARTITION (sampleInfo = 'sampleValue1');
```

#### Verification

```hive
SHOW PARTITIONS sampleTable;
```

##  View Operations

### Create View

#### Syntax

```hive
CREATE VIEW [IF NOT EXISTS] [db_name.]view_name [(column_name [COMMENT column_comment], ...)]
[COMMENT view_comment]
[TBLPROPERTIES (property_name = property_value, ...)]
AS SELECT ...;
```

#### Example

```hive
CREATE VIEW sampleView
AS
SELECT *
FROM sampleTable
WHERE sampleID = 0;
```

#### Verification

```hive
SHOW TABLES;
```

### Alter View

```hive
// Altering properties:
ALTER VIEW [db_name.]view_name SET TBLPROPERTIES table_properties;

// Altering as SELECT:
ALTER VIEW [db_name.]view_name AS select_statement;
```

#### Example

```hive
ALTER VIEW sampleView AS SELECT * FROM sampleTable WHERE sampleID = 1;
```

#### Verification

```hive
SHOW TABLES;
```

### Drop View

#### Syntax

```hive
DROP VIEW [IF EXISTS] [db_name.]view_name;
```

#### Example

```hive
DROP VIEW IF EXISTS sampleView;
```

#### Verification

```hive
SHOW TABLES;
```

## Index Operations

### Create Index

#### Syntax

```hive
CREATE INDEX index_name
ON TABLE base_table_name (col_name, ...)
AS index_type
[WITH DEFERRED REBUILD]
[IDXPROPERTIES (property_name=property_value, ...)]
[IN TABLE index_table_name]
[
    [ ROW FORMAT ...] STORED AS ...
    | STORED BY ...
]
[LOCATION hdfs_path]
[TBLPROPERTIES (...)]
[COMMENT "index comment"];
```

#### Example

```hive
CREATE INDEX sampleIndex
ON TABLE sampleTable(sampleName)
AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler';
```

#### Verification

```hive
SHOW INDEX ON sampleTable;
```

### Alter Index

#### Syntax

```hive
ALTER INDEX index_name ON table_name [PARTITION partition_spec] REBUILD;
```

#### Example

```hive
ALTER INDEX sampleIndex ON sampleTable PARTITION (sampleName = 'Chung', sampleInfo = 'Ky Son') REBUILD;
```

#### Verification

```hive
SHOW FORMATTED INDEX ON sampleTable;
```

### Drop Index

#### Syntax

```hive
DROP INDEX [IF EXISTS] index_name ON table_name;
```

#### Example

```hive
DROP INDEX IF EXISTS sampleIndex ON sampleTable;
```

#### Verification

```hive
SHOW INDEX ON sampleTable;
```

