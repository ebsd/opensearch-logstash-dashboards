# Cluster Opensearch (elasticsearch) + logstash + dashboards (Kibana)

Here are some quick setup to start a cluster. You'll need to work on :
- ssl verify enabling
- heap memory configuration (read bests practices)

## Prerequisite

1. At least 3 VM (for 3 opensearch nodes). Place logstash and dashboards where you want.
2. Docker install
3. You'll probably need to set up : `sysctl -w vm.max_map_count=262144` for opensearch running.

## Configure Opensearch docker-compose.yaml on 3 nodes

In each `opensearch-node[1|2|3]/docker-compose.yaml` directory, modify at least :

```
    extra_hosts:
      - "opensearch-node1:192.168.56.137"
      - "opensearch-node2:192.168.56.138" 
      - "opensearch-node3:192.168.56.139"
```

## Configure logstash

Link you logstash configuration and place your config file in `./pipeline/`. Mine is for winlogbeat.

```
    volumes:
      - ./pipeline/:/usr/share/logstash/pipeline/
```


## Configure Daskboards

In `docker-compose.yaml`, set the Kibana host IP.
```
    extra_hosts:
      - "kibana.local:192.168.56.137"
```

And in `config/kibana.yml`, set de Opensearch nodes IP.
```
elasticsearch.hosts: ["https://192.168.56.137:9200","https://192.168.56.138:9200","https://192.168.56.139:9200"]
```


## Run Opensearch on each nodes

Default creds : admin/admin

```
$ cd opensearch-node[1|2|3]
$ docker compose up -d
```

## Run logstash

```
$ cd logstash
$ docker compose up -d
```

## Run Dashboards

Default creds : admin/admin

```
$ cd opensearch-dashboards
$ docker compose up -d
```
