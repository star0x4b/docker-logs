# Catching nginx logs to ELK with Docker

## Description:
This repo represents set of containers used to generate and collect logs from one or any number of applications (nginx in this example) to log collector (logstash + elasticsearch + kibana).

## Flow:
Nginx writes logs to /var/log/nginx/nginx_access.log path, which is mounted to host dir. Filebeat takes this logs and sends them to logstash. Logstash uses elasticsearch as storage and Kibana can be used as interface to view, search and analyse logs.\
nginx->filebeat->logstash->elasticsearch

## Usage:
```
$ mkdir /tmp/docker/elk/log/nginx
$ docker-compose up --build 	# --build is only needed for the first run
$ curl http://{$HOST}		# curl nginx host ($HOST) to generate some logs
```
Go to _http://{$HOST}:5601_ to see logs. At the first run you have to choose what index pattern you want to use. Set this pattern to "filebeat-*".

## Notes:
1. You can have one filebeat container on one host and any number app containers. Logs can be writen in one shared file on mounted volume or in files with host-dependent names. Filebeat pattern can be changed to read files by some mask in that case.
2. Logs from other apps can be added to filebeat in the same manner - by configuring path where logs are and adding pattern for this logs.
3. Logstash address configured in filebeat config, so can be changed easy. Also, filebeat can even send different types of logs to different logstash hosts.
4. Logstash and Elasticsearch are scalable services so issues with availability and load can be easily avoided.
5. You can use local disk as storage for elasticsearch data or mount it's data dir to remote storage in any way.
6. Logstash can forward logs to lot of starages, including AWS Cloudwatch. 


