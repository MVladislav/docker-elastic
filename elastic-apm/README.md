# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy apm conf file](#createcopy-apm-conf-file)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

## basic

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.0.0

DOMAIN=apm.home.local
PROTOCOL=http
PORT=8200

ELK_MEM_USE_GB=1g

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

### create/copy apm conf file

> do not forget to edit it, with your settings

```sh
$cp config/apm-server_template.yml config/apm-server.yml
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

- <https://www.elastic.co/observability/application-performance-monitoring>
- <https://www.elastic.co/guide/en/apm/get-started/current/quick-start-overview.html>
- <https://www.elastic.co/guide/en/apm/guide/master/configuration-ssl.html>
