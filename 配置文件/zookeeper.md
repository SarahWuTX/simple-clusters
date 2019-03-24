# zookeeper

配置文件：zookeeper/conf/zoo.cfg

```
tickTime=2000
dataDir=/home/lyx/app/zookeeper/data
dataLogDir=/home/lyx/app/zookeeper/logs
clientPort=2181
initLimit=10
syncLimit=10
server.1=cMaster:2888:3888
server.2=cSlave0:2888:3888
server.3=cSlave1:2888:3888
server.4=cSlave2:2888:3888
server.5=cSlave3:2888:3888
```

