## <span id='top'>Issue</span>

Install Hadoop. 

[[Install Hadoop]](#install)  
[[Run `hadoop`]](#run)


<br>

## Rerequisites

- `openjdk-<version>-jdk` 
- `ssh`
- `pdsh`

<br>

### `jdk` - Install

    sudo apt install openjdk-<ver>-jdk


If you have it installed, check your version

1) `java --version`

        openjdk 11.0.13 2021-10-19
        OpenJDK Runtime Environment (build 11.0.13+8-Ubuntu-0ubuntu1.20.04)
        OpenJDK 64-Bit Server VM (build 11.0.13+8-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)

2) `ls urs/lib/jvm`

        default-java  java-1.11.0-openjdk-amd64  java-11-openjdk-amd64

<br>

### `jdk` - Edit `bashrc`

_.bashrc is a Bash shell script that Bash runs whenever it is started interactively. It initializes an interactive shell session. You can put any command in that file that you could type at the command prompt._ (Warren Young, StackExchange)


    nano ~/.bashrc

    export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64  <-------- Make sure that these line match your version.
    export PATH=$PATH:export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin
    export HADOOP_HOME=~/Downloads/hadoop-3.2.2/             <--------
    export PATH=$PATH:$HADOOP_HOME/bin
    export PATH=$PATH:$HADOOP_HOME/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export YARN_HOME=$HADOOP_HOME
    export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
    export
    HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.2.1.jar   <--------
    export HADOOP_LOG_DIR=$HADOOP_HOME/logs
    export PDSH_RCMD_TYPE=ssh
    export HIVE_HOME=~/Downloads/apache-hive-3.1.2-bin                                <--------
    export PATH=$PATH:~/Downloads/apache-hive-3.1.2-bin/bin                           <--------
    
<br>
<br>


### `ssh` - Install

    sudo apt-get install ssh    

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
        ncurses-term openssh-server openssh-sftp-server ssh-import-id
    Suggested packages:
        molly-guard monkeysphere ssh-askpass
    The following NEW packages will be installed
        ncurses-term openssh-server openssh-sftp-server ssh ssh-import-id
    0 to upgrade, 5 to newly install, 0 to remove and 17 not to upgrade.

<br>

### `pdsh` - Install

- A multithreaded remote shell client which executes commands on multiple remote hosts in parallel. 
- Used for multiple node clusters. 

        sudo apt install pdsh

        Reading package lists... Done
        Building dependency tree       
        Reading state information... Done
        The following additional packages will be installed:
            genders libgenders0
        Suggested packages:
            rdist
        The following NEW packages will be installed
            genders libgenders0 pdsh
        0 to upgrade, 3 to newly install, 0 to remove and 17 not to upgrade.

<br>
<br>

## <span id='install'>Install `Hadoop`</span>

[[Top]](#top)

Download `tar` file from https://dlcdn.apache.org/hadoop/common/hadoop-3.2.2/    

    tar -zxvf ~/Downloads/hadoop-3.2.2.tar.gz

<br>

The `gunzipped` hadoop directory may be found in `Home` or `Desktop` directory. Move that directory to the same directory where your `tar` file is located.

<br>

### Configure - 5 files 

Amongst the below files at the following location, we will set up a few things in different files. 

- 1 `hadoop-evn.sh`
- 2 `core-site.xml`
- 3 `sdfs-site.xml`
- 4 `mapred-site.xml`
- 5 `yarn-site.xml`

<br>

1. `hadoop-evn.sh`

edit the environment (`hadoop-evn.sh`) to configure the `java` location.

    cd ~/Downloads/hadoop-3.2.2/etc/hadoop/; ls

    capacity-scheduler.xml            httpfs-log4j.properties     mapred-site.xml
    configuration.xsl                 httpfs-signature.secret     shellprofile.d
    container-executor.cfg            httpfs-site.xml             ssl-client.xml.example
    core-site.xml                     kms-acls.xml                ssl-server.xml.example
    hadoop-env.cmd                    kms-env.sh                  user_ec_policies.xml.template
    hadoop-env.sh                     kms-log4j.properties        workers
    hadoop-metrics2.properties        kms-site.xml                yarn-env.cmd
    hadoop-policy.xml                 log4j.properties            yarn-env.sh
    hadoop-user-functions.sh.example  mapred-env.cmd              yarnservice-log4j.properties
    hdfs-site.xml                     mapred-env.sh               yarn-site.xml
    httpfs-env.sh                     mapred-queues.xml.template

<br>

Edit as follows: 

    nano hadoop-env.sh  
    
    /JAVA_HOME              <------ search for this string in vi

Then edit: 

    JAVA_HOME=/usr/java/java-1.11.0-openjdk-amd64   <------ Make sure you write your version here.

    :wq                     <------ Write and Quit vi.
    
2. `core-site.xml`

    nano core-site.xml

Paste the following at the configuration section: 

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
        </property>
        <property>
            <name>hadoop.proxyuser.dataflair.groups</name>
            <value>*</value>
        </property>
        <property>
            <name>hadoop.proxyuser.dataflair.hosts</name>
            <value>*</value>
        </property>
        <property>
            <name>hadoop.proxyuser.server.hosts</name>
            <value>*</value>
        </property>
        <property>
            <name>hadoop.proxyuser.server.groups</name>
            <value>*</value>
        </property>
    </configuration>

<br>

3. `hdfs-site.xml`

Edit as follows: 

    nano hdfs-site.xml

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
    </configuration>

<br>

4. `mapred-site.xml`

    Edit as follows: 

        <!-- Put site-specific property overrides in this file. -->

        <configuration>
            <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
            </property>
            <property>
            <name>mapreduce.application.classpath</name>
            <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
            </property>
        </configuration>

<br>

5. `mapred-site.xml`

Edit as follows: 

    nano yarn-site.xml


    <configuration>

    <!-- Site specific YARN configuration properties -->

        <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
        </property>
        <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREP
        END_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
        </property>

    </configuration>


<br>
 
## <span id="run">Run `hadoop`</span>

[[Top]](#top)

- 1 Establish a local connection.
- 2 Generate `ssh key`.
- 3 Grant permission.
- 4 Test-run `hadoop hdfs`.
- 5 Configure `PDSH_RCMD_TYPE`
- 6 Run `hadoop`

<br>

- If you want to stop the process.


<br>

**1) Establish a local connection.**

_`ssh localhost` means that a connection is conducted to the own machine, to the current user. In general, that means a new login shell with (perhaps) new privileges, when the list of groups the user is member in has changed._ (glglgl on StackExchange)


    ssh localhost


    Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.13.0-27-generic x86_64)

    * Documentation:  https://help.ubuntu.com
    * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage

    22 updates can be applied immediately.
    To see these additional updates run: apt list --upgradable

    Your Hardware Enablement Stack (HWE) is supported until April 2025.
    Last login: Tue Feb  1 02:43:12 2022 from 127.0.0.1

<br>

**2) Generate `ssh key`**

_ssh-keygen is a standard component of the Secure Shell protocol suite found on Unix, Unix-like and Microsoft Windows computer systems used to establish secure shell sessions between remote computers over insecure networks, through the use of various cryptographic techniques_ ([Wikipedia](https://en.wikipedia.org/wiki/Ssh-keygen)).


    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

    Generating public/private rsa key pair.
    Your identification has been saved in /home/coding-forest/.ssh/id_rsa
    Your public key has been saved in /home/coding-forest/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:17IiChOlFr/XLRMd......1QQ coding-forest@codingforest
    The key's randomart image is:
    +---[RSA 3072]----+
    |     =o.o++.  E. |
    |    S.   oo .   .|
    |  ...+  . .-   o |
    |   =. B ..XL. . .|
    |  + o+ BS.o+.+   |
    | . . .=..+.o+    |
    |  o + B = o      |
    |   + o . +       |
    |    .            |
    +----[SHA256]-----+

<br>

Copy your key to the `authorized_keys` file. 

    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

<br>

**3) Grant permission**

Then grant the permission to the file.

    chmod 0600 ~/.ssh/authorized_keys

<br>

**4) Test-run `hadoop hdfs`**

    ~/Downloads/hadoop-3.2.2/bin/hdfs namenode -format

    WARNING: /home/coding-forest/Downloads/hadoop-3.2.2//logs does not exist. Creating.
    2022-02-01 03:03:52,005 INFO namenode.NameNode: STARTUP_MSG: 
    /************************************************************
    STARTUP_MSG: Starting NameNode
    STARTUP_MSG:   host = codingforest/192.168.xx.xxx
    STARTUP_MSG:   args = [-format]
    STARTUP_MSG:   version = 3.2.2
    STARTUP_MSG:   classpath = 


**5) Configure `PDSH_RCMD_TYPE`**

Add `environmental variable` to `~/.bashrc` file. This modifies the default `pdsh` login authentication protocol to `ssh` (Dancing Developer on Tistory).

    export PDSH_RCMD_TYPE=ssh
    

<br>

**6) Run Hadoop**

    ~/Downloads/hadoop-3.2.2/sbin/start-dfs.sh

    Starting namenodes on [localhost]
    Starting datanodes
    Starting secondary namenodes [codingforest]


<br>
<br>

**If you want to stop the process**

    stop-all.sh


    WARNING: Stopping all Apache Hadoop daemons as coding-forest in 10 seconds.
    WARNING: Use CTRL-C to abort.
    Stopping namenodes on [localhost]
    Stopping datanodes
    Stopping secondary namenodes [codingforest]
    codingforest: Warning: Permanently added 'codingforest,192.168.xx.xxx' (ECDSA) to the list of known hosts.
    Stopping nodemanagers
    Stopping resourcemanager

<br>
<br>

### References 

- Dipanshu Modi (2020) Hadoop Step by Step easy Installation (in less than 10 minutes)on Ubuntu 20.04 LTS and Explanation. https://www.youtube.com/watch?v=KN6uzfQ42zE
- Dancing Developer https://log-laboratory.tistory.com/308