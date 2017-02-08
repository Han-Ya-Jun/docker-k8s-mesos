# use docker to init kubernetes

## init etcd single
- exec the command below
```
  docker run -d -p 2379:2379 -p 4001:4001 --restart=always --name etcd etcd:2.2.5 \
    --name etcd0 \
    --advertise-client-urls http://k8s01:2379 \
    --listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
    --initial-advertise-peer-urls http://k8s01:2380 \
    --listen-peer-urls http://0.0.0.0:2380 \
    --initial-cluster-token etcd-cluster-1 \
    --initial-cluster etcd0=http://k8s01:2380 \
    --initial-cluster-state new \
    --force-new-cluster
```

## set the flannel
- exec the command below
```
etcdctl set /coreos.com/network/config '{ "Network": "172.18.0.0/16" }'
sudo flanneld --etcd-endpoints="http://k8s01:2379" --iface="eth0" --ip-masq=true >/tmp/flanneld.log 2>&1 &
```

## set docker use flannel connect
```
sudo mk-docker-opts.sh -d /run/flannel_docker_opts.env -i
source /run/flannel/subnet.env
sudo ifconfig docker0 ${FLANNEL_SUBNET}
sudo rm /var/run/docker.pid
sudo systemctl restart docker
```

## start the k8s master node
- exec the command below to start the apiserver
```
kube-apiserver \
   --logtostderr=true \
   --v=2 \
   --allow-privileged=false \
   --etcd-servers=http://127.0.0.1:2379 \
   --insecure-bind-address=0.0.0.0 \
   --insecure-port=8080 \
   --service-cluster-ip-range=10.100.0.0/16 \
   --admission-control=NamespaceAutoProvision,LimitRanger,SecurityContextDeny \
   --secure-port=6443 \
   --client_ca_file=${HOME}/cert/ca.crt \
   --tls-private-key-file=${HOME}/cert/server.key \
   --tls-cert-file=${HOME}/cert/server.crt \
   --cors-allowed-origins='.*' \
   > /tmp/kube-apiserver.log 2>&1 &
```  

- exec the command below to start the controller-manager
```  
  kube-controller-manager \
   --logtostderr=true \
   --v=2 \
   --master=https://k8s01:6443 \
   --service-account-private-key-file=${HOME}/cert/server.key \
   --root-ca-file=${HOME}/cert/ca.crt \
   --kubeconfig=${HOME}/.kube/config \
   > /tmp/kube-controller-manager.log 2>&1 &
```
- exec the command below to start the scheduler
```  
  kube-scheduler \
   --logtostderr=true \
   --v=2 \
   --master=https://k8s01:6443 \
   --kubeconfig=${HOME}/.kube/config \
   >/tmp/kube-scheduler.log 2>&1 &
```

## start the k8s node node 
- exec the command to start the kube-proxy
```
  sudo kube-proxy \
   --logtostderr=true \
   --v=2 \
   --master=https://k8s01:6443 \
   --kubeconfig=${HOME}/.kube/config \
   --hostname-override=$(hostname) \
   --proxy-mode=iptables \
   > /tmp/kube-proxy.log 2>&1 &
```
- exec the command to start the kubelet
```
sudo kubelet \
    --logtostderr=true \
    --v=2 \
    --api-servers=https://k8s01:6443 \
    --address=0.0.0.0 \
    --port=10250 \
    --hostname-override=$(hostname) \
    --allow-privileged=false \
    --kubeconfig=${HOME}/.kube/config \
    --cluster-dns=10.100.0.10 \
    --cluster-domain=cluster.local \
    > /tmp/kubelet.log 2>&1 &
```
## start the dns 
- kubectl create -f skydns/skydns-rc.yaml

- kubectl create -f skydns/skydns-svc.yaml


## start the dashboard
- kubectl create -f ui/dashboard-rc.yaml

- kubectl create -f ui/dashboard-svc.yaml

