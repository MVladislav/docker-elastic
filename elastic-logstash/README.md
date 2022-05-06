# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
  - [References](#references)

---

## basic

> defined to work with treafik

### create `.env` file following:

- _HINT: `ELASTICSEARCH_PASSWORD` should be changed_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.1.3

# DOMAIN=logstash.home.local
PORT_OPNSENSE=5140
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=1g

LOGSTAST_RELOAD_AUTOMATIC=true
XPACK_MONITORING_ENABLED=true

ELASTICSEARCH_PROTOCOL=https
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=<PASSWORD>

ELASTICSEARCH_SSL_VERIFICATIONMODE=none

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

---

## References

- <https://www.docker.elastic.co/r/logstash/logstash>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://github.com/pfelk/pfelk>
