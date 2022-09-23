# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
  - [create index/dasboards/pipelines](#create-indexdasboardspipelines)
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

## create index/dasboards/pipelines

> after metricbeat stats, you can run:

```sh
$docker exec "$(docker ps -q -f name=metricbeat)" metricbeat setup --dashboards -e
```

---

## References

- <https://github.com/elastic/beats/tree/main/metricbeat>
- <https://stackoverflow.com/questions/72087195/metricbeat-not-showing-pod-cpu-usage>
