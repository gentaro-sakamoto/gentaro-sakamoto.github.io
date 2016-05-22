---
published: true
title: Removed Huge Unused Data on My Mac
layout: post
---
In a couple of months, my mac for developing was getting stupid. Especially I was annoyed by tons of crushes of Google Chrome which seems to be caused by lack of disk space. So I decided to go over my disk usage. I used the utility [Disk Inventory](http://www.derlien.com/). According to the disk usage it shows, I removed the huge unused data anymore [lost+found](http://bamka.info/lost-found-file) and Xcode which took the huge disk space. After that, I got huge space. Hope the space could produce creativity!

```
ruby-2.2.3(develop)
dev-mac:unique gentaro$ df -h
Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk1     112Gi   63Gi   48Gi    57% 16680371 12641355   57%   /
devfs          182Ki  182Ki    0Bi   100%      628        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home
```

