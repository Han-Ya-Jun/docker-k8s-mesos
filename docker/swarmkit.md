# swarmkit

### 准备swarmkit
- exec git clone https://github.com/docker/swarmkit to download the source
- exec the docker run to make the swarmkit:[docker run --rm -it -v `pwd`:/go/src/github.com/docker/swarmkit -w /go/src/github.com/docker/swarmkit golang:1.7 make binaries]
- copy run command to system path:[sudo cp swarmctl swarmd /usr/local/bin]
- exec swarmd --verison to verify

### 创建swarmkit集群
- exec shell to init first node:[swarmd -d /tmp/node-1 --listen-control-api /tmp/node-1/swarm.sock --hostname node-1]
- use SWARM_SOCKET=/tmp/node-1/swarm.sock to export the env on node-1,specified in --listen-control-api
- exec shell to get the Worker or Manager tokens [swarmctl cluster inspect default]
- use this shell [swarmd -d /tmp/node-2 --hostname node-2 --join-addr ${node-1-ip}:4242 --join-token <Worker Token>] to join cluster
- use this shell [swarmd -d /tmp/node-3 --hostname node-3 --join-addr ${node-1-ip}:4242 --join-token <Worker Token>] to join cluster
- use [swarmctl node ls] to see the cluster node

### 使用swarmkit
- use [swarmctl service create --name redis --image redis:3.0.5] to start redis service
- use [swarmctl service update redis --replicas 6] to update the redis node
- use [swarmctl service update redis --images redis:3.0.6 <--update-parallelism 2 --update-delay 10s>] to upgrade the redis service
- use [watch -n1 "swarmctl service inspect redis"] to watch the redis update
