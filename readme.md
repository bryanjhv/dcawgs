# Docker Compose Automated Web+Gen+SSL

Script for automatic Nginx+SSL setup with Docker.

## Installation

### Prerequisites

#### Install Docker

These commands are for Ubuntu, taken from [Docker][docker].

```bash
curl -sSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -
echo "deb https://download.docker.com/linux/ubuntu/ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install -y docker-ce
sudo systemctl enable --now docker
```

#### Install Docker Compose

Taken from [Docker Compose][compose].

```bash
sudo curl -sSL "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### This project

```bash
cd "$(mktemp -d)"
curl -sSL https://github.com/bryanjhv/dcawgs/archive/master.tar.gz -o dcawgs.tar.gz
tar xfz dcawgs.tar.gz
cd dcawgs-master
find -name .gitkeep -delete
curl -sSL https://raw.githubusercontent.com/nginx-proxy/nginx-proxy/master/nginx.tmpl -o src/data/nginx.tmpl
sudo chown -R root:root src
sudo mv src /root/dcawgs
sudo docker network create webgen
sudo docker network create services
sudo docker-compose -f /root/dcawgs/docker-compose.yml up -d
```

## Usage

This is just an example for running Apache:

```yaml
version: "3"

services:
  own-web:
    image: httpd:alpine
    restart: always
    networks:
      - outside
    environment:
      VIRTUAL_HOST: mydomain.com
      LETSENCRYPT_HOST: mydomain.com

networks:
  outside:
    external:
      name: services
```

## Learn more

- [nginx-proxy][proxy] documentation includes a lot of advanced topics.
- The excellent [repository][companion] which served as the base for this one.

## License

This project is released under the [MIT license](license.txt).

[docker]: https://docs.docker.com/engine/install/ubuntu/
[compose]: https://docs.docker.com/compose/install/
[proxy]: https://github.com/nginx-proxy/nginx-proxy#docker-compose
[companion]:
  https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion
