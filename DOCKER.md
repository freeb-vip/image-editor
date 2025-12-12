# Docker 部署说明

## 文件说明

本项目已配置 Docker 支持，包含以下文件：

- `Dockerfile` - Docker 镜像构建文件
- `docker-compose.yml` - Docker Compose 配置文件
- `nginx.conf` - Nginx 服务器配置
- `.dockerignore` - Docker 构建时忽略的文件

## 快速启动

### 1. 构建并启动服务

```bash
docker compose up -d --build
```

### 2. 访问应用

服务启动后，可以通过以下地址访问：

- **本地访问**: http://localhost:3000
- **局域网访问**: http://你的IP地址:3000

### 3. 查看容器状态

```bash
docker ps
```

### 4. 查看容器日志

```bash
docker logs image-editor
```

### 5. 停止服务

```bash
docker compose down
```

### 6. 重启服务

```bash
docker compose restart
```

## 端口配置

默认端口映射为 `3000:80`，如需修改端口，请编辑 `docker-compose.yml` 文件：

```yaml
ports:
  - "你的端口:80"
```

## 技术架构

- **构建阶段**: 使用 Node.js 18 Alpine 镜像构建项目
- **运行阶段**: 使用 Nginx Alpine 镜像提供静态文件服务
- **多阶段构建**: 减小最终镜像体积，提高部署效率

## 注意事项

1. 首次构建可能需要较长时间（下载依赖、编译代码）
2. 容器默认会自动重启（restart: unless-stopped）
3. 如需修改源码，重新构建镜像：`docker compose up -d --build`
4. 生产环境建议配置 HTTPS 和域名

## 故障排查

### 容器无法启动

```bash
# 查看容器日志
docker logs image-editor

# 查看所有容器（包括停止的）
docker ps -a
```

### 端口被占用

修改 `docker-compose.yml` 中的端口号，或停止占用 3000 端口的程序。

### 重新构建镜像

```bash
# 停止并删除容器
docker compose down

# 删除镜像
docker rmi image-editor-image-editor

# 重新构建
docker compose up -d --build
```

## 镜像管理

### 查看镜像

```bash
docker images | grep image-editor
```

### 清理未使用的镜像

```bash
docker image prune -a
```

## 生产环境优化

1. 配置反向代理（如使用 Traefik 或 Nginx）
2. 启用 HTTPS/SSL 证书
3. 配置域名解析
4. 设置资源限制（CPU、内存）
5. 配置日志轮转
6. 定期备份和更新

---

**当前状态**: ✅ 服务已启动并运行在 http://localhost:3000
