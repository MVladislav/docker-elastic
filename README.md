# Elastic - SIEM - Docker - Deploy

```sh
  MVladislav
```

---

- [Elastic - SIEM - Docker - Deploy](#elastic---siem---docker---deploy)
  - [about](#about)

---

## about

this repo is used to deploy **elasticsearch** with **kibana** as **SIEM**

then deploy **elastic-agent** for handle device integration to collect logs

\+ deploy **logstash** from [pfelk](https://github.com/pfelk/pfelk)

---

## info to run all

> cd into every folder (you need to run) and run following command in correct folder.
>
> do not foget to create `.env` files and `cp` conf templates (described in READMEs).

```sh
$docker-swarm-compose elasticsearch
$docker-swarm-compose kibana
$docker-swarm-compose logstash
$docker-swarm-compose elastic-agent
$docker-swarm-compose apm-server
$docker-swarm-compose filebeat
```

---

## other

### best practice start-up

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

### production

run following on the host system:

```sh
$sysctl -w vm.max_map_count=262144
```
