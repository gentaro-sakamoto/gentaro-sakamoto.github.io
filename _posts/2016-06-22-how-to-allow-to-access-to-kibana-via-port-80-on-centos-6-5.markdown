---
published: true
title: How To Allow To Access To Kibana Via Port 80 On CentOS 6.5
layout: post
---
# Prerequisite

- CentOS6.5
- Kibana4.5.1

# Install Kibana

Setup yum repository for kibana

```
cat <<EOF > /etc/yum.repos.d/kibana.repo
[kibana-4.5]
name=Kibana repository for 4.5.x packages
baseurl=http://packages.elastic.co/kibana/4.5/centos
gpgcheck=1
gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
enabled=1
EOF
```

Install kibana

```
yum install kibana -y
```

Allow kibana to start automatically

```
chkconfig --add kibana
```

# Setup web server

Install apache and allow it to start automatically

```
yum install httpd -y
chkconfig httpd on
```

Setup reverse proxy

```
APACHE_LOG_DIR=/var/log/httpd
cat <<EOF >> /etc/httpd/conf/httpd.conf
<VirtualHost *:80>
    ProxyRequests Off
    ServerName [The Proxy Server Name]
    ProxyPass / http://127.0.0.1:5601
    ProxyPassReverse / http://127.0.0.1:5601
    RewriteEngine on
    RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
    RewriteRule .* http://127.0.0.1:5601%{REQUEST_URI} [P,QSA]
    ErrorLog ${APACHE_LOG_DIR}/kibana_error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/kibana_access.log combined
</VirtualHost>
EOF
```

Start httpd and kibana

```
service kibana start
service httpd start
```

You can access the kibana via port 80

```
http://[Proxy Server IP or Domain]/
```


# References

- [ELASTICSEARCH KIBANA BETA BEHIND APACHE PROXY](http://mmbash.de/blog/kibana-beta-behind-apache-proxy/)