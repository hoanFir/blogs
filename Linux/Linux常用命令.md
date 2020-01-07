🐾 Linux常用命令

🕘 2020.01.07 由 hoanfirst 编辑


- **ls**

```

ls -l
显示文件详细信息

ls -a
显示隐藏文件

```


- **cat**

连接指定文件并打印其内容到标准输出设备



- **pwd**

prints working directory，打印当前所处路径



- **du**

disk usage，用于显示文件系统磁盘使用情况。

```
du -hd 1 .
显示当前目录下所有各个文件夹总大小，并以可读格式也就是kb/mb
-h 可读
-d 深度

du -hs .
显示当前目录的总大小，并以可读格式也就是kb/mb
-h 可读
-s 总大小

```



- **which**

where is the execute located, 显示命令所处位置



- **ssh**

Secure shell, an encrypted network protocol allowing for remote login and command execution.



- **netstat**

network statistics，显示网络信息，一般用于查看各端口到网络连接情况

```
netstat -nlptu

netstat -an

netstat -anp|grep 80

-n numeric，使用IP地址，而不显示域名
-l listening，显示监听中到服务器到socket
-p programs，显示正在使用socket的程序
-t 显示tcp传输协议的连接信息 
-u 显示udp传输协议的连接信息
-a 显示所有连接中的socket

```



- **ifconfig**

显示或设置网络设备的状态信息，如配置网卡的IPv6、MAC、IP地址

```

ifconfig eth0 down
关闭指定网卡

ifconfig eth0 up
启动指定网卡


ifconfig eth0 -arp
关闭arp协议

ifconfig eth0 arp
启动arp协议

ifconfig eth0 mtu 1500
设置最大传输单元为1500bytes

```



- **top**

what is eating your cpu，实时显示CPU和内存使用情况，以及占用资源由高到低排序低进程，关注点在于实时查看各进程资源占用情况

```

top -n 2
只显示最活跃的2个进程的信息

top -d 3
top -s 3（Mac OS）
更新delay为3秒

top -p 123
top -pid 123（Mac OS）
显示进程号为123的进程实时信息，如CPU、内存占用率等

输入q即可退出

```



- **ps**

process status，查看指定进程信息快照

```

ps -ef|grep keyword
返回系统所有用户的所有进程信息
-e 显示其他用户类似进程的信息
-f 显示详细信息
| 管道，前一个命令的输出作为后一个命令的输入
grep global regular expression print，查找，基于正则表达式进行匹配


```



- **free**

相比于top，使用free查看内存信息会更准确，在Mac OS使用vm_stat替代

vm_stat, virtual memory statistics




