# Graylog with logstash multiline parser
Sample configuration for Graylog-Logstash-Elasticsearch-Mongo stack using logstash multiline plugin
## BUILD
To run docker containers just clone repo and run:
```bash
docker-compose up --build -d
```
## VARIABLES

Admin password: *GRAYLOG_ROOT_PASSWORD_SHA2*

To change admin password generate sha2 string with terminal:
```bash
$ echo -n admin | shasum -a 256
8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918  -
```
And set env for graylog service in docker-compsoe.yml:
```bash
environment:
  - GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918 # password: admin
```

External graylog address: *GRAYLOG_HTTP_EXTERNAL_URI*
Set URL set where graylog will be available. For example:
```bash
environment: 
  - GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000
```

## TEST

Configure GELF UPD input in graylog and try to send logs to logstash with netcat:
```bash
echo -e '{"version": "1.1","host":"example.org","short_message":"Short message","full_message":"Backtrace here\\n\\nmore stuff","level":1,"_user_id":9001,"_some_info":"foo","_some_env_var":"bar"}' | nc -u -w 1 localhost 12201
```
