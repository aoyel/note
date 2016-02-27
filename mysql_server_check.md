##mysql 服务自动启动脚本


```
#!/bin/bash
pgrep -x mysqld &> /dev/null
 
if [ $? -ne 0 ]
 
then
 
echo "At time: `date` :MySQL  is stop .">> /var/log/mysql_messages
 
service mysql start
#echo "At time: `date` :MySQL server is stop."
 
else
 
echo "MySQL server is running ."
 
fi

```

加入定时任务
```
* */1 * * * sh /patch/to/script.sh
```

