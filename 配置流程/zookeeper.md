# zookeeper

#### 1.下载并解压zookeeper

#### 2.修改配置文件zookeeper/conf/zoo.cfg

参见"配置文件"

#### 3.修改zookeeper/bin/zkEnv.sh

增加一句

```
JAVA_HOME="/home/lyx/lib/jvm/java"
```

#### 4.创建文件夹，分别是dataDir和dataLogDir的路径

zookeeper/data

zookeeper/logs

#### 5.创建myid文件

myid存放在dataDir文件夹中，myid的内容为一个数字。

每个主机的myid都不同，与zoo.cfg中的server.<myid>相对应。

如zoo.cfg中`server.1=cMaster:2888:3888`，则cMaster节点的myid是1。

#### 6.启动zookeeper

`zookeeper/bin/zkServer.sh start`

查看状态

`zookeeper/bin/zkServer.sh status`

