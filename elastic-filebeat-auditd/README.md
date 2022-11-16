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

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.5.1

ELK_MEM_USE_GB=1g

GRAYLOG_HOST=graylog
GRAYLOG_PORT=5044
GRAYLOG_SSL_ENABLE=true
GRAYLOG_SSL_VERIFICATION_MODE=none
```

---

## References

- <https://www.elastic.co/guide/en/beats/filebeat/current/running-on-docker.html>
- <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html>
- <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-threatintel.html>
- <https://www.elastic.co/blog/ingesting-threat-data-with-threat-intel-filebeat-module>
- <https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html>
- <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-netflow.html>
