
# Pi-hole

* Generate key pairs and copy public key to remote host
    ```shell
    $ cd ~/.ssh
    $ ssh-keygen -t rsa -b 4096 -C "user@host.com"
    $ chmod 600 id_rsa id_rsa.pub
    $ ssh-add -l
    $ ssh-add -D
    $ ssh-add -K ~/.ssh/id_rsa
    $ ssh-copy-id -i ~/.ssh/id_rsa kevin@mac-mini.local
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
    $ docker logs pihole
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
