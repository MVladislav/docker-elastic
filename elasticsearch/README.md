# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create password file, for initial elastic password](#create-password-file-for-initial-elastic-password)
  - [Other commands](#other-commands)
  - [References](#references)

---

## basic

> defined to work with treafik

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.2.0

DOMAIN=elastic.home.local
PROTOCOL=http
PORT=9200
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=2g

ELASTIC_PASSWORD_FILE=/run/secrets/bootstrapPassword.txt
# ELASTIC_PASSWORD=

NETWORK_HOST=0.0.0.0
DISCOVERY_TYPE=single-node
NODE_NAME=elasticsearch-docker-home-01
CLUSTER_NAME=elasticsearch-docker-cluster

INGEST_GEOIP_DOWNLOADER_ENABLED=true

XPACK_SECURITY_ENABLED=true
XPACK_SECURITY_AUTHC_TOKEN_ENABLED=true
XPACK_SECURITY_AUDIT_ENABLED=true
XPACK_SECURITY_AUTHC_REALMS_FILE_FILE1_ORDER=0
XPACK_SECURITY_AUTHC_REALMS_NATIVE_NATIVE1_ORDER=1
XPACK_SECURITY_AUTHC_API_KEY_ENABLED=true

XPACK_SECURITY_TRANSPORT_SSL_ENABLED=true
XPACK_SECURITY_HTTP_SSL_ENABLED=false

XPACK_HTTP_SSL_VERIFICATION_MODE=none # certificate | none
XPACK_SECURITY_HTTP_SSL_VERIFICATION_MODE=none # certificate | none
XPACK_SECURITY_TRANSPORT_SSL_VERIFICATION_MODE=none # certificate | none

XPACK_LICENSE_SELF_GENERATED_TYPE=basic # basic | trial
ACTION_DESTRUCTIVE_REQUIRES_NAME=false
```

### create password file, for initial elastic password

example command to create it:

> change it to your secure password

```sh
$echo 'swordfish$4' > config/elastic_secret.txt
```

if something goes wrong you can reset it this way:

> can also be used to set pw for user like 'kibana_system, logstash_system, ...'

```sh
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u logstash_system
```

---

## Other commands

delete all:

```sh
$curl -u 'USERNAME:PASSWORD' -XDELETE ELASTICSEARCHIP:9200/*
```

delete all-indices:

```sh
$curl -u 'USERNAME:PASSWORD' -XDELETE ELASTICSEARCHIP:9200/indices
```

get all indices with size:

```sh
$curl -u 'USERNAME:PASSWORD' -XGET  localhost:9200/_cat/indices?v
```

---

## References

- <https://www.elastic.co/downloads/elasticsearch>
- <https://www.docker.elastic.co/r/elasticsearch/elasticsearch>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
- <https://www.elastic.co/guide/en/elasticsearch/client/curator/5.x/pip.html>
