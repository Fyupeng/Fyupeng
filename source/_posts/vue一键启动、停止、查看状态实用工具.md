---
title: vue一键启动、停止、查看状态实用工具
top: false
cover: false
toc: true
mathjax: false
date: 2023-02-01 12:07:59
author:
img:
coverImg:
password:
summary:
tags:
- Java工具
categories:
- 脚本工具
---

# 一、直接给代码
## 1. start.sh
```ruby
#!/bin/bash
function read_dir() {
if [ $# -eq 0 ]
then
	echo "Usage: $0 [DirFile]"
	exit 1
fi

echo "Service is starting...." 


cd ./$1
mkdir logs/ &> /dev/null &
nohup npm run serve >& logs/catalina-$(date +%Y-%m-%d).log &

echo "Service starting succuesful!"
}

read_dir $1

```

## 2. stop.sh
```ruby
# !/bin/bash

function read_dir() {
if [ $# -eq 0 ]
then
	echo "Usage: $0 [DirFile]"
	exit 1
fi

echo "Service is stop...."

if [[ -f $1 ]]
then
	echo "$1 is not a DirFile!"
	exit 1
fi

pid=`ps -ef | grep $1 | grep -v grep | awk '{print $2}'`
if [ -z $pid ]; then
	echo ""
	echo "Service $1 is not running! It's not necessary to stop it!"
	echo ""
else
	kill -9 $pid
	echo ""
	echo "Servuce stop successfuly! pid:${pid} which has been killid forcibly!"
	echo ""
fi
}

read_dir $1

```
## 3.status.sh
```ruby
# !/bin/bash

function read_dir() {
if [ $# -eq 0 ]
then
	echo "Usage: $0 [DirFile]"
	exit 1
fi

if [[ -f $1 ]]
then
	echo "$1 is not a DirFile!"
	exit 1
fi

pid=`ps -ef | grep $1 | grep -v grep | awk '{print $2}'`
if [ -z $pid ]; then
	echo ""
	echo "Service $1 is not running!"
	echo ""
else
	echo ""
	echo "Servuce $1 is running. It's pids=${pid}"
	echo ""
fi
}

read_dir $1

```

# 二、结束语
评论区可留言，可私信，可互相交流学习，共同进步，欢迎各位给出意见或评价，本人致力于做到优质文章，希望能有幸拜读各位的建议！

>专注品质，热爱生活。
交流技术，寻求同志。
—— 嗝屁小孩纸 QQ：1160886967
