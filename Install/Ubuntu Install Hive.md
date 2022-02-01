## <span id='top'>Issue</span>

Install Hadoop. 

[[Install Hive]](#install)  
[[Run `hadoop`]](#run)  
[[Troubleshooting 1]](#Troubleshooting1)
[[Troubleshooting 2]](#Troubleshooting2)

Make sure that you have `jdk 8` available on your machine. Higher version has some compatibility issues so likely `hive` might not run. 


<br>

## <span id='install'>Install `Hive`</span>

[[Top]](#top)

1. Download and unzip.
2. Install
3. Launch `Hive Metastore` and `Hive Database`.

<br>

### 1. Download and unzip

Download `tar` file from https://downloads.apache.org/hive/hive-3.1.2/

    wget https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz

    --2022-02-01 14:38:37--  https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
    Resolving downloads.apache.org (downloads.apache.org)... 88.99.95.219, 135.xxx.xxx.104, 2a01:4f8:10a:201a::2, ...
    Connecting to downloads.apache.org (downloads.apache.org)|88.99.95.219|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 278813748 (266M) [application/x-gzip]
    Saving to: ‘apache-hive-3.1.2-bin.tar.gz’

        apache-hive-3.1   100%[==================] 984.00K   545KB/s

<br>

    tar xzf apache-hive-3.1.2-bin.tar.gz

<br>

### Configure - 5 files

_The .bashrc file is a script file that’s executed when a user logs in. The file itself contains a series of configurations for the terminal session. This includes setting up or enabling: coloring, completion, shell history, command aliases, and more_ (JournalDev).

- 1 `.bashrc`
- 2 `hive-config.sh`
- 3 Create a directory `/tmp`
- 4 Create a directory `/user/hive/warehouse`
- 5 Launch `hive`

<br>

1. `.bashrc`

Move to `home` directory then: 

    nano .bashrc

    export HIVE_PATH=/home/coding-forest/Downloads/apache-hive-3.1.2-bin
    export PATH=$PATH:$HIVE_HOME/bin

    ^O^X


<br>
    
2. `hive-config.sh`

Tell `hive` where our `hadoop` is, because `hive` RUN ON `hadoop`.

    nano $HIVE_HOME/bin/hive-config.sh

Add the arrow-marked line in the following section of `hive-config.sh`

    # Default to use 256MB 
    export HADOOP_HEAPSIZE=${HADOOP_HEAPSIZE:-256}
    export HADOOP_HOME=/home/coding-forest/Downloads/hadoop-3.2.2

<br>

3. <span id='mkdir'>Create `/tmp` + change privilege. </span>
If trouble with connection, jump to [troubleshooting](#Troubleshooting).

- Purpose: every time we run a query, the temporary results of our queries are stored here.
- After creating the directory, change the permission so any user can read and write it. 

    hdfs dfs -mkdir /tmp   
    hdfs dfs -chmod g+w /tmp

<br>

4. Create a directory `/user/hive/warehouse` + change privilege.

- Very important: all `hive tables` will reside in this folder. 

    hdfs dfs -mkdir -p /user/hive/warehouse
    hdfs dfs -chmod g+w /user/hive/warehouse

- If we complete up to this stage, `hadoop` now knows there `hive` is. 
- `hdfs` also knows where to hide the `hive tables`.


<br>

5. <span id='launch'>Launch `hive`</span>

We have to tell hdfs: 

  - which datastore to use
  - which database to use

        schematool -initSchema -dbType derby

If you run into trouble, check <span id='Troubleshooting2'>this section below</span>.

<br>

### Start the db.

    schematool -initSchema -dbType derby


    Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
    Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
    Metastore connection User:	 APP
    Starting metastore schema initialization to 3.1.0
    Initialization script hive-schema-3.1.0.derby.sql

    Initialization script completed
    schemaTool completed

<br>

### Launch `hive`

    hive

    Logging initialized using configuration in jar:file:/home/coding-forest/Downloads/apache-hive-3.1.2-bin/lib/hive-common-3.1.2.jar!/hive-log4j2.properties Async: true



<br>
<br>
 
## <span id=""></span>

[[Top]](#top)


<br>

## <span id="Troubleshooting1">Troubleshooting</span>

[[Top]](#top)


Steps 

- 1. First, make `hadoop` up and running.
- 2. Successfully `mkdir /tmp`

<br>

1. Run `hadoop`

- **Summary**

  - `stop-all.sh`
  - `start-all.sh` 
  - `hdfs namenode`: start the server
  - `hive`

There is no hadoop server running lol. If you want to run `hive` which runs on `hadoop`, get `hadoop` up and running first. 

<br>

    hdfs dfs -mkdir /tmp

    mkdir: Call From codingforest/192.168.56.129 to localhost:9000 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused

`ConnectionRefused`: _You get a ConnectionRefused Exception when there is a machine at the address specified, but there is no program listening on the specific TCP port the client is using -and there is no firewall in the way silently dropping TCP connection requests._ (Confluence, 2019)

`dfs`: _A Distributed File System (DFS) as the name suggests, is a file system that is distributed on multiple file servers or multiple locations. It allows programs to access or store isolated files as they do with the local ones, allowing programmers to access files from any network or computer._ (GeeksforGeeks, 2021)

- `~/Downloads/hadoop-3.2.2/sbin/start-dfs.sh`: After the first run, you don't this this line. 
- `hdfs namenode`: run this instead.

    hadoop namenode -format     <---------------------------------- Starts Hadoop namenode. 
    
    WARNING: Use of this script to execute namenode is deprecated.
    WARNING: Attempting to execute replacement "hdfs namenode" instead.

<br>

2. Successfully `mkdir /tmp`

    hdfs dfs -mkdir /tmp 
    hdfs dfs -chmod g+w /tmp

Go back to the [hive installation process](#mkdir)

<br>

 
## <span id="Troubleshooting2"></span>

[[Top]](#top)


### Context

    Exception in thread "main" java.lang.NoSuchMethodError: 'void com.google.common.base.Preconditions.checkArgument(boolean, java.lang.String, java.lang.Object)'

### Solution

As we have installed both `hadoop` and `hive`, there are common jars that are present in both programmes. When we run `hive`, it gets confused as to which one to choose. We can resolve this issue by removing either of the duplicates and coping the remaining one to replace the removed one.

    ls $HIVE_HOME/lib

    ...
    groovy-all-2.4.11.jar                                  jetty-xml-9.3.20.v20170531.jar
    gson-2.2.4.jar                                         jline-2.12.jar
    guava-19.0.jar       <-----------this jar              joda-time-2.9.9.jar
    hbase-client-2.0.0-alpha4.jar                          joni-2.1.11.jar
    hbase-common-2.0.0-alpha                               ...
    ...

Remove one jar (from `hive`).

    rm $HIVE_HOME/lib/guava-19.0.jar

Copy the jar from `hadoop` to `hive`.
 
    cp $HADOOP_HOME/share/hadoop/hdfs/lib/guava-27.0-jre.jar $HIVE_HOME/lib/
    
    ...
    groovy-all-2.4.11.jar
    gson-2.2.4.jar
    guava-27.0-jre.jar              <--------This copied from hadoop to hive.
    hbase-client-2.0.0-alpha4.jar
    ...

    
Go back to the [hive launching process](#launch)

<br>
 

### References 

- Journaldev (n.d) What is .bashrc file in Linux? https://www.journaldev.com/41479/bashrc-file-in-linux#:~:text=The%20.,won't%20show%20the%20file.
- Confluence (2019) ConnectionRefused https://cwiki.apache.org/confluence/display/HADOOP2/ConnectionRefused
- Information Sciences Institute University of Southern California (1981) TRANSMISSION CONTROL PROTOCOL (TCP) https://www.ietf.org/rfc/rfc793.txt
- GeeksForGeeks (2021) What is DFS (Distributed File System)? https://www.geeksforgeeks.org/what-is-dfsdistributed-file-system/ 
- Commandstech (2019) How to resolve connection refused error while running Hive in Hadoop https://commandstech.com/how-to-resolve-connection-refused-error-while-running-hive-in-hadoop/