# Docker 安装与国内镜像源配置指南

## 1. 下载安装脚本

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```

## 2. 执行安装脚本

```bash
sudo sh get-docker.sh
```

> ⚠️ 注意：请使用 sudo 权限执行，确保脚本有足够权限安装 Docker。

## 3. 配置国内镜像源

### 3.1 创建或更新 Docker 镜像源配置文件

编辑或创建 /etc/docker/daemon.json 文件，添加国内镜像源：

```bash
echo '{ "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://noohub.ru",
    "https://huecker.io",
    "https://dockerhub.timeweb.cloud"
] }' | sudo tee /etc/docker/daemon.json 
```

> ✅ 推荐镜像源说明：

[DaoCloud](https://docker.m.daocloud.io) – 国内稳定加速，支持多种镜像仓库

[Noohub](https://noohub.ru) – 俄罗斯 Docker Hub 代理

[Huecker](https://huecker.io) – 开源 Docker Hub 代理

[Timeweb](https://dockerhub.timeweb.cloud) – 俄罗斯 Timeweb Cloud 提供的镜像代理

应用配置并重启 Docker

```bash
# 重载 systemd 配置
sudo systemctl daemon-reload

# 清理 Docker 缓存（可选）
docker system prune -a -f

# 重启 Docker 服务
sudo systemctl restart docker
```

验证镜像源是否生效

```bash
sudo docker pull redis:latest
```

> ✅ 若下载速度明显提升，说明国内镜像源配置成功。
