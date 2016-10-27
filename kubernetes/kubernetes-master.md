安装Master：
yum install -y kubernetes-master

编辑/etc/kubernetes/apiserver文件，配置master参数：
```
# The address on the local server to listen to.
KUBE_API_ADDRESS="--insecure-bind-address=0.0.0.0"

# The port on the local server to listen on.
# KUBE_API_PORT="--port=8080"                          

# Port minions listen on
# KUBELET_PORT="--kubelet-port=10250"

# Comma separated list of nodes in the etcd cluster
KUBE_ETCD_SERVERS="--etcd-servers=http://10.6.131.110:2379,http://10.6.131.114:2379,http:/
/10.6.131.120:2379"                                    
# Address range to use for services
KUBE_SERVICE_ADDRESSES="--service-cluster-ip-range=172.19.0.0/16"     

# default admission control policies
KUBE_ADMISSION_CONTROL="--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger
,SecurityContextDeny,ResourceQuota"

# Add your own!
KUBE_API_ARGS=""
```

编辑/etc/kubernetes/config文件，配置master地址：
```
# logging to stderr means we get it in the systemd journal
KUBE_LOGTOSTDERR="--logtostderr=true"

# journal message level, 0 is debug
KUBE_LOG_LEVEL="--v=0"

# Should this cluster be allowed to run privileged docker containers
KUBE_ALLOW_PRIV="--allow-privileged=false"

# How the controller-manager, scheduler, and proxy find the apiserver
KUBE_MASTER="--master=http://10.6.131.110:8080"
```

启动master服务,master包括kube-apiserver、 kube-controller-manager、kube-scheduler服务：
systemctl enable kube-apiserver && systemctl start kube-apiserver
systemctl enable kube-controller-manager && systemctl start kube-controller-manager
systemctl enable kube-scheduler && systemctl start kube-scheduler


验证Master启动成功：
curl http://10.6.131.110:8080/healthz
