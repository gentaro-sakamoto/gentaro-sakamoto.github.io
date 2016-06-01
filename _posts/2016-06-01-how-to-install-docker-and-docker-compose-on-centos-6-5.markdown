---
published: true
title: How To Install Docker and Docker Compose On CentOS 6.5
layout: post
---
How To Install Docker and Docker Compose on CentOS 6.5

### Check Your Current Kernel Version
> Docker requires a 64-bit installation regardless of your CentOS version. Also, your kernel must be 3.10 at minimum, which CentOS 7 runs.

```
[user@instance ~]$ uname -r
2.6.32-431.29.2.el6.x86_64
```

My kernel was quite old. So I've updated the kernel to the following version following the article [Install Kernel 3.10 on CentOS 6.5](http://bicofino.io/2014/10/25/install-kernel-3-dot-10-on-centos-6-dot-5/).

Then now we have the kernel version 3.10.

```
[user@instance ~]$ uname -r
3.10.101-1.el6.elrepo.x86_64
```

### To Allow Non-root User To Use Docker

If you don't have docker group, let's create the docker group.

```
sudo groupadd docker
```

And add your user to the docker group

```
sudo usermod -a -G docker your_username
```

Then you can run docker without sudo

```
docker run hello-world
```

### To Ensure Docker Starts When You Boot Your System

```
sudo chkconfig docker on
```

### To Install Docker Compose

Download the Docker Compose binary

```
curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

Apply executable permissions to the binary

```
chmod +x /usr/local/bin/docker-compose
```

Verify the installation

```
docker-compose --version
```

# References
- [Installation on CentOS](https://docs.docker.com/engine/installation/linux/centos/)
- [Install Docker Compose](https://docs.docker.com/compose/install/)