# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy elasticsearch conf file [optional (not active in composer)]](#createcopy-elasticsearch-conf-file-optional-not-active-in-composer)
    - [create password file, for initial elastic password](#create-password-file-for-initial-elastic-password)
    - [create ssl files](#create-ssl-files)
  - [best practice start-up](#best-practice-start-up)
  - [production](#production)
  - [References](#references)

---

## basic

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=8.0.0

ELK_MEM_USE_GB=3g

ELASTIC_PASSWORD_FILE=/run/secrets/bootstrapPassword.txt
NETWORK_HOST=0.0.0.0
DISCOVERY_TYPE=single-node
NODE_NAME=elasticsearch-docker-default-1
CLUSTER_NAME=elasticsearch-docker-cluster

BOOTSTRAP_MEMORY_LOCK=true
NODE_STORE_ALLOW_MMAP=false
INGEST_GEOIP_DOWNLOADER_ENABLED=false

XPACK_SECURITY_ENABLED=true
XPACK_SECURITY_AUTHC_TOKEN_ENABLED=true
XPACK_SECURITY_AUDIT_ENABLED=true
XPACK_SECURITY_AUTHC_REALMS_FILE_FILE1_ORDER=0
XPACK_SECURITY_AUTHC_REALMS_NATIVE_NATIVE1_ORDER=1
XPACK_SECURITY_AUTHC_API_KEY_ENABLED=true

XPACK_SECURITY_TRANSPORT_SSL_ENABLED=true
XPACK_SECURITY_HTTP_SSL_ENABLED=true

XPACK_SECURITY_HTTP_SSL_KEY=certs/elasticsearch_node.key
XPACK_SECURITY_HTTP_SSL_CERTIFICATE=certs/elasticsearch_node.crt
XPACK_SECURITY_TRANSPORT_SSL_KEY=certs/elasticsearch_node.key
XPACK_SECURITY_TRANSPORT_SSL_CERTIFICATE=certs/elasticsearch_node.crt

XPACK_HTTP_SSL_VERIFICATION_MODE=certificate # certificate | none
XPACK_SECURITY_HTTP_SSL_VERIFICATION_MODE=certificate # certificate | none
XPACK_SECURITY_TRANSPORT_SSL_VERIFICATION_MODE=certificate # certificate | none

XPACK_LICENSE_SELF_GENERATED_TYPE=basic # basic | trial
ACTION_DESTRUCTIVE_REQUIRES_NAME=false
```

### create/copy elasticsearch conf file [optional (not active in composer)]

do not forget to edit it, with your settings

```sh
$cp config/elasticsearch_template.yml config/elasticsearch.yml
```

### create password file, for initial elastic password

example command to create it:

> change it to your secure password

```sh
$echo 'swordfish$4' > config/bootstrapPassword.txt && chmod 600 config/bootstrapPassword.txt
```

if something goes wrong you can reset it this way:

> can also be used to set pw for user like 'kibana_system, logstash_system, ...'

```sh
$docker exec -it elasticsearch /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

### create ssl files

```sh
$openssl genrsa -out config/ssl/elasticsearch_node.key 4096 && openssl req -new -x509 -sha256 -key config/ssl/elasticsearch_node.key -out config/ssl/elasticsearch_node.crt -days 365 -subj '/CN=elasticsearch'
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

## production

run following on the host system:

```sh
$sysctl -w vm.max_map_count=262144
```

---

## References

- <https://www.elastic.co/downloads/elasticsearch>
- <https://www.docker.elastic.co/r/elasticsearch/elasticsearch>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
