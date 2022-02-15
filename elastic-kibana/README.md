# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy kibana conf file](#createcopy-kibana-conf-file)
    - [create ssl files](#create-ssl-files)
  - [dashboard](#dashboard)
    - [index patter](#index-patter)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

## basic

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=8.0.0

DOMAIN=kibana.home.local
PROTOCOL=http
PORT=5601

ELK_MEM_USE_GB=1g

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

### create/copy kibana conf file

do not forget to edit it, with your settings

```sh
$cp config/kibana_template.yml config/kibana.yml
```

### create ssl files

```sh
$openssl genrsa -out config/kibana_node.pem 4096 && openssl req -new -x509 -sha256 -key config/ssl/kibana_node_key.pem -out config/ssl/kibana_node.pem -days 365 -subj '/CN=kibana'
```

---

## dashboard

### index patter

- `alert-*`
- `conn-*`
- `dns-*`
- `http-*`
- `sip-*`
- `tls-*`
- `alert-*,conn-*,dns-*,http-*,sip-*,tls-*`
- `pfelk-captive-*`
- `pfelk-dhcp-*`
- `pfelk-firewall-*`
- `pfelk-firewall_processes-*`
- `pfelk-haproxy-*`
- `pfelk-snort-*`
- `pfelk-suricata-*`
- `pfelk-squid-*`
- `pfelk-unbound-*`
- `pfelk-openvpn-*`
- `pfelk-captive-*,pfelk-dhcp-*,pfelk-firewall-*,pfelk-firewall_processes-*,pfelk-haproxy-*,pfelk-snort-*,pfelk-suricata-*,pfelk-squid-*,pfelk-unbound-*,pfelk-openvpn-*`
- `apm-*-transaction*`
- `traces-apm*`
- `auditbeat-*`
- `endgame-*`
- `filebeat-*`
- `logs-*`
- `packetbeat-*`
- `winlogbeat-*`

---

## best practice start-up

use docker-swarm to manage and start containers.

for that is in each service following defined:

```yml
services:
  ...:
    ...
    deploy:
      mode: replicated
      replicas: 1
      placement:
        max_replicas_per_node: 1
        constraints:
          # - "node.id==${NODE_ID}"
          - "node.role==${NODE_ROLE}"
      restart_policy:
        condition: on-failure
    ...
    ports:
      - target: ...
        published: ...
        mode: host
```

to start this configuration with all supportings between docker-stack and docker-composer
run it with following commando:

```sh
$docker-compose config | docker stack deploy --compose-file - <STACK_NAME>
```

or create directly an alias for it:

```sh
$alias docker-swarm-compose="docker-compose config | docker stack deploy --compose-file -"
```

and run:

```sh
$docker-swarm-compose <STACK_NAME>
```

---

## References

- <https://www.elastic.co/downloads/kibana>
- <https://www.docker.elastic.co/r/kibana/kibana>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
