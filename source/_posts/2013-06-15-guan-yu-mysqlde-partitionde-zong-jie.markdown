---
layout: post
title: "关于MySQL的Partition的总结"
date: 2013-06-15 16:53
comments: true
categories: 
- MySQL
---

检查你的MySQL是否支持partition
{% codeblock demo.sql %}
SHOW VARIABLES LIKE '%partition%';
{% endcodeblock %}

MySQL支持RANGE，LIST，HASH，KEY分区类型。
创建以RANGE方式的分区：
{% codeblock demo.sql %}
CREATE TABLE foo (
id INT NOT NULL AUTO_INCREMENT,
created DATETIME,
PRIMARY KEY(id, created)
) ENGINE=INNODB PARTITION BY RANGE (TO_DAYS(created)) (
PARTITION foo_1 VALUES LESS THAN (TO_DAYS('2009-01-01')),
PARTITION foo_2 VALUES LESS THAN (TO_DAYS('2010-01-01'))
)
{% endcodeblock %}
<!--more-->
创建完分区之后可以管理
{% codeblock demo.sql %}
# 添加分区
ALTER TABLE foo ADD PARTITION (
PARTITION foo_3 VALUES LESS THAN (TO_DAYS('2011-01-01'))
);

# 删除分区
ALTER TABLE FOO DROP PARTITION foo_3;

{% endcodeblock%}

MySQL会把数据保存到不同的数据文件里，同时索引也是分区的，相对未分区的表来说，分区后单独的数据文件和索引文件的大小都明显降低，效率则明显提升。

**重要提示：**使用分区功能之后，相关查询最好都用EXPLAIN PARTITIONS过一遍，确认分区是否生效。

到底应该采用哪种分区类型呢？通常来说使用range类型是个不错的选择，不过也不尽然，比如说在主从结构中，
主服务器由于很少使用SELECT查询，所以在主服务器上使用range类型的分区通常并没有太大意义，
此时使用hash类型的分区相对更好一些，假设使用PARTITION BY HASH(id) PARTITIONS 10，那么当插入新数据时，
会根据id把数据平均分散到各个分区上，由于文件小，所以效率高，更新操作会变得更快。

到底应该按哪个字段来分区呢？通常来说按时间字段分区是个不错的选择，不过还是应该按需求而定，
通常有很多种划分应用的方式，比如说按时间，或者按用户，哪种用的多，就选哪种来分区。如果使用主从结构的话，
还可能用的更灵活些，有的从服务器使用时间分区，有的从服务器使用用户分区，不过如此一来，当执行查询时，
程序里应该负责选择正确的从服务器去查询，写个MySQL Proxy脚本应该可以透明实现。

**使用限制：**
主键或者唯一索引必须包含分区字段：如`PRIMARY KEY(id, created)`，不过对INNODB来说，大主键不爽。
很多时候，使用了分区就不要再使用主键，否则可能影响性能。
只能通过int类型的字段或者返回int类型的表达式来分区：通常使用`YEAR`或`TO_DAYS`等函数。
每个表最多1024个分区：不可能无限制的扩展分区，而且过度使用分区往往会消耗大量系统内存。
采用分区的表不支持外键：相关的约束逻辑必须通过程序来实现。

