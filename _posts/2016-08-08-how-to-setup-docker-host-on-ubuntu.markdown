---
published: false
title: How To Deploy Rails App With AWS CodeDeploy
layout: post
---

### Prerequisites
- You have done with initial server setup mentioned in [the artcle](http://gentaro-sakamoto.github.io/2016/07/10/how-to-set-up-web-instance-on-ubuntu14-04.html)

### Installation on Ubuntu

```
> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.3 LTS
Release:	14.04
Codename:	trusty

> uname -r
3.13.0-74-generic
```

Update package information.

```
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
vi /etc/apt/sources.list.d/docker.list
deb https://apt.dockerproject.org/repo ubuntu-trusty main
sudo apt-get update
sudo apt-get purge lxc-docker
apt-cache policy docker-engine
sudo apt-get install linux-image-extra-$(uname -r)
sudo apt-get install apparmor
```

Install Docker

```
sudo apt-get install docker-engine
sudo service docker start
sudo docker run hello-world
```

Create Docker Group

```
sudo groupadd docker
sudo usermod -aG docker dev
docker run hello-world
> docker -v
Docker version 1.12.0, build 8eab29e
```

Install DockerCompose

```
sudo -i
curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
> docker-compose --version
docker-compose version 1.8.0, build f3628c7
```


### Reference
- [Installation on Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
- [Installation Docker Compose on Ubuntu](https://docs.docker.com/compose/install/)