---
layout: post
title: "mysql note"
date: 2013-02-20 14:35
comments: true
categories: [work, mysql]
---
1. Mysql修改权限
{% codeblock demo.sql %}
grant all on db.* to user
flush privileges
{% endcodeblock %}