---
published: true
title: How To Set Up Web Instance On Ubuntu14.04
layout: post
---
---
published: false
title: How To Set Up Web Instance On Ubuntu14.04
layout: post
---
### Prerequisites
- It will take 2 hours.
- Git 1.9.1
- RVM 1.27
- Ruby 2.2.3
- Ubuntu 14.04.03
- Rails 4.2
- Apache
- Passenger
- MySQL6.5
- ImageMagick 


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

#### Install Ruby 2.2.3
```
> rvm install 2.2.3
> ruby -v
ruby 2.2.3p173 (2015-08-18 revision 51636) [x86_64-linux]
rvm use 2.2.3 --default
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
vi /etc/apache2/sites-available/unique.conf
```

*/etc/apache2/sites-available/unique.conf*

```
<VirtualHost *:80>
    ServerName 52.69.41.184
    ServerAlias 52.69.41.184
    ServerAdmin dev@localhost
    DocumentRoot /var/www/unique/public
    RailsEnv production
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <Directory "/var/www/unique/public">
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```

Setup your code

```
mkdir -p /var/www/
sudo chown dev:dev /var/www

> git clone https://github.com/gentaro-sakamoto/unique.git
Cloning into 'unique'...
remote: Counting objects: 1929, done.
remote: Compressing objects: 100% (37/37), done.
remote: Total 1929 (delta 17), reused 7 (delta 7), pack-reused 1885
Receiving objects: 100% (1929/1929), 6.23 MiB | 2.00 MiB/s, done.
Resolving deltas: 100% (422/422), done.
Checking connectivity... done.
```

Disable default site, enable my new site, and restart Apacche.

```
sudo a2dissite 000-default
sudo a2ensite unique
sudo service apache2 restart
```

#### Setup MySQL Client

```
sudo apt-get install mysql-client-5.6
sudo apt-get install libmysqld-dev 
```

#### Install ImageMagick
```
sudo apt-get install imagemagick libmagickwand-dev
```

#### Install libxml2

```
sudo apt-get install ruby-dev zlib1g-dev liblzma-dev
```

#### Setup Rails App

Install gems

```
gem install bundler
bundle config build.nokogiri --use-system-libraries
bundle install --without=development test
```

Check your database.yml and secrets.yml deployed successfully

```
less config/secrets.yml
less config/database.yml
```

Setup database

```
bundle exec rake db:setup RAILS_ENV=production
```

Asset precompile

```
bundle exec rake assets:precompile RAILS_ENV=production
```

#### That's all
You can visit your page like this.

```
http://52.69.41.184/user/
```

#### Create AMI

Just in case you terminate the web instance, you'd better create the AMI of it.


### References
- [Ultimate Prompt and bashrc. File](http://www.linuxquestions.org/questions/linux-general-1/ultimate-prompt-and-bashrc-file-4175518169/)
- [システムのタイムゾーンを設定する](https://www.server-world.info/query?os=Ubuntu_14.04&p=timezone)
- [How To Create a Sudo User on Ubuntu ](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
- [How to change visudo editor from nano to vim? ](http://askubuntu.com/questions/539243/how-to-change-visudo-editor-from-nano-to-vim)
- [How to add new users to EC2 and give SSH Key access](http://www.ampedupdesigns.com/blog/show?bid=44)
- [How To Deploy a Rails App with Passenger and Apache on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-passenger-and-apache-on-ubuntu-14-04)
- [sudoでリダイレクトをしたいとき](http://yut.hatenablog.com/entry/20111013/1318436872)