# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [dashboard](#dashboard)
    - [index patter](#index-patter)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

create `.env` file following:

- _HINT: `ELASTICSEARCH_ENC_KEY` must be 32-chars long_
- _HINT: `ELASTICSEARCH_PASSWORD` should be changed_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=7.14.1
# ELASTICSEARCH_VERSION=7.14.1-amd64
# ELASTICSEARCH_VERSION=7.14.1-arm64

ELASTICSEARCH_ACTIVATE_SECURITY=true
ELASTICSEARCH_ENC_KEY=<ENC_KEY>

KIBANA_MEM_USE_GB=1g

ELASTICSEARCH_PROTOCOL=http
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=kibana_system
ELASTICSEARCH_PASSWORD=<PASSWORD>

ELASTICSEARCH_NETWORK_NAME=elastic_default
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
- `pfelk-captive-*`
- `pfelk-dhcp-*`
- `pfelk-firewall-*`
- `pfelk-haproxy-*`
- `pfelk-snort-*`
- `pfelk-suricata-*`
- `pfelk-squid-*`
- `pfelk-unbound-*`
- `pfelk-openvpn-*`
- `apm-*-transaction*`
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
