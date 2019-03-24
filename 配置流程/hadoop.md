# hadoop

#### 1.下载并解压hadoop

#### 2.配置环境变量

```
export HADOOP_HOME=/usr/share/hadoop/hadoop-2.7.7
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

#### 3.修改$HADOOP_HOME/etc/hadoop中的配置文件

参见"配置文件"

#### 4.创建临时文件夹tmp

```
cd /home/lyx/app/hadoop
sudo mkdir tmp
sudo chmod -R 777 tmp
```

#### 5.修改hadoop-env.sh

```
sudo vim $HADOOP_HOME/etc/hadoop/vim hadoop-env.sh 
#将export JAVA_HOME=${JAVA_HOME} 修改成
export JAVA_HOME=/usr/local/java/jdk1.8.0_151
```

#### 6.启动hadoop

```
$HADOOP_HOME/sbin/start-all.sh
```

#### 7.查看进程

```
jps
```

