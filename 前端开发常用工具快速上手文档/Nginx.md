启动

start nginx 或 nginx.exe 或 

nginx //直接启动

nginx -t //检查并启动



修改nginx.conf后关闭任务管理器中nginx进程(所有)

taskkill /IM  nginx.exe  /F



修改nginx.conf后重启nginx

nginx.exe -s reload



重新打开日志文件

nginx -s reopen









