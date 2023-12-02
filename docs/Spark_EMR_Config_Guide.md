# Guide for Spark Configuration 
## Considering AWS EMR (Amazon Elastic MapReduce) Cluster Hardware Configuration 

#### To understand better lets take below example. 

Assume EMR Cluster has below hardware configuration 
- 20 Core Nodes and 1 Master Node
- Each Node is of EC2 instance type r5.8xlarge which has 32vCores and 256 Gib Memory

Lets use 30 vCores per node for executers and 254 Gib Mem 
Hence, to create 6 executers per node
we can allocate 5 (30/6) cores, 42.33 (254/6) GB Mem per executer 

```
Tota Executers in Cluster = Total Nodes * Executer per Node 
                          = 20 * 6 = 120
```

Default value for parallelism/ partitions can be calculated as below

```
Default parallelism = Tota Executers in Cluster * No of Cores per executer * 2 (Task per core) = 120 * 5 * 2 = 1200
```

#### Consider below spark memory configuration (Not optimal values):

- spark.memory.fraction = 0.80
- spark.memory.storageFraction = 0.30

Memory overhead is used when spark runs out of heap memory and have to data on disk space. Here we are considering 5 GB of standard memory overhead.

spark.executor.memory = Java Heap Memory per Executor which can be calculated as below. 

```
Heap memory per executer = Total executer memory - memory overhead
                         = 47.33 GB - 5 GB = 37.33 GB
```

Reserved memory is for storing spark objects and its hard coded in spark to 300 MB. 

```
Usable Heap Memory per executer = Total Heap Memory - Reserved Memory 
                                = 37.33 - 0.30 GB ~ 37 GB
```

User Memory is the memory used to store user-defined data structures, Spark internal metadata, any UDFs created by the user, the data needed for RDD conversion operations such as the information for RDD dependency information etc.

```
User Memory per executer = Usable Heap Memory * (1 - spark.memory.fraction)
                         = 37.33 GB * (1- 0.80) = 7.4 GB
```


Spark Memory is used for Execution & Storage  
```
Spark Memory per executer = Usable Heap Memory - User Memory 
                          = 37.33 GB - 7.4 GB = 29.6 GB
```

Memory left for execution 

```
Execution Memory per executor = Spark Memory * (1 - spark.memory.storageFraction)
                              = 29.6 * (1 - 0.30)
                              = 20.72 GB
```

Memory left for actual storage 

```
Storage Memory per executor = Spark Memory * (spark.memory.storageFraction)
                              = 29.6 * 0.30
                              = 8.88 GB
```










