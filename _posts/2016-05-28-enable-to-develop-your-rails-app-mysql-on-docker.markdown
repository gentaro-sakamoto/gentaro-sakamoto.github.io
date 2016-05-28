---
published: true
title: Enable to develop your Rails app(MySQL) on Docker.
layout: post
---
### Prerequisite
- Docker Machine 
  
```
dev-mac:unique gentaro$ docker-machine -v
docker-machine version 0.7.0, build a650a40
```

- Docker Compose

```
dev-mac:unique gentaro$ docker-compose -v
docker-compose version 1.7.1, build 0a9ab35

```

### Prepare your docker machine

```
dev-mac:unique gentaro$ docker-machine create --driver virtualbox default
Creating CA: /Users/sakamotogentarou/.docker/machine/certs/ca.pem
Creating client certificate: /Users/sakamotogentarou/.docker/machine/certs/cert.pem
Running pre-create checks...
Creating machine...
(default) Copying /Users/sakamotogentarou/.docker/machine/cache/boot2docker.iso to /Users/sakamotogentarou/.docker/machine/machines/default/boot2docker.iso...
(default) Creating VirtualBox VM...
(default) Creating SSH key...
(default) Starting the VM...
(default) Check network to re-create if needed...
(default) Found a new host-only adapter: "vboxnet2"
(default) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env default

```


### Check docker machine running

```
dev-mac:unique gentaro$ docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default   *        virtualbox   Running   tcp://192.168.99.100:2376           v1.11.1
ruby-2.2.3(feature/120479181_enable_to_develop_on_docker)
```

### Check docker machine environment variables

```
dev-mac:unique gentaro$ docker-machine env default
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/sakamotogentarou/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval $(docker-machine env default)
ruby-2.2.3(feature/120479181_enable_to_develop_on_docker)
```

### Setup the env variables

Add the comand below to your .bashrc

```
eval "$(docker-machine env default)"

```

### Configure your Dockerfile

```
dev-mac:unique gentaro$ cat Dockerfile
FROM ruby:2.2.3
RUN apt-get update -qq && apt-get install -y build-essential imagemagick nodejs
RUN mkdir /unique
WORKDIR /unique
ADD Gemfile /unique/Gemfile
ADD Gemfile.lock /unique/Gemfile.lock
RUN bundle install
ADD . /unique

```

### Configure your docker-compose.yml

```
dev-mac:unique gentaro$ cat docker-compose.yml
version: '2'
services:
  db:
    image: mysql
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: "yes"
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/unique
    ports:
      - "3000:3000"
    depends_on:
      - db
```

### Configure database.yml to use db on docker


```
dev-mac:unique gentaro$ cat config/database.yml
development: &development
  adapter: mysql2
  encoding: utf8
  reconnect: false
  database: unique_development
  pool: 5
  host: db
  socket: /var/run/mysqld/mysqld.sock

test:
  <<: *development
  database: unique_test

```

### Run web and db with docker-compose

```
dev-mac:unique gentaro$ docker-compose up
Starting unique_db_1
Starting unique_web_1
Attaching to unique_db_1, unique_web_1

・・・・・・・・・・
```

### Prepare your rails app db

```
dev-mac:unique gentaro$ docker-compose run web rake db:create
dev-mac:unique gentaro$ docker-compose run web rake db:migrate
dev-mac:unique gentaro$ docker-compose run web rake db:seed
```

### Let's visit your the app on docker

```
http://192.168.99.100:3000/
```