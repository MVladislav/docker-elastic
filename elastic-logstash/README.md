# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
  - [agents](#agents)
    - [winlogbeat](#winlogbeat)
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

VERSION=8.2.0

# DOMAIN=logstash.home.local
PORT_OPNSENSE=5140
PORT_BEATS=5044
# default-whitelist@file | protected-whitelist@file | admin-whitelist@file
MIDDLEWARE_SECURED=default-whitelist@file

ELK_MEM_USE_GB=1g

LOGSTAST_RELOAD_AUTOMATIC=true
XPACK_MONITORING_ENABLED=true

ELASTICSEARCH_PROTOCOL=http
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=<PASSWORD>

ELASTICSEARCH_SSL_VERIFICATIONMODE=none

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

## agents

### winlogbeat

> `winlogbeat.yml`

```yml
###################### Winlogbeat Configuration Example #########################
# Winlogbeat 6, 7, and 8 are currently supported!
# You can download the latest stable version of winlogbeat here:
# https://www.elastic.co/downloads/beats/winlogbeat
# https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-reference-yml.html

# --------------------------- Windows Logs To Collect --------------------------
winlogbeat.event_logs:
  - name: Application
    ignore_older: 30m
  - name: Security
    ignore_older: 30m
  - name: System
    ignore_older: 30m
  - name: Microsoft-windows-sysmon/operational
    ignore_older: 30m
  - name: Microsoft-Windows-PowerShell/Operational
    event_id: 4103, 4104, 4105, 4106
    ignore_older: 30m
  - name: Windows PowerShell
    event_id: 400, 403, 600, 800
    ignore_older: 30m
  - name: Microsoft-Windows-WMI-Activity/Operational
    event_id: 5857,5858,5859,5860,5861

# ------------------------------- Logstash output ------------------------------
output.logstash:
  hosts: ["<LOGSTASH-IP>:5044"]
  topic: "winlogbeat"
  max_retries: 2
  max_message_bytes: 1000000
```

---

## References

- <https://www.docker.elastic.co/r/logstash/logstash>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://github.com/pfelk/pfelk>
- <https://github.com/Cyb3rWard0g/HELK>
