# Redis 使用教程

配置好镜像源后，执行以下命令启动 Redis：

```bash
sudo docker run -d \
  --name redis \
  -p 6379:6379 \
  redis:latest
```

✅ 验证是否运行成功

```bash
sudo docker ps
```

应该能看到类似输出：

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                    NAMES
xxxxxxxxx      redis:latest   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:6379->6379/tcp   redis
```

✅ 测试连接

```bash
sudo docker exec -it redis redis-cli ping
```

如需持久化数据，可添加卷挂载：

```bash
sudo docker run -d \
  --name redis \
  -p 6379:6379 \
  -v redis-data:/data \
  redis:latest redis-server --appendonly yes
```
