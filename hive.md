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

  * *Syntax:*

    ```hive
    ARRAY<data_type>
    ```

  * *Example:*

    ```hive
    ARRAY<STRING>
    ```

* Maps

  * *Syntax:*

    ```hive
    MAP<primitive_type, data_type>
    ```

  * *Example:*

    ```hive
    ARRAY<INT, STRING>
    ```

* Structs

  * *Syntax:*

    ```hive
    STRUCT<col_name : data_type [COMMENT col_comment], ...>
    ```

  * *Example:*

    ```hive
    STRUCT<col1 : INT COMMENT 'Column 1', col2 : STRING COMMENT 'Column 2'>
    ```

* Unions

  * *Syntax:*

    ```hive
    UNIONTYPE<data_type, data_type, ...>
    ```

  * *Example:*

    ```hive
    UNIONTYPE<INT, STRING>
    ```