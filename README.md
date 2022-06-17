# Cluster Opensearch (elasticsearch) + logstash + dashboards (Kibana)

## Prerequisite

At least 3 VM (for 3 opensearch nodes). Place logstash and dashboards where you want.

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

```
$ cd opensearch-dashboards
$ docker compose up -d
```
