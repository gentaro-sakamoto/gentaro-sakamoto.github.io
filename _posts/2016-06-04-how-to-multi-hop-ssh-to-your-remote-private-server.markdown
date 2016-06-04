---
published: true
title: How To Multi-hop SSH To Remote Private Server
layout: post
---
Let's say we have 2 instances on AWS. In terms of SSH access, the one instance is the bastion instance which is public and the other is web instance which is private. You can ssh to the web instance through the bastion. In this context, someone ssh-logins to the web instance with the following consecutive commands.

```
ssh user@bastion-instance
# and then
ssh user@web-instance
```

You know it's bothersome. But you can ssh-login to the web instance with one command by utilising the Multi-hop SSH like as follow.

```
Host bastion.unique
  HostName        [Your Global IP]
  Port            22
  IdentityFile    ~/.ssh/uniqueadmin.pem
  User            root
  ForwardAgent yes
Host develop.unique
  HostName        [Your Private IP]
  Port            22
  IdentityFile    ~/.ssh/uniqueadmin.pem
  User            root
  ProxyCommand    ssh bastion.unique -W %h:%p
```

Now you can ssh-login to ssh the private server with the one comand.

```
dev-mac:.ssh gentaro$ ssh develop.unique
Last login: Sat Jun  4 03:01:12 2016 from 172.30.1.47
     ___   _        __   __   ____            __
    / _ \ (_)___ _ / /  / /_ / __/____ ___ _ / /___
   / , _// // _ `// _ \/ __/_\ \ / __// _ `// // -_)
  /_/|_|/_/ \_, //_//_/\__//___/ \__/ \_,_//_/ \__/
           /___/

Welcome to a virtual machine image brought to you by RightScale!
```

