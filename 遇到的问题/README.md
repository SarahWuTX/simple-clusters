# 遇到的问题

## 1.主namenode无法启动其他节点上的进程

当namenode执行start-all.sh时，无法启动其他节点上的进程，原因是几个节点的用户名、文件路径不一致

#### 解决方案

复制虚拟机，统一所有节点的文件路径、用户名

## 2.能启动zookeeper进程但状态出错

查看zookeeper状态时，显示"it's probably not running"

#### 解决方案

更改hadoop配置文件core-site.xml

```xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://ns</value>
    <!-- 原先是 hdfs://cMaster:9000 -->
</property>
```

## 3.无法远程连接hive（impersonate user）

> Error: Failed to open new session: 
>
> java.lang.RuntimeException: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.authorize.AuthorizationException): 
>
> User: lax is not allowed to impersonate root

由Hadoop2中的用户权限认证导致的

[官方文档](https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-common/Superusers.html)

> If more lax security is preferred, the wildcard value * may be used to allow impersonation from any host or of any user. For example, by specifying as below in core-site.xml, user named `oozie` accessing from any host can impersonate any user belonging to any group.

#### 解决方案

修改hadoop 配置文件 etc/hadoop/core-site.xml,加入如下配置项

设置 hadoop的代理用户，lyx是启动HiveServer2的用户

```xml
<property> 
    <name>hadoop.proxyuser.lyx.hosts</name> 
    <value>*</value>
</property> 
<property> 
    <name>hadoop.proxyuser.lyx.groups</name> 
    <value>*</value>
</property>
```

使用超级用户hadoop刷新配置（直接关闭并重启hadoop集群也可以）：

`yarn rmadmin -refreshSuperUserGroupsConfiguration`

`hdfs dfsadmin -refreshSuperUserGroupsConfiguration`

##### 注意

如果是对namenode做过HA，则需要在**主备namenode**上执行（其中ns是自己设置的NamenodeServiceID）：

hdfs dfsadmin -fs hdfs://ns -refreshSuperUserGroupsConfiguration

## 4.zkfc启动不成功

启动zkfc后，jps看不到ZKFailoverController进程，或是有两个standby的namenode，只能手动使namenode变成active状态

#### 解决方案

在core-site.xml中增加以下配置

```xml
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
```

