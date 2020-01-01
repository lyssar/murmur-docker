# Murmur Docker Container

TBD

## Docker tags:
| Tag | Murmur Version | Description | Release Date |
| --- | :---: | --- | :---: |
TBD

---

## Volumes

This container exposes four volumes:
* `/opt/murmur/config` - Murmur configuration files
* `/opt/murmur/data` - Murmur database and other data files
* `/opt/murmur/log` - Murmur log for troubleshooting

## Ports
* `64738/tcp` Murmur server TCP port
* `64738/udp` Murmur server UDP port

---

**Start via Docker**

```bash
$ docker run --name cef-murmur -d \
    -p 64738:64738/udp -p 64738:64738 \
    cefurox/murmur
```  

---

**Recommended: Start via [Docker Compose](https://docs.docker.com/compose/):**

Have the container store the config & logs on a local file-system or in a specific, known data volume (recommended for persistence and
 troubleshooting):
 
 
```yaml
version: '3.7'

networks:
    murmur:
services:
    murmur:
        container_name: cef-murmur
        image: cefurox/murmur
        networks:
            - murmur
        restart: always
        ports:
            - 64738:64738
            - 64738:64738/udp
        volumes:
            - ./config:/opt/murmur/config
            - ./data:/opt/murmur/data
            - ./log:/opt/murmur/log
        environment:
            TZ: UTC
```

---

## Change super password

This can be done while the mumble server is running.

```bash
docker exec -it cef-murmur ./setpw 
```

To get the random generated password from your first boot:

```bash
docker logs cef-murmur 2>&1 | grep "Password for 'SuperUser'"
```

[//]: # (Licensed under the Apache 2.0 license)
[//]: # (Copyright 2020 cef - devmaint@cefurox.de)