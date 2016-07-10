---
published: false
title: How To Set Up Web Instance On Ubuntu14.04
layout: post
---
### Prerequisites
- It will take 2 hours.

### Check OS Version

```
> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.3 LTS
Release:	14.04
Codename:	trusty
```

### Initial server setup

#### Setup password for root user

```
root@ip-172-30-1-213:~# sudo passwd root
sudo: unable to resolve host ip-172-30-1-213
Enter new UNIX password:
Retype new UNIX password:
```

#### Setup timezone

```
timedatectl set-timezone Asia/Tokyo
root@ip-172-30-1-213:/home/ubuntu# timedatectl
      Local time: Sun 2016-07-10 13:22:54 JST
  Universal time: Sun 2016-07-10 04:22:54 UTC
        RTC time: Sun 2016-07-10 04:22:53
        Timezone: Asia/Tokyo (JST, +0900)
     NTP enabled: yes
NTP synchronized: no
 RTC in local TZ: no
      DST active: n/a
```

#### Create sudo user

```
root@ip-172-30-1-213:/home/ubuntu# adduser dev
Adding user `dev' ...
Adding new group `dev' (1001) ...
Adding new user `dev' (1001) with group `dev' ...
Creating home directory `/home/dev' ...
Copying files from `/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Changing the user information for dev
Enter the new value, or press ENTER for the default
	Full Name []:
	Room Number []:
	Work Phone []:
	Home Phone []:
	Other []:
	
root@ip-172-30-1-213:/home/ubuntu# usermod -aG sudo dev

```

#### Use vim instead of nano

```
dev@ip-172-30-1-213:~$ sudo update-alternatives --config editor
There are 4 choices for the alternative editor (providing /usr/bin/editor).

  Selection    Path                Priority   Status
------------------------------------------------------------
* 0            /bin/nano            40        auto mode
  1            /bin/ed             -100       manual mode
  2            /bin/nano            40        manual mode
  3            /usr/bin/vim.basic   30        manual mode
  4            /usr/bin/vim.tiny    10        manual mode

Press enter to keep the current choice[*], or type selection number: 3
update-alternatives: using /usr/bin/vim.basic to provide /usr/bin/editor (editor) in manual mode
```

#### Setup .bashrc

```
/bin/rm -f ~/.bashrc* && cd ~ && wget http://mindnugget.com/bashrc/.bashrc && wget http://mindnugget.com/bashrc/.bashrc_help

(Sun Jul-7 4:16:20am)-(CPU 0.0%:0:Net 3)-(ubuntu@ip-172-30-1-213:~)-(64K:8)
```

#### Allow dev to SSH to your EC2 instance
```
mkdir ~/.ssh
chmod 700 .ssh
chown dev:dev .ssh
sudo cat /home/ubuntu/.ssh/authorized_keys > .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
chown dev:dev .ssh/authorized_keys

```

##### Install basic tools
```
sudo apt-get update
sudo apt-get install build-essential libssl-dev libyaml-dev libreadline-dev openssl curl git-core zlib1g-dev bison libxml2-dev libxslt1-dev libcurl4-openssl-dev libsqlite3-dev sqlite3
```

### Setup web instance

#### Setup RVM

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable --ruby
source ~/.rvm/scripts/rvm
> rvm -v
rvm 1.27.0 (latest) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]
```

#### Install Ruby 2.1
```
> rvm install 2.1
> ruby -v
ruby 2.1.8p440 (2015-12-16 revision 53160) [x86_64-linux]
rvm use 2.1 --default
```

#### Setup Apache
```
sudo apt-get install apache2
```

#### Setup Passenger
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7

echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main' | sudo tee /etc/apt/sources.list.d/passenger.list
sudo chown root: /etc/apt/sources.list.d/passenger.list
sudo chmod 600 /etc/apt/sources.list.d/passenger.list
sudo apt-get update
sudo apt-get install libapache2-mod-passenger

> sudo a2enmod passenger
Module passenger already enabled

sudo service apache2 restart

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/unique.conf

#TBC
```

#### Setup MySQL Client
#### Install ImageMagick

### Create AMI

### References
- [Ultimate Prompt and bashrc. File](http://www.linuxquestions.org/questions/linux-general-1/ultimate-prompt-and-bashrc-file-4175518169/)
- [システムのタイムゾーンを設定する](https://www.server-world.info/query?os=Ubuntu_14.04&p=timezone)
- [How To Create a Sudo User on Ubuntu ](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
- [How to change visudo editor from nano to vim? ](http://askubuntu.com/questions/539243/how-to-change-visudo-editor-from-nano-to-vim)
- [How to add new users to EC2 and give SSH Key access](http://www.ampedupdesigns.com/blog/show?bid=44)
- [How To Deploy a Rails App with Passenger and Apache on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-passenger-and-apache-on-ubuntu-14-04)
- [sudoでリダイレクトをしたいとき](http://yut.hatenablog.com/entry/20111013/1318436872)
