🐾 Nginx

🕘 2019.11.22 由 hoanfirst 编辑

1. 启动

nginx服务默认为80端口。

```bash

start nginx

nginx.exe

# 检查配置文件是否正确
nginx -t

```

2. 关闭

```

# 正常关闭，在关闭前会继续完成已接受的连接请求，也会保存相关信息
nginx -s quit

# 粗暴关闭，可能不保存相关信息
nginx -s stop

# 使用Widows系统taskkill命令终止nginx进程(所有)
taskkill /im nginx.exe /f

```


3. 修改配置信息后重载

```bash

nginx.exe -s reload

```


4. 重新打开日志文件

```bash

nginx -s reopen

```











