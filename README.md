# Elastic stack (ELK) on Docker for Harmony Node Monitoring

 [![Elastic Stack version](https://img.shields.io/badge/ELK-7.5.1-blue.svg?style=flat)](https://github.com/deviantony/docker-elk/issues/462)
 
Run [Elastic stack][elk-stack] with Docker and Docker Compose to feed logs from Harmony nodes to Elastic search.  

Based on the official Docker images from Elastic:

* [Elasticsearch](https://github.com/elastic/elasticsearch/tree/master/distribution/docker)
* [Logstash](https://github.com/elastic/logstash/tree/master/docker)
* [Kibana](https://github.com/elastic/kibana/tree/master/src/dev/build/tasks/os_packages/docker_generator)

### Step0: Install docker-compose on AWS EC2 (AMI2)

The instruction is [here](https://docs.docker.com/compose/install/).

## Docker CE Install

```console
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```

Make docker auto-start

`sudo chkconfig docker on`

Reboot to verify it all loads fine on its own.

`sudo reboot`

## docker-compose install

Copy the appropriate `docker-compose` binary from GitHub:

`sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose`

Fix permissions after download: 

`sudo chmod +x /usr/local/bin/docker-compose`

Verify success: 

`docker-compose version`

### Step1: Bring up ELK using docker-compose in AWS EC2

```console
git clone https://github.com/harmony-one/node-log-monitoring.git
cd node-log-monitoring/docker-elk
docker-compose build
docker-compose up
```

Note: open port 5601 for kibana and port 9200 for elasticsearch 

### Step2: Bring up Filebeats using docker in ALL Harmony nodes which generates zerolog

Note that "`pwd`/zerolog" is the path to the actual directory containing zerlog, please update based on your node.
Also, for production, replace elasticsearch IP address in filebeat.yml:

output.elasticsearch:
  hosts: ["localhost:9200"]  ==>  hosts:["production_ip:9200"]

```console
cd ../docker-filebeat/
docker image build -t filebeat:latest .
docker run -v "`pwd`/zerolog:/var/lib/zerolog:ro" -v '/var/run/docker.sock:/var/run/docker.sock' filebeat:latest
```

### Step3: Now you can use Kibana 


Testing   http://localhost:5601  
Production   http://<producition_ip>:5601 

### TODO

1) Add Nginx in Docker to reverse proxy port 9200 and 5601, 
2) Use logstash to stash logs 

  
