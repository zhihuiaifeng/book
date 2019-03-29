# Elasticsearch开机自启动配置

## 1.切换用户至root

```sh
su - root
```

## 2.编辑elasticsearch文件

```sh
vim /etc/init.d/elasticsearch
```

加入内容

```sh
#!/bin/sh
#chkconfig: 2345 80 05
#description: elasticsearch
#author: taft
 
export JAVA_HOME=/home/leyou/jdk1.8.0_144
export JAVA_BIN=/home/leyou/jdk1.8.0_144/bin
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JAVA_BIN PATH CLASSPATH

case "$1" in
start)
    su leyou<<!
    cd /home/leyou/elasticsearch
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;  
stop)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    ;;  
restart)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    su leyou<<!
    cd /home/leyou/elasticsearch
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;  
*)
    echo "start|stop|restart"
    ;;  
esac

exit $?

```



## 3.修改文件权限

```sh
sudo chmod +x /etc/init.d/elasticsearch
```

## 4.添加开机自启动

```sh
sudo chkconfig --add /etc/init.d/elasticsearch
```

## 5.注意事项

### 5.1以上脚本的用户为leyou，如果你的用户不是，则需要替换

### 5.2以上脚本的JAVA_HOME以及elasticsearch_home如果不同请替换，其他无需关注











