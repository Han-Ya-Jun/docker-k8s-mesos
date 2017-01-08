# docker swarm new way

### 验证docker swarm

- install docker 1.12
- type the docker swarm， and return docker swarm info

### 创建docker swarm
- 在任意节点执行docker swarm init，当前节点则为swam集群的manager管理节点
- 使用docker node ls来查看节点信息
- 使用docker swarm join的方式来是其余的节点加入到集群中
  通过docker swarm join --token ${token-string} ${manager-ip}:2377
- 使用docker swarm join-token manager来添加manager管理节点，通过这行命令能得到join的manager token
  通过docker swarm join --token ${manager-token-string} ${manager-ip}:2377

### docker swarm操作
- 使用docker node promote ${node-id}来升级节点为manager节点
- 使用docker node demote ${node-id}来降级节点
- 在节点上使用docker swarm leave来把自己从集群中移除
- 使用docker node rm ${node-id}直接移除节点
- 在节点上使用docker swarm leave --force来移除最后一个的manager节点

