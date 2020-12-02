# Docker-Nextcloud
How to create a Docker container with Nextcloud

## Index

1. [What's NextCloud](#what)
2. [How to install NextCloud](#install)
3. [References](#references)

<a name="what"></a>
## 1. What's Nextcloud?

Nextcloud is a suite of client-server software for creating and using file hosting services. Nextcloud is free and open-source, which means that anyone is allowed to install and operate it on their own private server devices.

With the integrated OnlyOffice, Nextcloud application functionally is similar to Dropbox, Office 365 or Google Drive, but can be used on home-local computers or for off-premises file storage hosting.

The original ownCloud developer Frank Karlitschek forked ownCloud and created Nextcloud, which continues to be actively developed by Karlitschek and other members of the original ownCloud team.

<a name="install"></a>
## 2.- How to install NextCloud

1.- 

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

2.- 

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

3.- 

```
sudo apt update
```

4.- 

```
sudo apt install docker-ce
```

5.-

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ docker-compose --version
docker-compose version 1.25.4, build 8d51620a
```

6.-

```
$ docker network create nextcloud_network
```

7.-

```
$ vi docker-compose.yml
```

8.-

```
version: '3' 

services:

  proxy:
    image: jwilder/nginx-proxy:alpine
    labels:
      - "com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy=true"
    container_name: nextcloud-proxy
    networks:
      - nextcloud_network
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./proxy/conf.d:/etc/nginx/conf.d:rw
      - ./proxy/vhost.d:/etc/nginx/vhost.d:rw
      - ./proxy/html:/usr/share/nginx/html:rw
      - ./proxy/certs:/etc/nginx/certs:ro
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/tmp/docker.sock:ro
    restart: unless-stopped
  
  letsencrypt:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: nextcloud-letsencrypt
    depends_on:
      - proxy
    networks:
      - nextcloud_network
    volumes:
      - ./proxy/certs:/etc/nginx/certs:rw
      - ./proxy/vhost.d:/etc/nginx/vhost.d:rw
      - ./proxy/html:/usr/share/nginx/html:rw
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
    restart: unless-stopped

  db:
    image: mariadb
    container_name: nextcloud-mariadb
    networks:
      - nextcloud_network
    volumes:
      - db:/var/lib/mysql
      - /etc/localtime:/etc/localtime:ro
    environment:
      - MYSQL_ROOT_PASSWORD=toor
      - MYSQL_PASSWORD=mysql
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
    restart: unless-stopped
  
  app:
    image: nextcloud:latest
    container_name: nextcloud-app
    networks:
      - nextcloud_network
    depends_on:
      - letsencrypt
      - proxy
      - db
    volumes:
      - nextcloud:/var/www/html
      - ./app/config:/var/www/html/config
      - ./app/custom_apps:/var/www/html/custom_apps
      - ./app/data:/var/www/html/data
      - ./app/themes:/var/www/html/themes
      - /etc/localtime:/etc/localtime:ro
    environment:
      - VIRTUAL_HOST=nextcloud.YOUR-DOMAIN
      - LETSENCRYPT_HOST=nextcloud.YOUR-DOMAIN
      - LETSENCRYPT_EMAIL=YOUR-EMAIL
    restart: unless-stopped

volumes:
  nextcloud:
  db:

networks:
  nextcloud_network:
```

9.-

```
docker-compose up -d
```

10.-

```

```

<a name="references"></a>
## 3.- References

- [Wikipedia: NextCloud](https://en.wikipedia.org/wiki/Nextcloud)
- [NextCloud's Official Website](https://nextcloud.com/)
