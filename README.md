# Elastic stack (ELK) on Docker for Harmony Node Monitoring

 [![Elastic Stack version](https://img.shields.io/badge/ELK-7.5.1-blue.svg?style=flat)](https://github.com/deviantony/docker-elk/issues/462)
 
Run [Elastic stack][elk-stack] with Docker and Docker Compose to feed logs from Harmony nodes to Elastic search.  

Based on the official Docker images from Elastic:

* [Elasticsearch](https://github.com/elastic/elasticsearch/tree/master/distribution/docker)
* [Logstash](https://github.com/elastic/logstash/tree/master/docker)
* [Kibana](https://github.com/elastic/kibana/tree/master/src/dev/build/tasks/os_packages/docker_generator)

### Step0: Install docker-compose on AWS , GCP or Digital Ocean VMs

The instruction is [here](https://docs.docker.com/compose/install/).

### Step1: Bring up ELK using docker-compose in AWS/GCP/DigitalOcean

```console
cd docker-elk
docker-compose build
docker-compose up
```

### Step2: Bring up Filebeats using docker in ALL Harmony nodes which generates zerolog

Note that "`pwd`/zerolog" is the path to the actual directory containing zerlog, please update based on your node.

```console
cd ../docker-filebeat/
docker image build -t filebeat:latest .
docker run -v "`pwd`/zerolog:/var/lib/zerolog:ro" -v '/var/run/docker.sock:/var/run/docker.sock' filebeat:latest
```

### Step3: For testing Access Kibana accessed http://localhost:5601  

### Step4: For production Access Kibana accessed http://<your_ip>:5601 in AWS/GCP/DigialOcean
  
