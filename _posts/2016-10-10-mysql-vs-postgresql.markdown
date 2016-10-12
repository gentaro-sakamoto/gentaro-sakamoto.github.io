---
published: true
title: MySQL VS PostgreSQL
layout: post
---

### Background
In a couple months, I had been using MySQL database in a project. From October, I've changed the project which has been using PostgreSQL. It's less famliar to me than MySQL. So this time, I'm going to organaize the differences between MySQL and PostgreSQL.

### Past image to the both databases

> MySQL has long been assumed to be the faster but less full-featured of the two database systems, while PostgreSQL was assumed to be a more densely featured database system often described as an open-source version of Oracle. MySQL has been popular among various software projects because of its speed and ease of use, while PostgreSQL has had a close following from developers who come from an Oracle or SQL Server background.

### Basic commands	

|Action|MySQL| PostgreSQL|
|---|---| ---|
|Connecti with database|mysql -uroot -h| psql -Upostgres|
|Show databases|Show databases| \l|
|Use database|\u| \c|
|Show tables|show tables;| \dt|
|Display selected database|select database();| select current_database();|
|Show process list |show processlist| SELECT * from pg_stat_activity|
|Show table schema |desc table| \d+ table|
|Show user list|SELECT User, Host FROM mysql.user;| \du|
|Logout|\q| \q|



### References
- [MySQL vs PostgreSQL](https://www.wikivs.com/wiki/MySQL_vs_PostgreSQL)
- [MySQL and Postgres command equivalents (mysql vs psql)](http://blog.endpoint.com/2009/12/mysql-and-postgres-command-equivalents.html)
- [PostgreSQLとMySQLの基本的なコマンドを比較](http://qiita.com/pugiemonn/items/75870ece3c8476bcb1c8)
