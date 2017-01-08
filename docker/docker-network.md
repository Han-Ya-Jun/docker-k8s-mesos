# docker network

### 验证docker network
- install docker 1.12
- use docker network shell, and return the docker network info

### 创建docker network
- 使用docker network ls查看docker网络，默认会有bridge host null三种网络类型，bridge为网桥网络，传统的docker默认网络，与主机网络是隔离的，host为与主机共享网络，null为没有网络
- 使用docker network create n1 --driver bridge
- 使用docker network create n2 --driver bridge

### 使用docker network
- 使用docker run -dt --name c1 --network n1 alpine sh
- 使用docker run -dt --name c2 --network n2 alpine sh
- 使用docker run -dt --name c3 --network n1 alpine sh
- 运行docker exec c1 ping c2和docker exec c1 ping c3，c1跟c3是能ping通的
- exec shell: docker inspect -f '{{.NetworkSettings.Networks.n1.IPAddress}}' c1   then return 172.18.0.2
- exec shell: docker inspect -f '{{.NetworkSettings.Networks.n2.IPAddress}}' c2   then return 172.19.0.2
- exec shell：docker inspect -f '{{.NetworkSettings.Networks.n1.IPAddress}}' c3   then return 172.18.0.3
- yes you got this

### 网络跨节点
- 创建docker swarm集群
- 使用docker network create o1 --driver overlay 
- 使用docker service create --name whoami --replicas 3 --network o1 --publish 8000:8000 jwilder/whoami
- use shell: docker service ls查看具体的容器
- use shell: docker exec ${whoami-1-container} ping ${whoami-2-container}
