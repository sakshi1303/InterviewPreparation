## Hadoop

1. Hadoop - based on Distributed Systems which is internally based on MapReduce concept.
2. It has one master node and rest are slave nodes. These are commodity machines not SSD's.
3. It has 3 components -

> - HDFS  -> Storage 
> - YARN  -> Resource Management
> - MapReduce -> Processing

4. Master Machine will run a process called NameNode and Slave machines will run process called DataNode.
5. In HDFS, modification of data/any specific record is not possible. Store and read the complete file or delete it.
6. Append operation is possible as we append in the end but editing a record in between is not possible.
7. Hadoop is for analysis not for transactional data.
8. Client machine connects with Hadoop cluster via Gateway machine with the help of username & password.
9. Hadoop cluster like any FileSystem has default Block Size of 128MB which can be changed.
10. DataNode sends heartbeat to the NameNode telling that they are alive.
11. Data gets replicated to other DataNodes so that if one system crashes, data can be recovered from others(Replication Factor).
12. Purpose of NameNode is to store metadata about data stored in different DataNodes.
13. If NameNode(Active Node) crashes, cluster cannot be accessed, hence, it has a backup node (Passive Node) for recovery.
14. Tool called DistCP (Distributed Copy) is used to synchronize the two clusters (Primary & Secondary).

## Hive

1. Hive provides SQL-like interface to Hadoop. Hive doesnot have its own storage and uses Hadoop's Storage.
2. It is Hadoop datawarehouse system. Data is stored in HDFS.
3. Top-level Apache project.
4. Hive internally writes a MapReduce Program for Hadoop to understand.
5. It is batch processing system and not suitable for OLTP system.
6. Hive language is called Hive Query Language (HQL).
7. Hive MetaStore should run for Hive to communicate and stores metadata.
8. Hive & Spark talk to same metastore hence a Hive Table can be read by Spark.
9. Cost Based Optimizer
10. It supports Data definition Language, Data Manipulation Language, and user-defined functions.

## Spark

1. Spark is independent and mostly runs on top of Hadoop.
2. Problem with Hadoop - MapReduce is slow and does batch-processing.
3. General Unified Engine
4. Faster as compared with other tools.
5. Ease of programming. For example - PySpark.
6. MapReduce , Pig, Storm (Real time Streaming) - Obselete
7. Supports Micro - Batching and Streaming 
8. Scheduling, Monitoring and Distribution
9. Languages supported - Scala, Python, Java, R
10. SparkSQL is integrated with Hive.
11. Neo4j - NOSQL database stores in graph format
12. Spark has no storage as it is an execution engine like MapReduce.
13. It gets data from Hadoop if we run Spark from Hadoop.
14. Spark also runs on Kubernetes.
15. Docker is a container which can be installed in our local and uses OS and other processes from machine itself unlike a virtual machine which has its own OS, memory etc.
16. Zookeeper is a service that Sparks supports which keeps information about Primary & Secondary Nodes and its coordinates health status of nodes(Active/Inactive).
17. Spark supports multiple FileSystem like Local, Amazon S3, NOSQL, RDBMS DB etc. 
18. Spark streaming supports Flume & Kafka.
19. Kafka is message queue and Flume is point to point delivery of data and these services provide backup of data.
