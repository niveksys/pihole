
# Pi-hole

[Pi-hole - Network-wide protection](https://pi-hole.net)

## Table of Contents <!-- omit in toc -->
- [1. Run `pihole` container on docker remote host](#1-run-pihole-container-on-docker-remote-host)
- [2. Change `pihole` password](#2-change-pihole-password)
- [3. Upgrade `pihole` container on docker remote host](#3-upgrade-pihole-container-on-docker-remote-host)

## 1. Run `pihole` container on docker remote host
* Connect docker remote host (e.g. Raspberry Pi, Synology NAS) with context
    ```shell
    $ docker context create context-name --docker "host=ssh://user@hostname.local"
    $ docker context ls
    $ docker context use context-name
    $ docker context ls
    $ docker ps
    ```
* Docker pull image at docker remote host
    ```shell
    $ docker pull pihole/pihole
    ```
* Start `pihole` with docker compose on docker remote host
    ```shell
    $ cd ~/Workspaces/pihole
    $ docker context use context-name
    $ docker compose config
    $ docker compose up -d
    ```
* Admin password is generated and displayed at log
    ```shell
    $ docker logs -f pihole
    ```
* Admin Dashboard: http://hostname.local:8080/admin/

## 2. Change `pihole` password
```shell
$ docker exec -it pihole pihole -a -p [password]
```

## 3. Upgrade `pihole` container on docker remote host

* Upgrade `pihole` with docker compose on docker remote host
    ```shell
    $ cd ~/Workspaces/pihole
    $ docker context use context-name
    $ docker compose pull
    $ docker compose up -d
    $ docker compose logs -f
    $ docker image prune -a
    ```
