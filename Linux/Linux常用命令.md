🐾 Linux常用命令

🕘 2020.01.07 由 hoanfirst 编辑


- **ls**

```

ls -l
显示文件详细信息

ls -a
显示隐藏文件

```



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

process status，查看进程信息快照

```

ps -ef|grep app
返回系统所有用户的所有进程信息
-e 
| 管道，前


```



- **free**

相比于top，使用free查看内存信息会更准确
