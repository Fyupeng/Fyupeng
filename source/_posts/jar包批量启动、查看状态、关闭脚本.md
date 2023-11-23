---
title: jar包批量启动、查看状态、关闭脚本
top: false
cover: false
toc: true
mathjax: false
date: 2023-02-01 11:55:30
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
#!/usr/bin/env bash
  
# 配置文件名称
# (该配置文件放置在jar包同级目录下并且必须存在已经配置文件名称具备统一性！！！请根据实际的配置文件名称进行修改)
CONFIG_FILE_NAME="application.properties"

# 启动一个目录下的所有jar包
function read_dir(){
for file in `ls $1`
do
  #如果当前文件是文件夹则递归处理
  if [ -d $1"/"$file ];
  then
    read_dir $1"/"$file
  else
    # 当前文件不是一个文件夹
    if [[ -f $1"/"$file ]];
    then
        # 如果当前文件是一个.jar结尾的文件则启动它
        if [[ ${file:0-4} == '.jar' ]];
        then
	  echo $1/$file 开始启动...
          cd ./$1
          #nohup java -jar $1"/"$file --spring.config.location=$1"/"$CONFIG_FILE_NAME > /dev/null &
 	  if [[ ! -d "./logs/" ]];
	  then  
      	    mkdir "./logs" > /dev/nul
	  fi
   	   nohup java -jar -Dlogging.config="./config/logback.xml" "./"$file > "./logs/catalina.log" &
 	    #nohup java -jar ./$file > ./logs/catalina.log &
            echo $1"/"$file  启动成功!
	    echo ""
            cd - > /dev/null
        fi
    fi
  fi
done
}
#读取第一个参数
read_dir $1

```

## 2. status.sh
```ruby
#!/usr/bin/env bash
# 查看某个目录下所有jar程序的状态
function read_dir(){
for file in `ls $1`
do
  #如果当前文件是文件夹则递归处理
  if [ -d $1"/"$file ]
  then
    read_dir $1"/"$file
  else
    # 当前文件不是一个文件夹
    if [[ -f $1"/"$file ]]
    then
        if [[ ${file:0-4} == '.jar' ]];
        then
            # 获取pid
                pid=`ps -ef | grep $file | grep -v grep | awk '{print $2}'`
            # -z 表示如果$pid为空时则输出提示
                if [ -z $pid ];then
                        echo ""
                echo "Service $file is not running!"
                        echo ""
                else
                        echo ""
                echo "Service $1"/"$file is running. It's pids=${pid}"
                        echo ""
                fi
        fi
    fi
  fi
done
}
#读取第一个参数
read_dir $1


```
## 3.stop.sh
```ruby
#!/usr/bin/env bash
# 停止一个目录下的所有jar程序
function read_dir(){
for file in `ls $1`
do
  #如果当前文件是文件夹则递归处理
  if [ -d $1"/"$file ]
  then
    read_dir $1"/"$file
  else
    # 当前文件不是一个文件夹
    if [[ -f $1"/"$file ]]
    then
        if [[ ${file:0-4} == '.jar' ]];
        then
            # 获取pid
            # 模糊匹配 $file 进程| 过滤自身命令进程 | 输出进程表中的进程号
                pid=`ps -ef | grep $file | grep -v grep | awk '{print $2}'`
            # -z 表示如果$pid为空时则输出提示
                if [ -z $pid ]; then
                echo "Service $file is not running! It's not necessary to stop it!"
                else
                # 杀死进程
                        kill -9 $pid
                        echo "Service stop successfully！pid:${pid} which has been killed forcibly!"
			echo ""
                fi
        fi
    fi
  fi
done
}
#读取第一个参数
read_dir $1


```

# 二、结束语
评论区可留言，可私信，可互相交流学习，共同进步，欢迎各位给出意见或评价，本人致力于做到优质文章，希望能有幸拜读各位的建议！

>专注品质，热爱生活。
交流技术，寻求同志。
