# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [endpoint protection - client setup](#endpoint-protection---client-setup)
    - [install](#install)
    - [enroll and run:](#enroll-and-run)
    - [alternative](#alternative)
      - [install](#install-1)
      - [enroll and run:](#enroll-and-run-1)
  - [best practice start-up](#best-practice-start-up)
  - [References](#references)

---

create `.env` file following:

- _HINT: `FLEET_SERVER_SERVICE_TOKEN` should be changed, get from elasticsearch_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=7.14.1

ELK_MEM_USE_GB=1g

ELASTICSEARCH_PROTOCOL=https
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

FLEET_SERVER_ENABLE=true
FLEET_SERVER_SERVICE_TOKEN=<SERVICE_TOKEN>
FLEET_SERVER_POLICY=<POLICY>
FLEET_SERVER_INSECURE_HTTP=false

ELASTICSEARCH_NETWORK_NAME=elastic_default
```

---

## endpoint protection - client setup

### install

amd64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-amd64.deb
$sudo apt install ./elastic-agent-7.14.1-amd64.deb
```

arm64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-arm64.deb
$sudo apt install ./elastic-agent-7.14.1-arm64.deb
```

### enroll and run:

```sh
$sudo elastic-agent enroll -f --insecure --url=https://<elasticsearch_host>:8220 --enrollment-token=<enrollment_token>
$sudo systemctl enable elastic-agent
$sudo systemctl start elastic-agent
```

### alternative

#### install

amd64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-linux-x86_64.tar.gz
$tar -xvf elastic-agent-7.14.1-linux-x86_64.tar.gz && cd elastic-agent-7.14.1-linux-x86_64
```

arm64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-linux-arm64.tar.gz
$tar -xvf elastic-agent-7.14.1-linux-arm64.tar.gz && cd elastic-agent-7.14.1-linux-arm64
```

#### enroll and run:

```sh
$sudo ./elastic-agent install -f --insecure --url=https://<elasticsearch_host>:8220 --enrollment-token=<enrollment_token>
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

- <https://www.elastic.co/guide/en/fleet/current/elastic-agent-container.html>
- <https://www.elastic.co/guide/en/fleet/7.15/fleet-server.html>
- <https://www.elastic.co/downloads/elastic-agent>
