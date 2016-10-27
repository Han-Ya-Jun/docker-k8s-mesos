|节点|操作系统|IP|
|-----|--------|------|
|etcd01|CentOS 7|10.6.131.110|
|etcd02|CentOS 7|10.6.131.114|
|etcd03|CentOS 7|10.6.131.120|

在3个节点上同时执行以下操作

[root@localhost ~]#yum install –y etcd


编辑/usr/lib/systemd/system/etcd.service文件修改etcd启动命令参数，将ExecStart这一段改为：
```
ExecStart=/bin/bash -c "GOMAXPROCS=$(nproc) /usr/bin/etcd --name=\"${ETCD_NAME}\" --data-dir=\"${ETCD_DATA_DIR}\" --listen-client-urls=\"${ETCD_LISTEN_CLIENT_URLS}\"   --listen-peer-urls=\"${ETCD_LISTEN_PEER_URLS}\" --advertise-client-urls=\"${ETCD_ADVERTISE_CLIENT_URLS}\" --initial-advertise-peer-urls=\"${ETCD_INITIAL_ADVERTISE_PEER_URLS}\" --initial-cluster-token=\"${ETCD_INITIAL_CLUSTER_TOKEN}\" --initial-cluster=\"${ETCD_INITIAL_CLUSTER}\" --initial-cluster-state=\"${ETCD_INITIAL_CLUSTER_STATE}\" "
```

分别编辑3个节点的/etc/etcd/etcd.conf文件，配置etcd参数（IP修改为各节点自己的IP）：
```
# [member]
ETCD_NAME=etcd01
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
#ETCD_WAL_DIR=""
#ETCD_SNAPSHOT_COUNT="10000"
#ETCD_HEARTBEAT_INTERVAL="100"
#ETCD_ELECTION_TIMEOUT="1000"
ETCD_LISTEN_PEER_URLS="http://0.0.0.0:2380"    #监听IP必须为“0.0.0.0”
ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379"  #监听IP必须为“0.0.0.0”
#ETCD_MAX_SNAPSHOTS="5"
#ETCD_MAX_WALS="5"
#ETCD_CORS=""
#
#[cluster]
ETCD_INITIAL_ADVERTISE_PEER_URLS="http://10.6.131.110:2380"
# if you use different ETCD_NAME (e.g. test), set ETCD_INITIAL_CLUSTER value for this name, i.e. "test=http://..."
ETCD_INITIAL_CLUSTER="etcd01=http://10.6.131.110:2380,etcd02=http://10.6.131.114:2380,etcd03=http://10.6.131.120:2380"
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_ADVERTISE_CLIENT_URLS="http://10.6.131.110:2379"
#ETCD_DISCOVERY=""
#ETCD_DISCOVERY_SRV=""
#ETCD_DISCOVERY_FALLBACK="proxy"
#ETCD_DISCOVERY_PROXY=""
#ETCD_STRICT_RECONFIG_CHECK="false"
...
```

启动etcd服务：

[root@localhost ~]# systemctl enable etcd.service && systemctl status etcd.service


使用etcdctl cluster-health命令可以查询etcd集群中节点的状态:
```
[root@localhost ~]#etcdctl cluster-health
member 2ca7ef4c803665ad is healthy: got healthy result from http://10.6.131.120:2379
member 788208dd0f0e9a3e is healthy: got healthy result from http://10.6.131.114:2379
member a5cb3f2ee2bbd3d2 is healthy: got healthy result from http://10.6.131.110:2379
cluster is healthy
```

增加节点
增加集群信息，并标示加入已存在的集群。
```
ETCD_INITIAL_CLUSTER="etcd01=http://10.6.131.110:2380,etcd02=http://10.6.131.114:2380,etcd03=http://10.6.131.120:2380,etcd04=http://10.6.131.121:2380"
ETCD_INITIAL_CLUSTER_STATE="existing"
```
