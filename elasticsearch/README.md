# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy elasticsearch conf file [optional]](#createcopy-elasticsearch-conf-file-optional)
    - [create password file, for initial elastic password](#create-password-file-for-initial-elastic-password)
    - [create ssl files](#create-ssl-files)
  - [best practice start-up](#best-practice-start-up)
  - [production](#production)
  - [References](#references)

---

## basic

### create `.env` file following:

- _HINT: `ELASTICSEARCH_PASSWORD` should be changed_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

ELASTICSEARCH_VERSION=7.14.1

ELK_MEM_USE_GB=3g

ELASTIC_PASSWORD_FILE=run/secrets/bootstrapPassword.txt
DISCOVERY_TYPE=single-node
NODE_NAME=elasticsearch-docker-default-1
CLUSTER_NAME=elasticsearch-docker-cluster

BOOTSTRAP_MEMORY_LOCK=true
NODE_STORE_ALLOW_MMAP=false
INGEST_GEOIP_DOWNLOADER_ENABLED=false
ACTION_DESTRUCTIVE_REQUIRES_NAME=false
XPACK_LICENSE_SELF_GENERATED_TYPE=basic # basic | trial

XPACK_SECURITY_ENABLED=true
XPACK_SECURITY_AUDIT_ENABLED=true
XPACK_SECURITY_AUTHC_REALMS_FILE_FILE1_ORDER=0
XPACK_SECURITY_AUTHC_REALMS_NATIVE_NATIVE1_ORDER=1
XPACK_SECURITY_AUTHC_TOKEN_ENABLED=true
XPACK_SECURITY_AUTHC_API_KEY_ENABLED=true

XPACK_SECURITY_TRANSPORT_SSL_ENABLED=true
XPACK_SECURITY_HTTP_SSL_ENABLED=true
XPACK_SECURITY_TRANSPORT_SSL_KEY=/usr/share/elasticsearch/config/elasticsearch_node.pem
XPACK_SECURITY_TRANSPORT_SSL_CERTIFICATE=/usr/share/elasticsearch/config/elasticsearch_node.crt
XPACK_SECURITY_HTTP_SSL_KEY=/usr/share/elasticsearch/config/elasticsearch_node.pem
XPACK_SECURITY_HTTP_SSL_CERTIFICATE=/usr/share/elasticsearch/config/elasticsearch_node.crt
XPACK_SECURITY_TRANSPORT_SSL_VERIFICATION_MODE=certificate
XPACK_HTTP_SSL_VERIFICATION_MODE=certificate
```

### create/copy elasticsearch conf file [optional]

do not forget to edit it, with your settings

```sh
$cp config/elasticsearch_template.yml config/elasticsearch.yml
```

### create password file, for initial elastic password

change it to your secure password

```sh
$echo 'swordfish$4' > config/bootstrapPassword.txt && chmod 600 config/bootstrapPassword.txt
```

### create ssl files

```sh
$openssl genrsa -out config/elasticsearch_node.pem 4096 && openssl req -new -x509 -sha256 -key config/elasticsearch_node.pem -out config/elasticsearch_node.crt -days 365 -subj '/CN=elasticsearch'
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
