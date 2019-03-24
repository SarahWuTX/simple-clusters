# hadoop

配置文件所在文件夹：$HADOOP_HOME/etc/hadoop

### slaves

```
cMaster
cSlave0
cSlave1
cSlave2
cSlave3
cSlave4
```

### core-site.xml

```xml
<configuration>
   	<property>
        <!-- Abase for other temporary directories -->
    	<name>hadoop.tmp.dir</name>
     	<value>file:/home/lyx/app/hadoop/tmp</value>
    </property>
    <property>
    	<name>fs.defaultFS</name>
     	<value>hdfs://ns</value>
        <!-- by default: hdfs://localhost:9000 -->
   	</property>
    
    <!-- namenode高可用配置 -->
    <property>
        <!-- ZooKeeper服务器的地址，ZKFC将使用该地址，在HA中需要以NameServiceID为后缀（.ns）-->
        <name>ha.zookeeper.quorum.ns</name>
        <value>cMaster:2181,cSlave0:2181,cSlave1:2181,cSlave2:2181,cSlave3:2181</value>
  	</property>
    
    <!-- 为hive远程连接而做的配置，lyx是启动HiveServer2的用户 -->
  	<property>
      	<name>hadoop.proxyuser.lyx.hosts</name>
      	<value>*</value>
  	</property>
  	<property>
      	<name>hadoop.proxyuser.lyx.groups</name>
      	<value>*</value>
  	</property>
</configuration>
```

### hdfs-site.xml

[参数说明](https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)

```xml
<configuration>
	<property>
    	<name>dfs.replication</name>
        <value>3</value>
	</property>
	<property>
        <!-- namenode store the name table(fsimage) on the local filesystem at -->
    	<name>dfs.namenode.name.dir</name>
    	<value>file:/home/lyx/app/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <!-- datanode store its blocks on the local filesystem at -->
    	<name>dfs.datanode.data.dir</name>
    	<value>file:/home/lyx/app/hadoop/tmp/dfs/data</value>
	</property>
    
    <property>
        <!--指定hdfs的nameservice为ns，需要和core-site.xml中的保持一致 -->
        <name>dfs.nameservices</name>
        <value>ns</value>
    </property>
    <property>
        <!-- ns下面有两个NameNode，分别是nn1，nn2 -->
       	<name>dfs.ha.namenodes.ns</name>
       	<value>nn1,nn2</value>
    </property>
    <property>
        <!-- nn1的RPC通信地址 -->
      	<name>dfs.namenode.rpc-address.ns.nn1</name>
       	<value>cMaster:9000</value>
    </property>
    <property>
        <!-- nn1的http通信地址 -->
        <name>dfs.namenode.http-address.ns.nn1</name>
        <value>cMaster:50070</value>
    </property>
    <property>
        <!-- nn2的RPC通信地址 -->
        <name>dfs.namenode.rpc-address.ns.nn2</name>
        <value>cSlave0:9000</value>
    </property>
    <property>
        <!-- nn2的http通信地址 -->
        <name>dfs.namenode.http-address.ns.nn2</name>
        <value>cSlave0:50070</value>
    </property>
    <property>
        <!-- 指定NameNode的元数据在JournalNode上的存放位置 -->
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://cSlave1:8485;cSlave2:8485;cSlave3:8485/ns</value>
    </property>
    <property>
        <!-- 指定JournalNode在本地磁盘存放数据的位置 -->
        <name>dfs.journalnode.edits.dir</name>
        <value>/home/lyx/app/hadoop/journal</value>
    </property>
    <property>
        <!-- 开启NameNode故障时自动切换，在HA中需要以NameServiceID为后缀（.ns） -->
        <name>dfs.ha.automatic-failover.enabled.ns</name>
        <value>true</value>
    </property>
    <property>
        <!-- 配置失败自动切换实现方式 -->
        <name>dfs.client.failover.proxy.provider.ns</name>
        <value>
            org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider
        </value>
    </property>
   	<!-- 配置隔离机制 -->
	<property>
        <!-- 配置隔离机制方法，多个机制用换行分割，即每个机制占用一行-->
		<name>dfs.ha.fencing.methods</name>
		<value>
			sshfence
			shell(/bin/true)
		</value>
	</property>
	<property>
        <!-- 使用sshfence隔离机制时需要ssh免登陆 -->
		<name>dfs.ha.fencing.ssh.private-key-files</name>
		<value>/home/admin1/.ssh/id_rsa</value>
	</property>
    <property>
        <!-- 在namenode和datanode上开启WebHDFS(REST API)功能 -->
    	<name>dfs.webhdfs.enabled</name>
    	<value>true</value>
    </property>
</configuration>
```

### mapred-site.xml

使用hadoop的（map-reduce）框架yarn

```xml
<configuration>
	<property>
    	<name>mapreduce.framework.name</name>
    	<value>yarn</value>
    </property>
   	<property>
        <name>mapreduce.jobhistory.address</name>
        <value>cMaster:10020</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>cMaster:19888</value>
    </property>
</configuration>
```

### yarn-site.xml

[yarn-site.xml 配置说明](https://www.jianshu.com/p/35374384a1aa)

[参数说明](https://blog.csdn.net/galaxy__fish/article/details/76943944)

[MapR 5.2 Documentation](https://mapr.com/docs/52/ReferenceGuide/yarn-site.xml.html)

```xml
<configuration>
<!-- Site specific YARN configuration properties -->
	<property>
        <!-- 指定resourcemanager -->
		<name>yarn.resourcemanager.hostname</name>
		<value>cMaster</value>
	</property>
	<property>
        <!-- 指定nodemanager启动时加载server的方式为shuffle server -->
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
	</property>
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <property>
        <name>yarn.log.server.url</name>
        <value>http://cMaster:19888/jobhistory/logs</value>
    </property>
    <property>
    <name>yarn.nodemanager.pmem-check-enabled</name>
    <value>false</value>
    </property>
    <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>false</value>
    </property>
</configuration>
```

