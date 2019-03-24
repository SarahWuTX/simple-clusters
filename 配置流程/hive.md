# hive

#### 1.启动hadoop

`$HADOOP_HOME/sbin/start-all.sh`

#### 2.创建hdfs

`$HADOOP_HOME/bin/hdfs namenode -format`

#### 3.创建hdfs中的目录给hive使用

`cd $HADOOP_HOME/bin`

```
hadoop fs -mkdir		/tmp
hadoop fs -mkdir -p    	/usr/hive/warehouse
hadoop fs -chmod g+w   	/tmp
hadoop fs -chmod g+w   	/usr/hive/warehouse
```

#### 4.下载并解压hive

#### 5.配置环境变量

```
export HIVE_HOME=/home/lyx/app/hive
export PATH=$PATH:$HIVE_HOME/bin
```

#### 6.下载mysql-connector-java，移到$HIVE_HOME/lib/中

#### 7.配置mysql

`mysql mysql -u root -p` 

```
create database hive; 
grant all privileges on hive.* to root@localhost identified by '设置密码' with grant option; 
flush privileges; 
exit;
```

#### 8.修改hive的配置文件$HIVE_HOME/conf

参见"配置文件"

#### 9.初始化hive的数据库

```
cd $HIVE_HOME/bin
schematool  -dbType mysql -initSchema
schematool  -dbType mysql -info
```

#### 10.启动hive

```
hive --service metastore
```

