# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy filebeat conf file](#createcopy-filebeat-conf-file)
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

### create/copy filebeat conf file

> do not forget to edit it, with your settings

```sh
$cp config/filebeat_template.yml config/filebeat.yml
```

## create index/dasboards/pipelines

> after filbeat stats, you can run:

```sh
$docker exec <CONTAINER> filebeat setup -e
```

---

## References

- <https://www.elastic.co/guide/en/beats/filebeat/current/running-on-docker.html>
- <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html>
- <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-threatintel.html>
- <https://www.elastic.co/blog/ingesting-threat-data-with-threat-intel-filebeat-module>
