# docker swarm old way

### 验证docker
- install the docker

### 创建consul
- 使用docker run -d -p "8400:8400" -p "8500:8500" -v /var/consul/data:/data --name consul --restart=always \
  consul-server:0.6.3 -bootstrap   -data-dir=/data

### 创建swarm manage
- 使用docker run -d -p 3376:2375 --name swarm-master --restart=always swarm manage consul://${ip}:8500

### 创建swarm node
- 使用docker run -d --name swarm-node --restart=always swarm join --addr=${ip}:2375 consul://${manage-ip}:8500

### 验证swarm集群
- 使用docker -H ${manage-ip}:3376 info
- 配置export DOCKER_HOST="docker -H ${manage-ip}:3376"，and then use docker info
