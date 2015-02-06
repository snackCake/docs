# Zookeeper Setup

Zookeeper is a framework for managing clusters.  It provides mechaisms for failover and distributed file and state management.  Zookeeper is essential to the function of SolrCloud.  For more information on Zookeeper, see the documentation on the [Solr wiki](https://cwiki.apache.org/confluence/display/solr/SolrCloud+Configuration+and+Parameters) and on the [Zookeeper Site](http://zookeeper.apache.org/).

Download the appropriate [version of Zookeeper](https://cwiki.apache.org/confluence/display/solr/Setting+Up+an+External+ZooKeeper+Ensemble) for the version of SolrCloud that you plan to use.  For example, for Solr 4.10, the version of Zookeeper is 3.4.6.

This documentation assumes the use of Solr 4.10.3 and Zookeeper 3.4.6.  This documentation will also explain the set up the Zookeeper and Solr nodes on a single physical machine.  This can (and should) be done, in reality, on independent machines.  However, it is quite acceptable to put a Zookeeper instance on a machine with each Solr instance.  Here are the steps to get it set up with 3 Zookeeper instances:

- Download [Zookeeper](http://zookeeper.apache.org/releases.html) and unzip it to the local file system. We'll call this Zookeeper Home, or ZK_HOME.
- Under Zookeeper Home, create a new directory called "data"
- Under the "data" directory, create 3 new directories, called "1", "2", "3" respectively
- Under each of the data directories (e.g. 1, 2, and 3), create a new file called "myid" with no file extension.  The contents of the file should simply be the id of the instance, represented by the name of the directory that it is in (e.g. just the digit ```1```, ```2```, ```3``` respectively for each file).
- At this point, you should have a data directory under the Zookeeper Home directory, and in the data directory there should be 3 directories, each with a single file:
```
/$ZK_HOME/data/1/myid -- This file should contain nothing but the digit 1
/$ZK_HOME/data/2/myid -- This file should contain nothing but the digit 2
/$ZK_HOME/data/3/myid -- This file should contain nothing but the digit 3
```
- Under the ```conf``` directory, create a new file called zoo1.cfg:
```xml
tickTime=2000
initTime=10
initLimit=5
syncLimit=5
clientPort=2181
dataDir=/$ZK_HOME/data/1
server.1=localhost:2888:3888
server.2=localhost:2889:3889
server.3=localhost:2890:3890
```
- Under the ```conf``` directory, create a new file called zoo2.cfg. Notice that the ```clientPort``` and ```dataDir``` are different:
```xml
tickTime=2000
initTime=10
initLimit=5
syncLimit=5
clientPort=2182
dataDir=/$ZK_HOME/data/2
server.1=localhost:2888:3888
server.2=localhost:2889:3889
server.3=localhost:2890:3890
```
- Under the ```conf``` directory, create a new file called zoo3.cfg. Notice that the ```clientPort``` and ```dataDir``` are different:
```xml
tickTime=2000
initTime=10
initLimit=5
syncLimit=5
clientPort=2183
dataDir=/$ZK_HOME/data/3
server.1=localhost:2888:3888
server.2=localhost:2889:3889
server.3=localhost:2890:3890
```
- Now you can start each of the Zookeeper instances:
```xml
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo1.cfg -noprompt
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo2.cfg -noprompt
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo3.cfg -noprompt
```

You can also start them in the foreground using ```start-foreground``` instead of ```start```. If you start in the foreground, you'll see logs indicating that the instances have connected to one another and have elected a leader.

At this point, your Zookeeper Quarum is set up with 3 instances (or nodes) and is ready for Solr!


