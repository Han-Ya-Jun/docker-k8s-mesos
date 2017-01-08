# docker volume

### 验证docker volume
- install docker 1.12
- use docker volume shell，and return some docker volume info

### 创建docker volume
- 使用docker volume ls来查看volumes
- 使用docker volume create --name vol

### 使用docker volume
- 使用docker run --rm -v vol:/data alpine sh -C "echo hello > /data/volume.log"
- 使用docker run --rm -v vol:/data alpine sh -C “cat /data/volume.log”
- 说明volume是跨容器生命周期存储的，但是默认内置的是不可以集群跨节点访问

### volume跨节点访问
- flocker https://github.com/ClusterHQ/flocker
- 使用docker volume create --name vol1 --driver flocker来创建volume
- 可以在跨节点中通过docker volume ls查看到vol1的flocker volume

