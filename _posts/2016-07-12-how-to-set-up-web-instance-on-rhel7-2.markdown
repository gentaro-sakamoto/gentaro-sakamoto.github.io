---
published: true
title: How To Set Up Web Instance On CentOS7.2
layout: post
---

### Prerequisites
- It will take 2 hours.
- Git 1.8.3.1
- RVM 1.27
- Ruby 2.2.3
- CentOS7.2
- Rails 4.2
- Apache
- Passenger
- MySQL6.5
- ImageMagick


### Check OS Version

```
cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)```

### Initial server setup

#### Setup password for root user

```
sudo passwd root
sudo: unable to resolve host ip-172-30-1-213
Enter new UNIX password:
Retype new UNIX password:
```

#### Create sudo user

```
su - root
adduser dev
passwd dev
Changing password for user dev.
New password:
Retype new password:

gpasswd -a dev wheel
Adding user dev to group wheel

su - dev
Password:
sudo ls -a /root/

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for dev:
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .cshrc  .ssh  .tcshrc

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for dev:
.  ..  anaconda-ks.cfg  .bash_history  .bash_logout  .bash_profile  .bashrc  .cshrc  .ssh  .tcshrc
```

#### Allow dev to SSH to your EC2 instance
```
mkdir ~/.ssh
chmod 700 .ssh
chown dev:dev .ssh
sudo cat /home/centos/.ssh/authorized_keys > ~/.ssh/authorized_keys
chmod 600 .ssh/authorized_keys
chown dev:dev .ssh/authorized_keys
```

#### Allow to ssh with not default port

```
sudo vi /etc/ssh/sshd_config
# Add Port [port num]
Port [port num]

sudo yum -y install policycoreutils\*
sudo semanage port -a -t ssh_port_t -p tcp [port num]
sudo systemctl restart sshd.service
```

#### Delete default ec2-user user

```
sudo userdel centos
sudo rm /home/centos
```

#### Update yum and install basic libraries
```
sudo yum update -yq
sudo yum install -y git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel curl-devel expat-devel gettext-devel openssl-devel zlib-devel
```

#### Setup timezone

```
sudo timedatectl set-timezone Asia/Tokyo
timedatectl
      Local time: 火 2016-07-12 11:52:16 JST
  Universal time: 火 2016-07-12 02:52:16 UTC
        RTC time: 火 2016-07-12 02:52:16
       Time zone: Asia/Tokyo (JST, +0900)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a```

#### Install vim
```
sudo yum install vim
```

#### Install wget
```
sudo yum install wget
```

#### Setup .bashrc

```
/bin/rm -f ~/.bashrc* && cd ~ && wget http://mindnugget.com/bashrc/.bashrc && wget http://mindnugget.com/bashrc/.bashrc_help

```

### Setup web instance

#### Setup RVM

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
sudo curl -sSL https://get.rvm.io | bash -s stable --ruby
source ~/.rvm/scripts/rvm
> rvm -v
rvm 1.27.0 (latest) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]
rvm get stable && rvm reload && rvm repair all
```

#### Install Ruby 2.2.3
```
> rvm install 2.2.3
> ruby -v
ruby 2.2.3p173 (2015-08-18 revision 51636) [x86_64-linux]
rvm use 2.2.3 --default
```

#### Install Apache + Passenger
```
sudo yum install -y epel-release yum-utils -y
sudo yum-config-manager --enable epel -y
sudo curl --fail -sSLo /etc/yum.repos.d/passenger.repo https://oss-binaries.phusionpassenger.com/yum/definitions/el-passenger.repo
sudo yum install -y mod_passenger
sudo systemctl start httpd
sudo systemctl enable httpd
sudo /usr/bin/passenger-config validate-install
sudo /usr/sbin/passenger-memory-stats

sudo chown -R root:dev /var/log/httpd
sudo chmod 775 -R /var/log/httpd
```

#### Setup Passenger
We need to tell Passenger which Ruby command it should use to run your app.

```
> passenger-config about ruby-command
passenger-config was invoked through the following Ruby interpreter:
  Command: /home/dev/.rvm/gems/ruby-2.1.8/wrappers/ruby
  Version: ruby 2.1.8p440 (2015-12-16 revision 53160) [x86_64-linux]
  To use in Apache: PassengerRuby /home/dev/.rvm/gems/ruby-2.1.8/wrappers/ruby
  To use in Nginx : passenger_ruby /home/dev/.rvm/gems/ruby-2.1.8/wrappers/ruby
  To use with Standalone: /home/dev/.rvm/gems/ruby-2.1.8/wrappers/ruby /usr/bin/passenger start


## Notes for RVM users
Do you want to know which command to use for a different Ruby interpreter? 'rvm use' that Ruby interpreter, then re-run 'passenger-config about ruby-command'.
```

*sudo vi /etc/httpd/conf.d/unique.conf*

```
<VirtualHost *:80>
    ServerName Hostname
    ServerAlias Hostname
    ServerAdmin dev@localhost
    DocumentRoot /var/www/unique/public
    RailsEnv production
    PassengerRuby /home/dev/.rvm/gems/ruby-2.1.8/wrappers/ruby
    ErrorLog /var/log/httpd/error.log
    CustomLog /var/log/httpd/access.log combined
    <Directory "/var/www/unique/public">
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>

```
Setup your code

```
sudo chown -R dev:dev /var/www
sudo chmod 775 -R /var/www

> git clone http:://repository_url
```

#### Setup MySQL Client

```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
sudo yum install mysql-server -y
sudo yum install mysql-devel -y
> mysql --version
mysql  Ver 14.14 Distrib 5.6.31, for Linux (x86_64) using  EditLine wrapp
```

#### Install ImageMagick
```
sudo yum install ImageMagick ImageMagick-devel -y
```

#### Install libxml2

```
sudo yum install libxml2-devel libxslt libxslt-devel -y
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
- [How To Add and Delete Users on a CentOS 7 Server](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-a-centos-7-server)
- [CHAPTER 2. CONFIGURING THE DATE AND TIME](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/chap-Configuring_the_Date_and_Time.html#sect-Configuring_the_Date_and_Time-timedatectl-Display)
- [30 Things to Do After Minimal RHEL/CentOS 7 Installation](do-after-minimal-rhel-centos-7-installation/3/)
- [CentOS 7 で ruby on rails 環境構築](https://heavy-metal-explorer.com/aws_centos7_apache_passenger/)
- [Initial Server Setup with CentOS 7](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7)
- [How To Install Ruby on Rails with rbenv on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-centos-7)
- [How To Change OpenSSH Port On CentOS 7](https://www.liberiangeek.net/2014/11/change-openssh-port-centos-7/)
- [Amazon Web Service EC2 instance RedHat yum does not work](http://stackoverflow.com/questions/25099702/amazon-web-service-ec2-instance-redhat-yum-does-not-work)
- [Default ssh Usernames For Connecting To EC2 Instances](https://alestic.com/2014/01/ec2-ssh-username/)
- [How to Install Apache on CentOS 7](http://www.liquidweb.com/kb/how-to-install-apache-on-centos-7/)
- [Deploying a Ruby app on a Linux/Unix production server](https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/apache/oss/el7/deploy_app.html)
