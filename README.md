# Docker-Nextcloud
How to create a Docker container with Nextcloud

## Index

1. [What's NextCloud](#what)
2. [How to install NextCloud](#install)
3. [How to access NextCloud](#acess)
4. [References](#references)

<a name="what"></a>
## 1. What's Nextcloud?

Nextcloud is a suite of client-server software for creating and using file hosting services. Nextcloud is free and open-source, which means that anyone is allowed to install and operate it on their own private server devices.

With the integrated OnlyOffice, Nextcloud application functionally is similar to Dropbox, Office 365 or Google Drive, but can be used on home-local computers or for off-premises file storage hosting.

The original ownCloud developer Frank Karlitschek forked ownCloud and created Nextcloud, which continues to be actively developed by Karlitschek and other members of the original ownCloud team.

<a name="install"></a>
## 2.- How to install NextCloud

1.- First step is deploying the container. The image's name is nextcloud. I'll be using 8087 port for this guide, but you can use another one without any problems:

```
version: '3'
services:
 mi-nextcloud:
  image: nextcloud
  ports:
  - "8087:80"
```

<a name="access"></a>
## 3.- How to access NextCloud

2.- Connect to nextcloud with localhost:[Port] to access your nextcloud container. The first thing we're going to do is create an admin user and install it:

![/images/4a.png](/images/4a.png)

3.- Nextcloud may take some time to complete the installation. If shouldn't take too long:

![/images/4.png](/images/4b.png)

4.- Once the install has been completed we'll be in nextcloud main page and it'll be ready to use:

![/images/4c.png](/images/4c.png)

<a name="references"></a>
## 3.- References

- [Wikipedia: NextCloud](https://en.wikipedia.org/wiki/Nextcloud)
- [NextCloud's Official Website](https://nextcloud.com/)
