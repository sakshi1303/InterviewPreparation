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
