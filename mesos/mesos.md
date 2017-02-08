#mesos command

## install the zookeeper
- docker run -d --name zookeeper -p 2181:2181 jplock/zookeeper:3.4.9

## install the mesos
- wget https://repos.mesosphere.com/ubuntu/pool/main/m/mesos/mesos1.1.0-2.0.107.ubuntu1404_amd64.deb 
- sudo dpkg -i mesos1.1.0-2.0.107.ubuntu1404_amd64.deb  # install have some error about the mesos dependencies
- sudo apt-get -f install   # force to install the dependencies
- for centos exec the sudo yum --nogpgcheck localinstall mesos.rpm

## start the mesos
- sudo mesos-master --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.235 --port=5050 [--advertise_ip=外部访问ip] --zk=zk://192.168.199.235:2181/mesos --quorum=1
- sudo mesos-slave --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.236 --port=5051 [--advertise_ip=外部访问ip] --master=zk://192.168.199.235:2181/mesos --containerizers=docker,mesos --image_providers=docker --isolation=filesystem/linux,docker/runtime --no-switch_user
- sudo mesos-slave --work_dir=/var/lib/mesos --log_dir=/var/log/mesos --ip=192.168.199.237 --port=5051 [--advertise_ip=外部访问ip] --master=zk://192.168.199.235:2181/mesos --containerizers=docker,mesos --image_providers=docker --isolation=filesystem/linux,docker/runtime --no-switch_user


