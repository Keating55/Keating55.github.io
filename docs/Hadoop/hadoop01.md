#  hadoop伪分布配置

## 安装jdk，hadoop

`tar -zxvf ****.tar.gz -C /opt`

## 环境变量

`vi /etc/profile`

```
#JAVA
export JAVA_HOME=/opt/jdk1.8.0_271
export PATH=$PATH:$JAVA_HOME/bin
#HADOOP
export HADOOP_HOME=/opt/hadoop-2.7.6
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

刷新环境变量`source /etc/profile`

## 修改主机名

方法一

修改主机名为`hadoop101`

```
hostnamectl set-hostname hadoop101
hostnamectl set-hostname hadoop102
hostnamectl set-hostname hadoop103
```
重启`reboot`

## 修改hosts

`vi /etc/hosts`

```
192.168.87.101 hadoop101
192.168.87.102 hadoop102
192.168.87.103 hadoop103
```

## 关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
#[可选]关掉seliunx
vi /etc/selinux/config
.....
SELINUx=disable
.....
```

## 配置ssh

`ssh-keygen -t rsa`，回车

分发本机用户`ssh-copy-id root@hadoop101`

将公钥分发给各节点

```
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
scp /root/.ssh/id_rsa.pub root@node2:/root/.ssh/authorized_keys
scp /root/.ssh/id rsa.pub root@node3:/root/.ssh/authorized_keys
```

给所有的虚拟机添加权限

```
chmod 644 /root/.ssh/authorized_keys
```

## hadoop

## 配置hadoop-env.sh

`vi $HADOOP_HOME/etc/hadoop/hadoop-env.sh`

```
export JAVA_HOME=/app/jdk1.8.0_271
```



## 配置core-site.xml

`vi $HADOOP_HOME/etc/hadoop/core-site.sh`

```xml
<property>
	<name>fs.defaultFS</name>
	<value>hdfs://node01:9000/</value>
</property>
<!-- hadoop缓存目录 --> 
<property>
	<name>hadoop.tmp.dir</name>
	<value>/$HADOOP_HOME/tmp/</value>
</property>
```

## 配置hdfs-site.xml

`vi $HADOOP_HOME/etc/hadoop/hdfs-site.sh`

```xml
<property>
	<name>dfs.replication</name>
	<value>1</value>
</property>
```


```
1810 NodeManager
1703 ResourceManager
1547 SecondaryNameNode
1261 NameNode
1358 DataNode
2078 Jps
```



