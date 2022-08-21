---
title: 玩转ubuntu——golong篇
date: 2019.07.17 14:57
tags: [ubuntu, golong]
---
服务端ngrok下新建start.sh文件
```
vi start.sh
# start.sh
pid=`ps -ef|grep "./bin/ngrokd -domain='domain.com' -httpAddr=':81' -httpsAddr=':444'"| grep -v "grep"|awk '{print $2}'`

if [ "$pid" != "" ]
then
echo "run.py already run, stop it first"
kill -9 ${pid}
fi

echo "starting now..."

nohup ./bin/ngrokd -domain="domain.com" -httpAddr=":81" -httpsAddr=":444" > test.out 2>&1 &

pid=`ps -ef|grep "./bin/ngrokd -domain="domain.com" -httpAddr=":81" -httpsAddr=:444> test.out 2>&1 &"| grep -v "grep"|awk '{print $2}'`

echo ${pid} > pid.out
echo "ngrokd -domain="domain.com" started at pid: "${pid}

```
