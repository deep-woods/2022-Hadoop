
# <span id='top'>01 HDFS</span>

<br>

[[HDFS]](#HDFS)  
[[Data Blocks]](#datablocks)  
[[HDFS Write Mechanism]](#writemechanism)  
[[HDFS Read Mechanism]](#readmechanism)  
[[MapReduce]](#MapReduce)  
[[YARN Components]](#yarn)  
[[]](#)  
[[]](#)  
[[]](#)  

<br>


## <span id='HDFS'>HDFS</span>

[[Top]](#top)

<br>

**Hadoop Distributed File System**

<br>

`NameNode` <<  >> `Secondary NameNode`  
├── `DataNode`  
├── `DataNode`   ( ~~~ `Heartbeats` ) Slave daemons  
└── `DataNode`  
  
- `NameNode`
  - Maintains and manages `DataNodes`
  - Records metadata about data blocks - location of stored blocks, file sizes, permissions,4Receives heartbet and block report from all the `DataNodes`.

- `DataNode`
  - Slave daemons
  - Stores actual data
  - Serves read-write requests from clients

- `Secondary NameNode`


<br>
<br>

## <span id='datablocks'>Data Blocks</span>

[[Top]](#top)

<br>

### Fault tolerance

- Don't put all your eggs in one basket. 
- Each data blocks are replicated (thrice by default) and are distributed across different `DataNodes`.

<br>
<br>

## <span id='writemechanism'>HDFS Write Mechanism</span>

[[Top]](#top)

<br>

<br>
<br>

## <span id='readmechanism'>HDFS Read Mechanism</span>

[[Top]](#top)

<br>

<br>
<br>

## <span id='MapReduce'>MapReduce</span>

[[Top]](#top)

<br>

`MapReduce` is a programming framework that allows us to perform distributed and parallel processing on large data sets in a distributed environment.  

<br>

### `MapReduce` Word Count programme

`Word Count`
├── Mapper code mapper logic that produces the word-count key-value pairs. 
├── Reducer code: reducer logic that combines the intermediate key=value pairs 
└── Driver code: configuration (job name, input path, output path)

<br>

## <span id='yarn'>YARN Components</span>

[[Top]](#top)

<br>

Yet Another Resource Negotiator (MapReduce ver 2.)

<br>
<br>

## <span id=''></span>

[[Top]](#top)

<br>


<br>
<br>

## <span id=''></span>

[[Top]](#top)

<br>


<br>
<br>

## <span id=''></span>

[[Top]](#top)

<br>



<br>
<br>

## <span id=''></span>

[[Top]](#top)

<br>


<br>
<br>

## <span id=''></span>

[[Top]](#top)

<br>