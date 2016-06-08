---
published: true
title: How To Install Docker and Docker Compose on CentOS 7.2
layout: post
---
Check your OS version

```
[user@instance ~]$ cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
```

### Check Your Current Kernel Version
> Docker requires a 64-bit installation regardless of your CentOS version. Also, your kernel must be 3.10 at minimum, which CentOS 7 runs.

```
[user@instance ~]$ uname -r
3.10.0-327.10.1.el7.x86_64
```

### Install Docker
Add the yum repo.

```
sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/$releasever/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

Install the docker package

```
sudo yum install docker-engine
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
sudo service docker start
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