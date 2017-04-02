## install the zookeeper
- docker run -d --name zookeeper -p 2181:2181 jplock/zookeeper:3.4.9

## install the mesos
- wget https://repos.mesosphere.com/ubuntu/pool/main/m/mesos/mesos1.1.0-2.0.107.ubuntu1404_amd64.deb 
- sudo dpkg -i mesos1.1.0-2.0.107.ubuntu1404_amd64.deb  # install have some error about the mesos dependencies
- sudo apt-get -f install   # force to install the dependencies
- for centos exec the sudo yum --nogpgcheck localinstall mesos.rpm

## start the mesos master
- sudo mesos-master --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.235 --port=5050 [--advertise_ip=外部访问ip] --zk=zk://192.168.199.235:2181/mesos --quorum=1

## start the mesos slave
- sudo mesos-slave --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.236 --port=5051 [--advertise_ip=外部访问ip] --master=zk://192.168.199.235:2181/mesos --containerizers=docker,mesos --image_providers=docker --isolation=filesystem/linux,docker/runtime --no-switch_user
- sudo mesos-slave --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.237 --port=5051 [--advertise_ip=外部访问ip] --master=zk://192.168.199.235:2181/mesos --containerizers=docker,mesos --image_providers=docker --isolation=filesystem/linux,docker/runtime --no-switch_user

## use mesos-agent
- use mesos-agent to instead the mesos-slave

### use dcos to build the app group
- build the json file named cmd-demo-group.json
- use the command [dcos marathon group add cmd-demo-group.json] to build the app group
- use the command [dcos marathon group list] to show the apps
- use the command [dcos marathon app update cmd-demo-group/demo-2 cmd='sleep 2h'] to update the app demo-2
- build the json file named cmd-demo.json
- use the command  [curl -X POST -H "Content-type: application/json" -s -d @cmd-demo.json http://192.168.199.235:8080/v2/apps] to init the app
- use the command [curl  http://192.168.199.235:8080/v2/apps/cmd-demo] to get the info
- use the command [curl -X PUT -H "Content-type: application/json" -s -d @cmd-demo.json http://192.168.199.235:8080/v2/apps] to update the app

### use dcos to build app

- download the dcos cli [curl -O https://downloads.dcos.io/binaries/cli/linux/x86-64/dcos-1.8/dcos]
- add the execute authority [sudo chmod +x dcos]
- mv the dcos [sudo mv dcos /usr/local/bin]
- establish relationship for dcos and marathon [dcos config set marathon.url  http://192.168.199.235:8080]
- use the command [dcos marathon app add cmd-demo.json] to build the app
- use the command [dcos marathon app list] to show the apps
- use the command [dcos marathon app update cmd-demo cpus=0.5] to update the app

### use dcos to build the app group
- build the json file named cmd-demo-group.json
- use the command [dcos marathon group add cmd-demo-group.json] to build the app group
- use the command [dcos marathon group list] to show the apps
- use the command [dcos marathon app update cmd-demo-group/demo-2 cmd='sleep 2h'] to update the app demo-2

### mesos docker container

- build the json file named cmd-demo-docker.json
- use the command [dcos marathon app add cmd-demo-docker.json] to build the app

### mesos unified container
- build the json file named cmd-demo-uc.json
- use the command [dcos marathon app add cmd-demo-uc.json] to build the app

### marathon-lb
- use docker to start the marathon-lb [docker run --name='lb' -d -e PORTS=9090 --net=host --privileged mesosphere/marathon-lb [sse|poll] --marathon http://192.168.199.235:8080 --group mylb --health-check]
- build the build the json file named cmd-demo-lb.json
- use the command [dcos marathon app add cmd-demo-lb.json] to build the app
- use the command [curl -H 'Host: mylb.xiaobaxi.com' http://192.168.199.235] to see the python server page
- use the command [curl  http://192.168.199.235:31080] to see the python server page
- use the command [curl -vH "http:X-Marathon-App-Id: /cmd-demo-lb" 192.168.199.235:9091] to see the python server page

### mesos-DNS
- wget https://github.com/mesosphere/mesos-dns/releases/download/v0.6.0/mesos-dns-v0.6.0-linux-amd64
- add the execute authority [sudo chmod +x mesos-dns-v0.6.0-linux-amd64]
- mv to bin folder [sudo mv mesos-dns-v0.6.0-linux-amd64 /usr/local/bin/mesos-dns]
- build the json file named config.json
- start the mesos-dns [sudo mesos-dns -config=config.json &]
- [nslookup  cmd-demo-lb.marathon.mesos 192.168.199.235]  to see the DNS server instance
- in every slave add a new nameserver [sudo sed -i '1s/^/nameserver 192.168.199.235\n /' /etc/resolv.conf] to allow mesos tasks to use mesos-DNS as the primary DNS server


