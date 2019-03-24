# hive

配置文件：$HIVE_HOME/conf

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
    <!-- hadoop -->
    <property>
        <!-- 存储在hdfs上的数据路径 -->
        <name>hive.metastore.warehouse.dir</name>
        <value>/usr/hive/warehouse</value>
    </property>
    
    <!-- metastore -->
    <property>
        <!-- hive的元数据库，这里用mysql -->
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://cMaster:3306/hive?useSSL=true</value>
        <description>JDBC connect string for a JDBC metastore</description>
    </property>
    <property>
        <!-- 连接元数据库的的驱动 -->
        <name>javax.jdo.option.ConnnectionDriverName</name>
    	<value>com.mysql.jdbc.Driver</value>
        <description>Driver class name for a JDBC metastore</description>
    </property>
    <property>
        <!-- 元数据库的密码 -->
        <name>javax.jdo.option.ConnectionPassword</name>
    	<value>123456</value>
    </property>
    <property>
        <!-- 元数据库的用户 -->
        <name>javax.jdo.option.ConnectionUserName</name>
		<value>root</value>
        <description>Username to use against metastore database</description>
    </property>
    
    <!-- HiveServer2的高可用配置 zookeeper -->
    <property>
        <name>hive.server2.support.dynamic.service.discovery</name>
        <value>true</value>
    </property>
    <property>
        <!-- 指定zookeeper中的命名空间 -->
        <name>hive.server2.zookeeper.namespace</name>
        <value>hiverserver2_zk</value>
    </property>
    <property>
        <!-- 监听的地址 -->
        <name>hive.server2.thrift.bind.host</name>
        <value>0.0.0.0</value>
    </property>
    <property>
        <!-- 监听的端口号 -->
        <name>hive.server2.thrift.port</name>
        <value>10001</value> <!-- 两个HiveServer2实例的端口号要一致 -->
    </property>
    <property>
        <!-- Zookeeper的集群连接串 -->
        <name>hive.zookeeper.quorum</name>
        <value>cMaster:2181,cSlave0:2181,cSlave1:2181,cSlave2:2181,cSlave3:2181</value>
    </property>
    <property>
        <name>hive.zookeeper.client.port</name>
        <value>2181</value>
    </property>
    
</configuration>
```

