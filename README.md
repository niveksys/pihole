
# Pi-hole

* Generate key pairs and copy public key to remote host
    ```shell
    $ cd ~/.ssh
    $ ssh-keygen -t ed25519 -f id_ed25519_mac-mini
    # ssh-keygen -t rsa -b 4096 -f id_rsa_mac-mini
    $ ssh-add -l
    $ ssh-add -D
    $ ssh-add -K ~/.ssh/id_ed25519_mac-mini
    $ ssh-copy-id -i ~/.ssh/id_ed25519_mac-mini kevin@mac-mini.local
    $ ssh kevin@mac-mini.local
    ```

* For without `ssh-copy-id` command, setup authorized_keys at remote host
    ```shell
    @mac-mini
    $ ssh kevin@mac-mini.local
    $ cd ~
    $ mkdir .ssh
    $ chmod 700 .ssh
    $ touch .ssh/authorized_keys
    $ chmod 640 .ssh/authorized_keys

    @macbook-air
    $ cat ~/.ssh/id_rsa.pub | ssh kevin@mac-mini 'cat >> .ssh/authorized_keys'
    ```

* Configure SSH environment at remote host
    - Update `/etc/ssh/sshd_config`, change `#PermitUserEnvironment no` to `PermitUserEnvironment yes`
    - System Preferences > Sharing > Remote Login: restart SSH by unchecking and checking the checkbox
    - create a new file `~/.ssh/environment`
        ```shell
        $ echo "PATH=$PATH:/usr/local/bin" >> ~/.ssh/environment
        ```

* Connect docker remote host with SSH connection
    ```shell
    $ docker -H ssh://kevin@mac-mini.local ps
    ```

* Connect docker remote host with Context
    ```shell
    $ docker context create mac-mini --docker "host=ssh://kevin@mac-mini.local"
    $ docker context ls
    $ docker context use mac-mini
    $ docker context ls
    $ docker ps
    ```

* Start Pi-hole with docker compose on remote host
    ```shell
    $ cd ~/Workspaces/pihole
    $ docker compose up -d
    ```

* Admin password is generated and displayed at log
    ```shell
    $ docker logs -f pihole
    ```

* Change Pi-hole password
    ```shell
    $ docker exec -it pihole pihole -a -p [password]
    ```

* Upgrade Pi-hole
    ```shell
    $ cd ~/Workspaces/pihole
    $ docker pull pihole/pihole
    $ docker rm -f pihole
    $ docker compose up -d
    ```

* Creating Docker macvlan network (broken on Mac OSX...)
    - [Set up a PiHole using Docker MacVlan Networks](https://blog.ivansmirnov.name/set-up-pihole-using-docker-macvlan-network/)
    - [Using Docker macvlan Networks](https://blog.oddbit.com/post/2018-03-12-using-docker-macvlan-networks/)
    - [macvlan driver doesn't work in MacOS](https://github.com/docker/for-mac/issues/3926)
    - [macvlan / ipvlan parent interface (e.g. en0 instead of eth0) broken on Mac OSX](https://github.com/moby/libnetwork/issues/2614)
    ```shell
    $ docker network create -d macvlan -o parent=en0 \
        --subnet 192.168.1.0/24 \
        --gateway 192.168.1.1 \
        --ip-range 192.168.1.10/28 \
        --aux-address 'host=192.168.1.11' \
        macvlan0
    ```
