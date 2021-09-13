# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

create `.env` file following:

- _HINT: `ELASTICSEARCH_PASSWORD` should be changed_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=7.14.1
# ELASTICSEARCH_VERSION=7.14.1-amd64
# ELASTICSEARCH_VERSION=7.14.1-arm64

ELASTICSEARCH_LICENSE_TYPE=basic
ELASTICSEARCH_ACTIVATE_SECURITY=true
MEMORY_LOCK=true

ELASTICSEARCH_MEM_USE_GB=3g

ELASTICSEARCH_PROTOCOL=http
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=swordfish
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

- <https://www.elastic.co/downloads/elasticsearch>
- <https://www.docker.elastic.co/r/elasticsearch/elasticsearch>
- <https://github.com/shazChaudhry/docker-elastic>