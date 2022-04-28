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

> defined to work with treafik

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.1.3

DOMAIN=apm.home.local
PROTOCOL=http
PORT=8200
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=1g

ELASTICSEARCH_PROTOCOL=https
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=<PASSWORD>

KIBANA_PROTOCOL=https
KIBANA_HOST=kibana
KIBANA_PORT=5601

KIBANA_USERNAME=elastic
KIBANA_PASSWORD=<PASSWORD>

SSL_VERIFICATION_MODE=none

ELASTICSEARCH_NETWORK_NAME=elasticsearch
KIBANA_NETWORK_NAME=kibana
```

### create/copy apm conf file

> do not forget to edit it, with your settings

```sh
$cp config/apm-server_template.yml config/apm-server.yml
```

---

## References

- <https://www.elastic.co/observability/application-performance-monitoring>
- <https://www.elastic.co/guide/en/apm/get-started/current/quick-start-overview.html>
- <https://www.elastic.co/guide/en/apm/guide/master/configuration-ssl.html>
