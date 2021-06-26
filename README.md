# subsonic-docker

This repository contains instructions for self-hosting [Subsonic](https://subsonic.org) on your own computer or server using Docker. These instructions have been tested on Ubuntu 18.04 LTS, however they should would unmodified on other platforms. 

## Pre-requisites

Before proceeding, make sure the following are installed and configured 

- `Docker` 
- `Docker Compose`
- `Git` Distributed Version Control System

## Initial setup

1. Start by cloning this repository to a folder on your server

## Starting Subsonic

Once you have your customizations set-up, run the following in the cloned folder
```bash
docker-compose up -d
```

To view the logs for docker, run the following command
```bash
docker-compose logs -f
```

Open up your browser (Firefox, Vivaldi, Chrome) on your workstation and enter `http://<address>:<port>/` of your Subsonic instance where `<address>` is the IP Address of your computer or server and `<port>` is the one configured in the `docker-compose.yml` file. By default, the port is `30002`.

Subsonic is set to start automatically on every system start-up.


## Securing connection to Subsonic using TLS (Optional)

### Pre-requisites

### Caddy v2

1. Download the `caddy` webserver
    ```bash
    sudo curl "https://caddyserver.com/api/download?os=linux&arch=amd64&idempotency=81212248794919" -o /usr/bin/caddy
    ```

2. Make the `caddy` binary executable
    ```bash
    sudo chmod 755 /usr/bin/caddy
    ```

3. Copy the following files to their respective destinations
    ```bash
    sudo cp caddy/caddy.service /etc/systemd/system/caddy.service
    sudo cp caddy/caddy.tmpfiles /usr/lib/tmpfiles.d/caddy.conf
    sudo cp caddy/caddy.sysusers /usr/lib/sysusers.d/caddy.conf
    ```

4. Create the user and folders necessary for caddy
    ```bash
    sudo systemd-sysusers /usr/lib/sysusers.d/caddy.conf
    sudo systemd-tmpfiles --create /usr/lib/tmpfiles.d/caddy.conf 
    ```

5. Create the caddy configuration file and folder
    ```bash
    sudo mkdir -p /etc/caddy/config.d
    sudo cp caddy/Caddyfile /etc/caddy/Caddyfile
    sudo cp caddy/subsonic.conf /etc/caddy/config.d/subsonic.conf
    ```

6. Start caddy
    ```bash
    sudo systemctl enable caddy
    sudo systemctl start caddy
    ```

## Inspired by
- https://github.com/svicknesh/foundryvtt-docker-deploy
- https://github.com/gzurowski/docker-subsonic
