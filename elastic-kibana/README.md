# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create `.env` file following:](#create-env-file-following)
    - [create/copy kibana conf file](#createcopy-kibana-conf-file)
  - [dashboard](#dashboard)
    - [add missing `@timestamp` or any other times](#add-missing-timestamp-or-any-other-times)
    - [add missing `event.category`](#add-missing-eventcategory)
    - [index patter](#index-patter)
  - [Dahboard Examples](#dahboard-examples)
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

DOMAIN=kibana.home.local
PROTOCOL=http
PORT=5601
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=protected-secured@file

ELK_MEM_USE_GB=1g

# ELASTICSEARCH_PROTOCOL=https
# ELASTICSEARCH_HOST=elasticsearch
# ELASTICSEARCH_PORT=9200
#
# ELASTICSEARCH_USERNAME=kibana_system
# ELASTICSEARCH_PASSWORD=<PASSWORD>
# ELASTICSEARCH_SSL_VERIFICATIONMODE=none
#
# XPACK_REPORTING_ENCRYPTIONKEY=<ADD_HERE (32_characters)>
# XPACK_SECURITY_ENCRYPTIONKEY=<ADD_HERE (32_characters)>
# XPACK_ENCRYPTEDSAVEDOBJECTS_ENCRYPTIONKEY=<ADD_HERE (32_characters)>

ELASTICSEARCH_NETWORK_NAME=elasticsearch
```

### create/copy kibana conf file

> do not forget to edit it, with your settings

```sh
$cp config/kibana_template.yml config/kibana.yml
```

---

## dashboard

### add missing `@timestamp` or any other times

> needed for all internal elastic function usage like security

```http
PUT <INDEXNAME>-*/_mapping
{
  "properties": {
    "@timestamp": {
      "type": "alias",
      "path": "start_time"
    }
  }
}
```

```http
PUT <INDEXNAME>-*/_mapping
{
  "properties": {
    "event.ingested": {
      "type": "alias",
      "path": "start_time"
    }
  }
}
```

### add missing `event.category`

> needed for all internal elastic function usage like security.
>
> value e.x.: network, file, process

```http
PUT <INDEXNAME>-*/_mapping
{
  "properties": {
    "event.category": {
      "type":  "constant_keyword",
      "value": "network"
    }
  }
}
```

### index patter

- `alert-*`
- `conn-*`
- `dns-*`
- `http-*`
- `sip-*`
- `tls-*`
- `alert-*,conn-*,dns-*,http-*,sip-*,tls-*`
- `pfelk-captive-*`
- `pfelk-dhcp-*`
- `pfelk-firewall-*`
- `pfelk-firewall_processes-*`
- `pfelk-haproxy-*`
- `pfelk-snort-*`
- `pfelk-suricata-*`
- `pfelk-squid-*`
- `pfelk-unbound-*`
- `pfelk-openvpn-*`
- `pfelk-captive-*,pfelk-dhcp-*,pfelk-firewall-*,pfelk-firewall_processes-*,pfelk-haproxy-*,pfelk-snort-*,pfelk-suricata-*,pfelk-squid-*,pfelk-unbound-*,pfelk-openvpn-*`
- `apm-*-transaction*`
- `traces-apm*`
- `auditbeat-*`
- `endgame-*`
- `filebeat-*`
- `logs-*`
- `packetbeat-*`
- `winlogbeat-*`

---

## Dahboard Examples

- <https://github.com/psychogun/zenarmor-kibana-dashboards>
- <https://github.com/pfelk/pfelk/tree/main/etc/pfelk/dashboard>

---

## References

- <https://www.elastic.co/downloads/kibana>
- <https://www.docker.elastic.co/r/kibana/kibana>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
