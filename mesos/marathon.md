# marathon install

### download the marathon
- wget http://downloads.mesosphere.com/marathon/v1.4.1/marathon-1.4.1.tgz
- tar xzf marathon-1.4.1.tgz
- cd marathon-1.4.1

### start the marathon
- ./bin/start --master zk://192.168.199.235:2181/mesos --zk zk://192.168.199.235:2181/marathon
- also start the marathon on the other two server
- you can use marathon-lb to manage the marathon loadbalance

### use marathon to build the deployment
- build the json file named cmd-demo.json
- use the command  [curl -X POST -H "Content-type: application/json" -s -d @cmd-demo.json http://192.168.199.235:8080/v2/apps] to init the app
- use the command [curl  http://192.168.199.235:8080/v2/apps/cmd-demo] to get the info
- use the command [curl -X PUT -H "Content-type: application/json" -s -d @cmd-demo.json http://192.168.199.235:8080/v2/apps] to update the app
