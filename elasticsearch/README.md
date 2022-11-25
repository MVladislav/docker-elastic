# SETUP

```sh
    MVladislav
```

---

- [SETUP](#setup)
  - [basic](#basic)
    - [create your `secrets`:](#create-your-secrets)
    - [create `.env` file following:](#create-env-file-following)
  - [Other commands](#other-commands)
  - [References](#references)

---

## basic

> defined to work with treafik

### create your `secrets`:

```sh
$echo "swordfish" > config/secrets/elastic_secret.txt
```

> manually reset or change afterwards:
>
> > ```sh
> > $docker exec -it "$(docker ps -q -f name=elasticsearch)" /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
> > ```

### create `.env` file following:

```env
NODE_ID=
NODE_ROLE=manager
NETWORK_MODE=overlay

VERSION=8.4.3

DOMAIN=elastic.home.local
PROTOCOL=http
PORT=9200
# default-secured@file | protected-secured@file | admin-secured@file
MIDDLEWARE_SECURED=default-secured@file

ELK_MEM_USE_GB=3g

NETWORK_HOST=0.0.0.0
DISCOVERY_TYPE=single-node
NODE_NAME=elasticsearch-docker-default-1
CLUSTER_NAME=elasticsearch-docker-cluster
```

---

## Other commands

delete all:

```sh
$curl -u 'USERNAME:PASSWORD' -XDELETE ELASTICSEARCHIP:9200/*
```

delete all-indices:

```sh
$curl -u 'USERNAME:PASSWORD' -XDELETE ELASTICSEARCHIP:9200/indices
```

get all indices with size:

```sh
$curl -u 'USERNAME:PASSWORD' -XGET  localhost:9200/_cat/indices?v
```

---

## References

- <https://www.elastic.co/downloads/elasticsearch>
- <https://www.docker.elastic.co/r/elasticsearch/elasticsearch>
- <https://github.com/shazChaudhry/docker-elastic>
- <https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html>
- <https://www.elastic.co/guide/en/elasticsearch/client/curator/5.x/pip.html>
