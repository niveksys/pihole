
# Pi-hole

[Pi-hole - Network-wide protection](https://pi-hole.net)

## Table of Contents <!-- omit in toc -->
- [1. Run `pihole` container on Raspberry Pi](#1-run-pihole-container-on-raspberry-pi)
- [2. Change Pi-hole password](#2-change-pi-hole-password)
- [3. Upgrade Pi-hole](#3-upgrade-pi-hole)

## 1. Run `pihole` container on Raspberry Pi
* Connect docker remote host with context
    ```shell
    $ docker context create rpi --docker "host=ssh://pi@raspberrypi.local"
    $ docker context ls
    $ docker context use rpi
    $ docker ps
    ```
* Docker pull image at Raspberry Pi
    ```shell
    $ docker pull pihole/pihole
    ```
* Start `pihole` with docker compose on Raspberry Pi
    ```shell
    $ cd ~/Workspaces/pihole
    $ docker context use rpi
    $ docker compose config
    $ docker compose up -d
    ```
* Admin password is generated and displayed at log
    ```shell
    $ docker logs -f pihole
    ```
* Admin Dashboard: http://raspberrypi.local:8080/admin/

## 2. Change Pi-hole password
```shell
$ docker exec -it pihole pihole -a -p [password]
```

## 3. Upgrade Pi-hole
```shell
$ cd ~/Workspaces/pihole
$ docker pull pihole/pihole
$ docker rm -f pihole
$ docker compose up -d
```

