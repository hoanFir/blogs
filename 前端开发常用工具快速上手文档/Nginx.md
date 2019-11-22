🐾 Nginx

🕘 2019.11.22 由 hoanfirst 编辑

1. 启动

nginx服务默认为80端口。

```bash

start nginx

nginx.ext

nginx -t

```

2. 停止

```

# 快速停止，可能不保存相关信息
nginx -s stop

# 完整有序地停止，保存相关信息
nginx -s quit

```


3. 关闭

```bash

# 修改nginx.conf后关闭任务管理器中nginx进程(所有)
taskkill /IM  nginx.exe  /F

```


4. 修改配置信息后重载

```bash

nginx.exe -s reload

```


5. 重新打开日志文件

```bash

nginx -s reopen

```











