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
