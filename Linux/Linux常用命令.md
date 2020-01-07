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

du -hs .
显示当前目录的总大小，并以可读格式也就是kb/mb

```



- **which**

where is the execute located, 显示命令所处位置



- **top**

what is eating your cpu，实时显示 process 信息

```

top -n 2
显示更新2次后就退出
只显示2个进程的信息（Mac OS）

top -d 3
top -s 3（Mac OS）
更新周期为3秒

top -p 123
top -pid 123（Mac OS）
显示进程号为123的进程实时信息，如CPU、内存占用率等


```
