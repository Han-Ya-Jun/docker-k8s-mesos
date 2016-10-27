安装Node：
yum install -y kubernetes-node

编辑/etc/kubernetes/kubelet文件，配置node启动参数：
```
# kubernetes kubelet (minion) config

# The address for the info server to serve on (set to 0.0.0.0 or "" for all interfaces)
KUBELET_ADDRESS="--address=0.0.0.0"

# The port for the info server to serve on
# KUBELET_PORT="--port=10250"

# You may leave this blank to use the actual hostname
KUBELET_HOSTNAME="--hostname-override=10.6.131.110"

# location of the api-server
KUBELET_API_SERVER="--api-servers=http://10.6.131.110:8080"

# pod infrastructure container
KUBELET_POD_INFRA_CONTAINER="--pod-infra-container-image=gcr.io/google_containers/pause:2.0"

# Add your own!
KUBELET_ARGS=""
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

启动node服务，node包括kubelet、kube-proxy服务：
systemctl enable kubelet.service && systemctl start kubelet.service
systemctl enable kube-proxy.service && systemctl start kube-proxy.service

验证node安装成功,在master节点上运行：

kubectl get node
```
NAME           STATUS    AGE
10.6.131.110   Ready     11m
```
