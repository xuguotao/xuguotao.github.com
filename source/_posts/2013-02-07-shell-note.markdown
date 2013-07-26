---
layout: post
title: "读linux shell脚本攻略--笔记"
date: 2013-02-07 10:30
comments: true
categories: [shell,]
---
#### Base
+ `echo` 和 `printf` 的区别：前者有自动换行，而后者需要手动添加
+ 常用的环境变量：`PATH` `HOME` `PWD` `USER` `UID` `SHELL`
+ 获取字符串长度：`echo ${#var}`
+ 识别当前的shell版本：`echo $SHELL` 或者 `echo $0`

<!--more-->
#### Doing math calculation
{% codeblock test.sh%}
let rs=no1+no2
$[ no1 + no2 ]
$(( no1 + no2 ))
\`expr 3 + 5\`
$[expr 3 + 5]
echo "4 * 0.43" | bc
{% endcodeblock %}
> 注意：当使用`expr`的时候 操作符左右一定要有空格，否则不能做预算，现在我也不知道为什么。。。

#### Calculation-bc
{% codeblock test.sh%}
echo "scale=5;3/8" | bc
no=15
echo "obase=2;$no" | bc
no=100
echo "obase=10;ibase=2;$no" | bc
echo "scale=10;sqrt(5)" | bc
echo "10^10" | bc
{% endcodeblock %}
