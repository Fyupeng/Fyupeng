---
title: nacos一键启动、停止、查看状态脚本
top: false
cover: false
toc: true
mathjax: false
date: 2023-02-28 10:17:50
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
- **使用方法**
```ruby
[root@localhost] #ls
nacos
./start.sh ./nacos -m standalone # 单机模式
./start.sh ./nacos -m cluster # 集群模式
```
- **代码**
```ruby
#!/bin/bash

function read_dir() {
  for file in `ls $1`
  do
    if [[ -d $1"/"$file ]];
      then
        read_dir $1"/"$file $2 $3
    else
      if [[ -f $1"/"$file ]] && [[ "$file" = startup.sh ]];
        then
          cd ./$1
          if [[ "$2" = "-m" ]];
	    then
	      if [[ "$3" = "standalone" ]];
	        then
	          ./startup.sh -m standalone > /dev/null
	          echo $1$file 单机模式启动成功！
		  cd - > /dev/null
	      elif [[ "$3" = "cluster" ]];
	        then
	          ./startup.sh -m cluster > /dev/null
 	          echo $1$file 集群模式启动成功！
		  cd - > /dev/null
	      else
	        echo "1adUsage: ./startup.sh [Directory] -m [standalone | cluster]"
	      fi
	  else
	    echo "2Usage: ./startup.sh [Directory] -m [standalone | cluster]"
	    exit -1;
          fi
      fi
    fi
  done
}

# 读取第一个参数
read_dir $1 $2 $3


```

## 2. status.sh
- **使用方法**
```ruby
[root@localhost] #ls
nacos
./status.sh ./nacos -m standalone # 单机模式
./status.sh ./nacos -m cluster # 集群模式
```
- **代码**
```shell
#!/bin/bash

target_dir=$1

if [[ "$target_dir" = "" ]] || [[ ! -d $target_dir ]];
then
  echo "Usage: ./status.sh [Dir]"
fi

pid=`ps ax | grep -i 'nacos.nacos' | grep ${target_dir} | grep java | grep -v grep | awk '{print $1}'`

if [[ -z "$pid" ]];
then
  echo "No NacosServer running."
 exit -1;
fi

echo "The nacosServer $1"nacos-server.jar"  is running, it's pids as follow:"
echo "$pid"




```
## 3.stop.sh

- **使用方法**
```ruby
[root@localhost] #ls
nacos
./status.sh ./nacos -m standalone # 单机模式
./status.sh ./nacos -m cluster # 集群模式
```
- **代码**
```shell
#!/bin/bash

target_dir=$1

pid=`ps ax | grep -i 'nacos.nacos' | grep ${target_dir} | grep java | grep -v grep | awk '{print $1}'`
if [ -z "$pid" ] ; then
        echo "No nacosServer running."
        exit -1;
fi

echo "The nacosServer $1"nacos-server.jar"  is running, it's pids as follow:"
echo  "$pid"

kill ${pid}

echo "Services stop successfully！ which has been killed forcibly!"
echo pid as follow: 
echo "$pid"

```

# 二、结束语
评论区可留言，可私信，可互相交流学习，共同进步，小生会努力写出优质文章，期待同友多多回访。

>专注品质，热爱生活。
交流技术，寻求同志。