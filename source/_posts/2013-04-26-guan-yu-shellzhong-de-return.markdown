---
layout: post
title: "关于Shell中的return"
date: 2013-04-26 11:46
comments: true
categories: [shell]
---

1. shell return 只能返回0-255的数字
2. 当返回0表示真，非0表示假
3. 在script里，用`exit RV`来指定返回值
4. 在function里，用`return RV`来制订返回值
5. 可以用 `$?` 来获取返回值
6. 每一个command在结束的时候都会返回一个 return value,不管你运行什么的命令。
[来源](http://www.5dtao.com) 我的BLOG
