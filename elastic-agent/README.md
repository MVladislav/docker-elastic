# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
  - [commands](#commands)
    - [reload dashboard](#reload-dashboard)
  - [endpoint protection - client setup](#endpoint-protection---client-setup)
    - [install](#install)
    - [enroll and run:](#enroll-and-run)
    - [alternative](#alternative)
      - [install](#install-1)
      - [enroll and run:](#enroll-and-run-1)
  - [References](#references)

---

## basic

> defined to work with treafik

### create `.env` file following:

- _HINT: `FLEET_SERVER_SERVICE_TOKEN` should be changed, get from elasticsearch_

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.1.3

DOMAIN_AGENT=agent.home.local
PROTOCOL_AGENT=http
PORT_AGENT=8220
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

PORT_IPTABLES=9002
PORT_PFSENSE=9003

ELK_MEM_USE_GB=1g

ELASTICSEARCH_PROTOCOL=https
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=

KIBANA_PROTOCOL=https
KIBANA_HOST=kibana
KIBANA_PORT=5601

KIBANA_USERNAME=elastic
KIBANA_PASSWORD=

SSL_VERIFICATION_MODE=none

FLEET_SERVER_ENABLE=true
FLEET_SERVER_SERVICE_TOKEN=<SERVICE_TOKEN>
FLEET_SERVER_POLICY=<POLICY>
FLEET_SERVER_INSECURE_HTTP=false

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

---

## commands

### reload dashboard

> if removed or broken, you can reload integrations

```sh
curl --request POST \
  --url https://kibana:5601/api/fleet/epm/packages/[package name]-[package version] \
   -u elastic:{PASSWORD} \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: elastic agent 8.0.0' \
  --header 'kbn-xsrf: xx' \
  --data '{ "force": true}'
```

---

## endpoint protection - client setup

### install

amd64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-amd64.deb
$sudo apt install ./elastic-agent-7.14.1-amd64.deb
```

arm64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-arm64.deb
$sudo apt install ./elastic-agent-7.14.1-arm64.deb
```

### enroll and run:

```sh
$sudo elastic-agent enroll -f --insecure --url=https://agent.home.local --enrollment-token=<enrollment_token>
$sudo systemctl enable elastic-agent
$sudo systemctl start elastic-agent
```

### alternative

#### install

amd64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-linux-x86_64.tar.gz
$tar -xvf elastic-agent-7.14.1-linux-x86_64.tar.gz && cd elastic-agent-7.14.1-linux-x86_64
```

arm64:

```sh
$curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.14.1-linux-arm64.tar.gz
$tar -xvf elastic-agent-7.14.1-linux-arm64.tar.gz && cd elastic-agent-7.14.1-linux-arm64
```

#### enroll and run:

```sh
$sudo ./elastic-agent install -f --insecure --url=https://agent.home.local --enrollment-token=<enrollment_token>
```

---

## References

- <https://www.elastic.co/guide/en/fleet/current/elastic-agent-container.html>
- <https://www.elastic.co/guide/en/fleet/7.15/fleet-server.html>
- <https://www.elastic.co/downloads/elastic-agent>
- <https://www.elastic.co/guide/en/fleet/current/beats-agent-comparison.html>
- <https://www.elastic.co/guide/en/fleet/current/fleet-faq.html>
