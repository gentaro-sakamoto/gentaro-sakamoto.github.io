---
published: true
title: How To Make Sure If HTTP Network Is Active
layout: post
---
Can you access to the httpd server via port 80? We need to make sure if the httpd is running.

####  From localhost
```
echo -en "GET / HTTP/1.1\n\n" | nc localhost 80
```

####  From remote
```
echo -en "GET / HTTP/1.1\n\n" | nc [public_ip] 80
```

Can you make a connection with the server via 80?

```
nc -zv localhost [public_ip]
```

### References
- [How to find out if httpd is running or not via command line?](http://unix.stackexchange.com/questions/135015/how-to-find-out-if-httpd-is-running-or-not-via-command-line)
- [nc コマンド 使い方メモ](http://qiita.com/yasuhiroki/items/d470829ab2e30ee6203f)