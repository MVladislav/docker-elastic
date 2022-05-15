# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
  - [elastic setup](#elastic-setup)
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

DOMAIN=apm.home.local
PROTOCOL=http
PORT=8200
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=1g

ELASTICSEARCH_PROTOCOL=http
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=<PASSWORD>

KIBANA_PROTOCOL=http
KIBANA_HOST=kibana
KIBANA_PORT=5601

KIBANA_USERNAME=elastic
KIBANA_PASSWORD=<PASSWORD>

SSL_VERIFICATION_MODE=none

ELASTICSEARCH_NETWORK_NAME=elasticsearch
KIBANA_NETWORK_NAME=kibana
```

## elastic setup

you need to go in kibana under `Management > Integration > Elastic APM`
and add the agent to elastic, else this service will not connect with elastic.

---

## References

- <https://www.elastic.co/observability/application-performance-monitoring>
- <https://www.elastic.co/guide/en/apm/get-started/current/quick-start-overview.html>
- <https://www.elastic.co/guide/en/apm/guide/master/configuration-ssl.html>
