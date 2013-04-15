---
layout: post
title: "Shell Note"
date: 2013-02-07 10:30
comments: true
categories: [shell,]
---

## 读linux shell脚本攻略--笔记


## 2013-03-04 Note

#### echo 彩色字
{% codeblock test.sh%}
echo -e "\033[1;42m this is a test \033[0m";
{% endcodeblock%}
文本终端的颜色可以使用“ANSI非常规字符序列”来生成。  
-e : 用来激活特俗字符的解释器。  
\033 : 引导非常规字符序列。有些资料写可以用\e来代替。但是我在mac的终端下不起作用。不知道为什么。  
m : 设置属性然后结束非常规字符序列。  
<!--more-->
+ 编码 颜色/动作
+ 0 重新设置属性到缺省设置
+ 1 设置粗体
+ 2 设置一半亮度（模拟彩色显示器的颜色）
+ 4 设置下划线（模拟彩色显示器的颜色）
+ 5 设置闪烁
+ 7 设置反向图象
+ 22 设置一般密度
+ 24 关闭下划线
+ 25 关闭闪烁
+ 27 关闭反向图象
+ 30 设置黑色前景
+ 31 设置红色前景
+ 32 设置绿色前景
+ 33 设置棕色前景
+ 34 设置蓝色前景
+ 35 设置紫色前景
+ 36 设置青色前景
+ 37 设置白色前景
+ 38 在缺省的前景颜色上设置下划线
+ 39 在缺省的前景颜色上关闭下划线
+ 40 设置黑色背景
+ 41 设置红色背景
+ 42 设置绿色背景
+ 43 设置棕色背景
+ 44 设置蓝色背景
+ 45 设置紫色背景
+ 46 设置青色背景
+ 47 设置白色背景
+ 49 设置缺省黑色背景
+ 其他有趣的代码还有：
+ \033[2J 　清除屏幕
+ \033[0q 　关闭所有的键盘指示灯
+ \033[1q 　设置“滚动锁定”指示灯 (Scroll Lock)
+ \033[2q 　设置“数值锁定”指示灯 (Num Lock)
+ \033[3q 　设置“大写锁定”指示灯 (Caps Lock)
+ \033[15:40H 把关闭移动到第15行，40列
+ \007 发蜂鸣生beep


#### Base
+ `echo` 和 `printf` 的区别：前者有自动换行，而后者需要手动添加
+ 常用的环境变量：`PATH` `HOME` `PWD` `USER` `UID` `SHELL`
+ 获取字符串长度：`echo ${#var}`
+ 识别当前的shell版本：`echo $SHELL` 或者 `echo $0`

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
