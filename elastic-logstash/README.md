# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy elasticsearch conf file [optional (not active in composer)]](#createcopy-elasticsearch-conf-file-optional-not-active-in-composer)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

## basic

### create `.env` file following:

- _HINT: `ELASTICSEARCH_PASSWORD` should be changed_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=8.0.0

ELK_MEM_USE_GB=1g

LOGSTAST_RELOAD_AUTOMATIC=true
XPACK_MONITORING_ENABLED=true

ELASTICSEARCH_PROTOCOL=https
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=logstash_system
ELASTICSEARCH_PASSWORD=<PASSWORD>
xpack.monitoring.elasticsearch.password=<PASSWORD>

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

### create/copy elasticsearch conf file [optional (not active in composer)]

do not forget to edit it, with your settings

```sh
$cp config/logstash/config/logstash_example.yml config/logstash/config/logstash.yml
```

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

- <https://www.docker.elastic.co/r/logstash/logstash>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://github.com/pfelk/pfelk>
