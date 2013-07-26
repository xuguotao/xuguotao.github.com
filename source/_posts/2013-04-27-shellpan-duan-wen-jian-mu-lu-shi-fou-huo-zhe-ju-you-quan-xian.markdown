---
layout: post
title: "Shell判断文件目录是否或者具有权限"
date: 2013-04-27 10:11
comments: true
categories: [shell]
---
1. 判断目录是否存在并且是否具有可执行权限  
` if [ -x "$myPath" ]; then `
2. 判断目录是否存在  
` if [ -d "$myPath" ]; then `
3. 判断文件是否存在  
` if [ -f "$myFile" ]; then `
4. 判断一个变量是否有值  
` if [ -n "$myVar" ]; then `
5. 判断两个变量是否相等  
` if [ "$myVar1" = "$myVar2" ]; then `
