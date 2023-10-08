# Hadoop

It's a scalable fault tolerant distributed system for Big Data.
It has 2 components: Distributed database for big data and map reduce paradigm.

Hadoop is designed for **data intensive workloads**, but for bigger amount of data and you need calculations you have to use **HPC High performance computer**

### Main Components
There are 2 base components: the first one is distributed big data processing infrastructure based on MapReduce paradigm, and the second is a **HDFS** (Hadoop distributed file system).
Both of this two components are fault-tolerant.

![[Pasted image 20231003165740.png]]

Each disk has a specific number of chunks (same number for all server). Data are spread around all server so we can recover data (usually 3 copies for chunk).
The combination of all disks is the HDFS.
Data inside HDFS are rarely updated, but reads and appends operations are common.
Each chunks/block contains a part of content of one single file.
You cannot have content from two different files inside the same blocks.

```ad-important
If we have to store many lightweight files Hadoop is not the solution, because each file is stored a block, HUGE WASTE OF MEMORY.
(1000 files of 10 KB means 1000 disks)
```

```ad-note
If there's a failure in a disk, the HDFS will immediatly create a copy of the chunk inside that broken disk, and put that copy in a different server 
```

Due to this infrastructure, when we program we tell servers just **what** we want to do, not how, because the management of the system is Hadoop's goal. We do not care about synchronization. 

Another main component is the **Master node**: it's a special node that stores HDFD metadata, like the mapping between the name of a file and its location.

![[Pasted image 20231003171820.png]]
