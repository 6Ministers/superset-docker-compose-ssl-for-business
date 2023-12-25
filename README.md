# Installing SuperSet+Caddy+SSL with docker-compose.

## Quick Installation

**Before starting the instance, direct the domain (subdomain) to the ip of the server where Superset will be installed!**

## 1. SuperSet Server Requirements
From and more
- 2 CPUs
- 3 RAM 
- 10 Gb 

Run for Ubuntu 22.04

``` bash
sudo apt-get purge needrestart
```

Install docker and docker-compose:

``` bash
curl -s https://raw.githubusercontent.com/6Ministers/superset-docker-compose-ssl-for-business/master/setup.sh | sudo bash -s
```

Clone Superset's repo in your terminal with the following command:

``` bash
git clone https://github.com/apache/superset.git
```

Navigate to the folder you created `superset`

``` bash
cd superset
```

Create the `Caddyfile`with the entry listed below


To change the domain in the `Caddyfile` to your own

``` bash
https://subdomain.your-domain:443 {
    header Strict-Transport-Security max-age=31536000;
    reverse_proxy 127.0.0.1:8088
#    tls admin@example.org
    encode zstd gzip
}
```

Add in `docker-compose-non-dev.yml`

``` bash
services:
  caddy:
    image: caddy:alpine
    restart: unless-stopped
    container_name: caddy
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./certs:/certs
      - ./config:/config
      - ./data:/data
      - ./sites:/srv
    network_mode: "host"
```

![Screenshot_77](https://github.com/6Ministers/superset-docker-compose-ssl-for-business/assets/11208423/e59180c6-4245-4f88-a45e-83f44f068297)


Create the code with the command
``` bash
openssl rand -base64 42
```

Paste it in the file  `superset/docker/.env-non-dev` instead `TEST_NON_DEV_SECRET`

``` bash
SUPERSET_SECRET_KEY=TEST_NON_DEV_SECRET
```
![Screenshot_78](https://github.com/6Ministers/superset-docker-compose-ssl-for-business/assets/11208423/7be40b6b-18dd-4eeb-9c5f-fc3c9b4242d8)


**Run SuperSet:**

Run a specific version of Superset by first checking out the branch/tag, and then starting docker compose with the TAG variable. For example, to run the 3.1.0 version, run the following commands on Linux-based systems:

``` bash
git checkout 3.1
docker compose -f docker-compose-non-dev.yml pull
docker compose -f docker-compose-non-dev.yml up
```

Then open `https://superset.domain.com:` to access SuperSet

![Screenshot_79](https://github.com/6Ministers/superset-docker-compose-ssl-for-business/assets/11208423/6fc9c4b3-e02a-403c-b1b0-14f7cc0d3fec)



Log in with the default username and password:

``` bash
username: admin
```
``` bash
password: admin
```

## SuperSet container management

**Run SuperSet**:

``` bash
docker-compose-non-dev up -d
```

**Restart**:

``` bash
docker-compose-non-dev up restart
```

**Restart**:

``` bash
sudo docker-compose-non-dev down && sudo docker-compose-non-dev up -d
```

**Stop**:

``` bash
docker-compose-non-dev down
```

## Documentation
https://superset.apache.org/docs/installation/installing-superset-using-docker-compose/

https://github.com/apache/superset/tree/master

