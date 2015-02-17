# Zookeeper Setup

Zookeeper is a framework for managing clusters.  It provides mechaisms for failover and distributed file and state management.  Zookeeper is essential to the function of SolrCloud.  For more information on Zookeeper, see the documentation on the [Solr wiki](https://cwiki.apache.org/confluence/display/solr/SolrCloud+Configuration+and+Parameters) and on the [Zookeeper Site](http://zookeeper.apache.org/).

Download the appropriate [version of Zookeeper](https://cwiki.apache.org/confluence/display/solr/Setting+Up+an+External+ZooKeeper+Ensemble) for the version of SolrCloud that you plan to use.  For example, for Solr 4.10, the version of Zookeeper is 3.4.6.

This documentation assumes the use of Solr 4.10.3 and Zookeeper 3.4.6.  This documentation will also explain the set up the Zookeeper and Solr nodes on a single physical machine.  This can (and should) be done, in reality, on independent machines.  However, it is quite acceptable to put a Zookeeper instance on a machine with each Solr instance.  Here are the steps to get it set up with 3 Zookeeper instances:

## Linux Setup

- Download [Zookeeper](http://zookeeper.apache.org/releases.html) and unzip it to the local file system. We'll call this Zookeeper Home, or ZK_HOME.
- Under Zookeeper Home, create a new directory called "data"
- Under the "data" directory, create 3 new directories, called "1", "2", "3" respectively
- Under each of the data directories (e.g. 1, 2, and 3), create a new file called "myid" with no file extension.  The contents of the file should simply be the id of the instance, represented by the name of the directory that it is in (e.g. just the digit `1`, `2`, `3` respectively for each file).
- At this point, you should have a data directory under the Zookeeper Home directory, and in the data directory there should be 3 directories, each with a single file:

```
/$ZK_HOME/data/1/myid -- This file should contain nothing but the digit 1
```

```
/$ZK_HOME/data/2/myid -- This file should contain nothing but the digit 2
```

```
/$ZK_HOME/data/3/myid -- This file should contain nothing but the digit 3
```

- Under the `conf` directory, create a new file called zoo1.cfg:

```
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

- Under the `conf` directory, create a new file called zoo2.cfg. Notice that the `clientPort` and `dataDir` are different:

```
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

- Under the `conf` directory, create a new file called zoo3.cfg. Notice that the `clientPort` and `dataDir` are different:

```
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

```
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo1.cfg -noprompt
```

```
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo2.cfg -noprompt
```

```
./$ZK_HOME/bin/zkServer.sh start $ZK_HOME/conf/zoo3.cfg -noprompt
```

You can also start them in the foreground using `start-foreground` instead of `start`. If you start in the foreground, you'll see logs indicating that the instances have connected to one another and have elected a leader.  Note that there may be connectivity errors in the logs until more than one Zookeeper instance is running.  This is because the running instance(s) cannot connect to the others.

At this point, your Zookeeper Quarum is set up with 3 instances (or nodes) and is ready for Solr!

## Windows Setup

The above setup generally applies to Windows as well.  However, out of the box Windows requires a few things that don't port directly from the instructions above.  

1. First, all of the above references to `*.sh` files are for Linux and Unix flavors of operating systems.  Windows users should use `*.cmd` files instead.
2. The `zkServer.cmd` file does not properly allow you to pass in the `zoo*.cfg` location from the command line.  Rather it requires that you have a single `zoo.cfg` file located at `%ZK_HOME%\conf`.  As a result, in order to allow multiple Zookeeper instances to run on a single machine, you can unzip Zookeeper to multiple locations and create a separate file for each:
    - Unzip to a directory called zookeeper-1
    - Unzip to a directory called zookeeper-2
    - Unzip to a directory called zookeeper-3
    - Under each of these new Zookeeper directories, create a `data` directory
    - Under each of these new Zookeeper directories' respective `conf` directories, create a `zoo.cfg` (without a number in the file name) file as directed above
    - The data director in each of the respective Zookeeper directories should contain a `myid` file as described above (Note there is no need to put the `myid` files under a subdirectory like `1`, `2`, or `3` because they each live under their own Zookeeper data directory)
    - The `dataDir` property in each of the `zoo.cfg` files should reflect or reference the file location of their respective `data` directory for that zookeeper installation (e.g. `dataDir=/path/to/zookeeper-1/data`)
    - Start Zookeeper 1 : `C:\path\to\zookeeper-1\bin\zkServer.cmd` - (Note that this will use the `zoo.cfg` file under the `conf` directory for this this installation of Zookeeper)
    - Start Zookeeper 2 : `C:\path\to\zookeeper-2\bin\zkServer.cmd` - (Note that this will use the `zoo.cfg` file under the `conf` directory for this this installation of Zookeeper)
    - Start Zookeeper 3 : `C:\path\to\zookeeper-3\bin\zkServer.cmd` - (Note that this will use the `zoo.cfg` file under the `conf` directory for this this installation of Zookeeper)
