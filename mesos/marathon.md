# marathon install

### download the marathon
- wget http://downloads.mesosphere.com/marathon/v1.4.1/marathon-1.4.1.tgz
- tar xzf marathon-1.4.1.tgz
- cd marathon-1.4.1

### start the marathon
- ./bin/start --master zk://192.168.199.235:2181/mesos --zk zk://192.168.199.235:2181/marathon
- also start the marathon on the other two server
- you can use nginx to lb
