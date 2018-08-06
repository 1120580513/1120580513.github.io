# Redis

---

## Install
1. 下载文件并添加环境变量
2. 修改redis.windows-service.conf以允许远程连接
```conf
#bind 127.0.0.1
protected-mode no
requirepass shaoxing
```
3. redis-server.exe：启动Redis服务
4. redis-cli.exe -h 127.0.0.1 -p 6379：连接到Redis
5. edis-server.exe --service-install redis.windows-service.conf：注册成服务
6. redis-server --service-start：启动Redis服务
7. redis-server --service-stop：停止Redis服务