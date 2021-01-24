# 常见Linux命令

### 端口操作

查看端口使用情况：

```shell
netstat -ntulp |grep 1935(端口号) 
```

杀死某端口的进程

```shell
kill -9 进程id
```

查看已开放的端口

```shell
firewall-cmd --list-ports 
```

查看进程

```shell
ps -ef | grep mysql(服务的名称)
```

### 开放端口步骤：

1、防火墙的常规操作 

```shell
 #开启防火墙
 systemctl start firewalld
 
 #查看防火墙状态
 systemctl status firewalld
 
 #关闭防火墙
 systemctl stop firewalld
 
 #永久关闭命令：
 systemctl disable firewalld
 
 #重启防火墙
 firewall-cmd --reload
```

2、开放端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

3、重启防火墙



### 服务器部署

后台运行jar包

```shell
nohup java -jar XXX.jar &
```

