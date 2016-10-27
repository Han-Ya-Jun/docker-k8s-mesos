安装flannel
yum install -y flannel

修改flannel的配置文件 /etc/sysconfig/flanneld
```
# Flanneld configuration options

# etcd url location.  Point this to the server where etcd runs
FLANNEL_ETCD="http://10.6.131.110:2379,http://10.6.131.114:2379,http://10.6.131.120:2379"

# etcd config key.  This is the configuration key that flannel queries
# For address range assignment
FLANNEL_ETCD_KEY="/k8s/network"

# Any additional options that you want to pass
#FLANNEL_OPTIONS=""
```
FLANNEL_ETCD :etcd的url，多个url使用”,”分隔。
FLANNEL_ETCD_KEY：flannel的数据在etcd存储的目录。

Flannel的配置信息全部在Etcd里面记录，往Etcd里面写入下面这个最简单的配置，指定Flannel能用来分配给每个Docker节点的拟IP地址段：172.19.0.0
etcdctl mk /k8s/network/config '{"Network": "172.19.0.0/16"}'

验证信息正确
etcdctl get /k8s/network/config 

如果docker服务已安装，需要先停止docker服务：
systemctl stop docker.service

启动Flannel服务
systemctl enable flanneld.service && systemctl start flanneld.service
