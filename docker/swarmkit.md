# swarmkit

### 准备swarmkit
- exec git clone https://github.com/docker/swarmkit to download the source
- exec the docker run to make the swarmkit:[docker run --rm -it -v `pwd`:/go/src/github.com/docker/swarmkit -w /go/src/github.com/docker/swarmkit golang:1.7 make binaries]
- copy run command to system path:[sudo cp swarmctl swarmd /usr/local/bin]
- exec swarmd --verison to verify

### 使用swarmkit
- exec shell:[]
