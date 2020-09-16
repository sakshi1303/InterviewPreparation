## System and Architecture Overview

#### DataWarehouse System Architecture

1. Amazon Redshift is a relational datawarehouse system which supports integration with various applications like BI, Reporting data, Analytic tools, ETL tools etc.
2. It achieves efficient storage and optimum query performance.
3. Core infrastructure component of Redshift is a Cluster which consists of leader and compute nodes.
4. Leader nodes communicates with client tools and compute nodes. It parses and develops execution plan, compiles code, distributes them and portion of data to compute nodes.
5. Compute node has its own dedicated CPU, Memory and Disk Storage. It executes the compiled code and sends the result back to leader node for final aggregation.
6. Compute node is composed of one or more Node Slices which is allocated a portion of compute node's memory and disk space.
7. Compute nodes run on separate , isolated network which is never directly accessed by client applications.
8. Redshift is based on PostgreSQL and provides same functionalities as that of typical RDMS including OLTP systems but is highly optimized for large datasets.

#### Performance

1. Massively Parallel processing which enables fast execution of complex queries on large datasets. Records are distributed onto compute nodes using appropriate distribution key for parellel processing.
2. Columnar storage reduces overall disk I/O operations and reduces amount of data needed to load from disk hence improves query performance.
3. Data Compression reduces storage which reduces disk I/O hence improving query performance.
4. Query Optimizer significantly helps in improving performance of complex analytical queries.
5. Result caching helps achieving in getting results faster as the data gets cached on leader node.
6. Leader node compiles code and distributes across all compute nodes which reduces overhead of compiling code at compute nodes hence resulting in faster query execution.

#### Columnar Storage

1. Data block stores data column-wise instead of storing row-wise and each block same stores type of data which can use specific compression technique , and inturn improves the system perfomance.
2. It requires of a third of I/O operations compared to row-wise storage.
3. OLTP systems have less rows and columns while DWH have more rows and columns and very few columns are read in practical, hence column-wise storage is highly efficient and significantly improves query performance.

#### Workload Management

1. WLM helps in managing the priority of queries so that short/fast running query do not get stuck behind long running queries.
2. It create query queues at runtime which defines configuration for various types of queues.
2. Automatic WLM is the Default option and it manages query concurrency and memory allocation.

#### Using Amazon Redshift with other services

1. Moving data between Amazon Redshift and Amazon S3
2. Using Amazon Redshift with Amazon DynamoDB using COPY command
3. Importing data from remote hosts over SSH using COPY command
4. Automating data loads using AWS Data Pipeline
5. Migrating data using AWS Database Migration Service (AWS DMS)

## Best Practises for designing tables

#### Choose the best sort key

1. If recent data is queried most frequently, specify the timestamp column as the leading column for the sort key.
2. If you do frequent range filtering or equality filtering on one column, specify that column as the sort key.
3. If you frequently join a table, specify the join column as both the sort key and the distribution key.

#### Choose the best distribution style

1. Distribute the fact table and one dimension table on their common columns.
2. Choose the largest dimension based on the size of the filtered dataset.
3. Choose a column with high cardinality in the filtered result set.
4. Change some dimension tables to use ALL distribution.

#### Use Automatic Compression

1. Let COPY command choose compression encodings.
2. The COPY command analyzes your data and applies compression encodings to an empty table automatically as part of the load operation.
3. Run the ANALYZE COMPRESSION command on the table, then use the encodings to create a new table, but leave out the compression encoding for the sort key.

#### Define constraints

1. Define primary and foreign key constraints in tables whereever possible.
2. Do not define primary key and foreign key constraints unless your application enforces the constraints.
3. Amazon Redshift does not enforce unique, primary-key, and foreign-key constraints.

#### Use the smallest possible column size

1. Don't make it a practice to use the maximum column size for convenience.
2. Consider the largest values you are likely to store in a VARCHAR column, for example, and size your columns accordingly. 
3. There might be intermediate queries stored in temporary tables which are not compressed hence using large columns consumes excessive memory and temporary disk space, which can affect query performance.

#### Use date/time data types for date columns

1. Amazon Redshift stores DATE and TIMESTAMP data more efficiently than CHAR or VARCHAR, which results in better query performance. 
2. Use DATE/TIMESTAMP data type rather than a character type when storing date/time information wherever required.

## Best Practises for designing queries

1. Design tables according to best practices.
2. Avoid using SELECT * instead use specific columns as required.
3. Use CASE expressions to perform complex aggregations instead selecting from same table multiple times.
4. Never use CROSS-JOINS unless necessary as it makes query slower.
5. Use subqueries where the information is required for predicate conditions not projection and when the subquery returns very few rows less than 200.
6. Use predicates to restrict the dataset as much as possible.
7. Comparison operators are preferred over LIKE operators which is preferrable than SIMILAR TO/POSIX operators.
8. Avoid functions in query predicates.
9. Use WHERE clause to restrict the dataset wherever possible so that it skips scanning large data blocks.
10. Add predicates to filter tables that participate in joins even if resultset is same so that it skips scanning data blocks not required, the participating joining column is not required to be added as filter.
11. Use Sort Keys in GROUP BY clause for efficient aggregation.
12. If GROUP BY & ORDER BY clause both are used in a query, then order of columns in both the clause should be same.
