
*** The below notes are adopted from IBM Cognitive Class Team (Spark Fundamentals I  Courese)
- The purpose of Spark
- Components of the Spark unified stack
- Basics of Resilient Distributed Dataset (RDD)
- Download and Install Spark standalone
- Overview of Scala and Python
- How to launch and use Spark’s Scala and Python shells


- Here we will explain how Spark can be used as an alternative to MapReduce. 
- how to program with Spark in different languages such as Python and Scala 
- how to work with different libraries and configurations. 

---------------------------------------------------------------------------

## Hadoop vs. Spark: What's the Difference?
#### The respective architectures of Hadoop and Spark, how these big data frameworks compare in multiple contexts and scenarios that fit best with each solution.

- Hadoop and Spark, both developed by the Apache Software Foundation, are widely used open-source frameworks for big data architectures. 
- Each framework contains an extensive ecosystem of open-source technologies that prepare, process, manage and analyze big data sets.
- Apache Hadoop is an open-source software utility that allows users to manage big data sets (from gigabytes to petabytes) by enabling a network of computers (or “nodes”) to solve vast and intricate data problems. 
- Hadoop is a highly scalable, cost-effective solution that stores and processes structured, semi-structured and unstructured data (e.g., Internet clickstream records, web server logs, IoT sensor data, etc.).
- Data protection amid a hardware failure
- Vast scalability from a single server to thousands of machines
- Real-time analytics for historical analyses and decision-making processes
- Apache Spark — which is also open source — is a data processing engine for big data sets. 
- Like Hadoop, Spark splits up large tasks across different nodes. 
- Spark tends to perform faster than Hadoop and it uses random access memory (RAM) to cache and process data instead of a file system. 
- This enables Spark to handle use cases that Hadoop cannot.
Benefits of Spark include:
Benefits of the Spark framework include the following:

- A unified engine that supports SQL queries, streaming data, machine learning (ML) and graph processing
- Can be 100x faster than Hadoop for smaller workloads via in-memory processing, disk data storage, etc.
- APIs designed for ease of use when manipulating semi-structured data and transforming data

The Hadoop ecosystem

Hadoop supports advanced analytics for stored data (e.g., predictive analysis, data mining, machine learning (ML), etc.). It enables big data analytics processing tasks to be split into smaller tasks. The small tasks are performed in parallel by using an algorithm (e.g., MapReduce), and are then distributed across a Hadoop cluster (i.e., nodes that perform parallel computations on big data sets).

### The Hadoop ecosystem consists of four primary modules:

Hadoop supports advanced analytics for stored data (e.g., predictive analysis, data mining, machine learning (ML), etc.). It enables big data analytics processing tasks to be split into smaller tasks. The small tasks are performed in parallel by using an algorithm (e.g., MapReduce), and are then distributed across a Hadoop cluster (i.e., nodes that perform parallel computations on big data sets).

1) Hadoop Distributed File System (HDFS): 
  - Primary data storage system that manages large data sets running on commodity hardware. 
  - It also provides high-throughput data access and high fault tolerance.
Yet Another Resource Negotiator (YARN): Cluster resource manager that schedules tasks and allocates resources (e.g., CPU and memory) to applications.
Hadoop MapReduce: Splits big data processing tasks into smaller ones, distributes the small tasks across different nodes, then runs each task.
Hadoop Common (Hadoop Core): Set of common libraries and utilities that the other three modules depend on.
