# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create password file, for initial elastic password](#create-password-file-for-initial-elastic-password)
  - [References](#references)

---

## basic

> defined to work with treafik

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.1.3

DOMAIN=elastic.home.local
PROTOCOL=http
PORT=9200
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=3g

ELASTIC_PASSWORD_FILE=/run/secrets/bootstrapPassword.txt
NETWORK_HOST=0.0.0.0
DISCOVERY_TYPE=single-node
NODE_NAME=elasticsearch-docker-default-1
CLUSTER_NAME=elasticsearch-docker-cluster

BOOTSTRAP_MEMORY_LOCK=true
NODE_STORE_ALLOW_MMAP=false
INGEST_GEOIP_DOWNLOADER_ENABLED=true

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

### create password file, for initial elastic password

example command to create it:

> change it to your secure password

```sh
$echo 'swordfish$4' > config/bootstrapPassword.txt && chmod 600 config/bootstrapPassword.txt
```

if something goes wrong you can reset it this way:

> can also be used to set pw for user like 'kibana_system, logstash_system, ...'

```sh
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system
$docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u logstash_system
```

---

## References

- <https://www.elastic.co/downloads/elasticsearch>
- <https://www.docker.elastic.co/r/elasticsearch/elasticsearch>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
