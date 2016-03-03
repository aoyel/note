1.首先有两台mysql服务器，配置相同版本的mysql

2.修改两台服务器的配置

```
vi /path/to/my.cnf 
[mysqld] 
log-bin=mysql-bin //[必须]启用二进制日志 
server-id=100 //[必须]服务器唯一ID，默认是1，一般取IP最后一段

```

3.重启两台服务器的mysql服务
```
sudo service mysql-server restart

```


4.配置master服务器

```
#创建用户
>mysql -uroot -proot 
>GRANT REPLICATION SLAVE ON *.* to 'sqlsync'@'%' identified by '123456'; #允许所有IP访问 

#或者绑定ip 
>GRANT REPLICATION SLAVE ON *.* to 'sqlsync'@'192.168.1.100' identified by '123456'; #只允许192.168.1.100访问
```

5.查询master状态信息
```
mysql> show master status;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000002 |      696 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)

 #记录下file和position
```

6.登录从服务器

```
#配置master 
mysql>change master to master_host='192.168.1.120',master_user='sqlsync',master_password='123456',master_log_file='mysql-bin.000002',master_log_pos=303; 
#file记得和master的file对应，master_log_pos记得和master postion对应 
#启动从服务器 
mysql>start slave;
```
7.查看从服务器的状态
```
mysql show slave status;

```

8.主从测试

master服务器创建一些数据，然后查看从服务器有没有数据即可！

注意：

主服务器要把 my.cnf中
```
bind 127.0.0.1
```
注释掉

