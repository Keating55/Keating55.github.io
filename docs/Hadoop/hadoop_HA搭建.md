# 高可用性(HA)Hadoop搭建
```bash
# 开启所有journalnode
hadoop-daemon.sh start journalnode

# 格式化一个namenode
hdfs namenode -format

# 开启namenode
hadoop-daemon.sh start namenode

# 在另一台namenode拉取镜像
hdfs namenode -bootstrapStandby

# 关闭开启的namenode
hadoop-daemon.sh stop namenode

#关闭所有journalnode
hadoop-daemon.sh stop journalnode

# 开启所有的QuorumPeerMain
zkServer.sh start
zkServer.sh stop

# 查看zkServer状态
zkServer.sh status

# 选择一个namenode格式化zkfc
hdfs zkfc -formatZK

#启动集群
start-dfs.sh
#拉取namenode

# 查看namenode状态
hdfs haadmin -getServiceState nn1
hdfs haadmin -getServiceState nn2

# 开HA集群时，要先开zookeeper服务，再开HDFS

yum -y install psmisc

#强行切换状态
hdfs haadmin -transitionToActive -forcemanual namenode11
hdfs haadmin -transitionToStandby -forcemanual namenode11
# 或关闭开启的namenode测试
hadoop-daemon.sh stop namenode

```

