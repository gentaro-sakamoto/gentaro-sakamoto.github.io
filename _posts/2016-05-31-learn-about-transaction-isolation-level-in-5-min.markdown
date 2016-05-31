---
published: true
title: Learn About Transaction Isolation Level In 5 Min
layout: post
---
### Transaction Isolation Level

| Level(priority) | Type | Trait | 
| :-------------: | :-------------: | ------------- |
| 4 | Serializable	| All read and write locks is released at the end of the transaction. Also, *range-locks* must be acquired.  | 
| 3 | Repeatable reads	| *fuzzy reads* is not allowed. One transaction can't see the committed change made by the other transactions untile the end of the transaction. But *range locks* are not managed where *phantom reads* come in.|
| 2 | Read committed	| *dirty reads* is not allowed. one transaction can't see not-yet-committed change made by other transactions. But the transaction can see the change after the other transactions committed. It's called *fuzzy reads* or *non-repetable reads* |
| 1 | Read uncommitted	| *dirty reads* is allowed so one transaction can see not-yet-committed change made by other transactions.|


### References
- [15.3.4 Phantom Rows](http://dev.mysql.com/doc/refman/5.7/en/innodb-next-key-locking.html)
- [MySQL locking for the busy web developer](https://www.brightbox.com/blog/2013/10/31/on-mysql-locks/)
- [2009-01-12 MySQL InnoDBのネクストキーロック おさらい](http://d.hatena.ne.jp/sh2/20090112)
- [MySQLでトランザクションの4つの分離レベルを試す](http://d.hatena.ne.jp/fat47/20140212/1392171784)
- [MySQLのインデックスによるロックの違いを確認してみる](http://d.hatena.ne.jp/yk5656/20140720/1406238608)
- [How to find out who is locking a table in MySQL](http://www.xaprb.com/blog/2006/07/31/how-to-analyze-innodb-mysql-locks/)
- [14.2.11 デッドロックの対処方法](https://dev.mysql.com/doc/refman/5.6/ja/innodb-deadlocks.html)
