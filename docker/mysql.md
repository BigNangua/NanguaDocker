### 1. 安装 Docker

如果你还没有安装 Docker，请先安装 Docker。可以参考 [Docker 官方文档](https://docs.docker.com/get-docker/) 进行安装。

### 1、拉取 MySQL 镜像

```bash
docker pull mysql:latest
```

如果你想拉取特定版本的 MySQL，可以指定版本号，例如：

```bash
docker pull mysql:8.0
```

### 2、运行 MySQL 容器

使用以下命令启动 MySQL 容器：

```bash
docker run -d --name mysql-container \
  -p 3306:3306 \  # 端口映射（宿主机:容器）
  -e MYSQL_ROOT_PASSWORD=root \  # 设置 root 用户密码
  -e MYSQL_DATABASE=testdb \  # 创建一个默认数据库
  -e MYSQL_USER=testuser \  # 创建一个普通用户
  -e MYSQL_PASSWORD=testpass \  # 设置普通用户密码
  -v /my/mysql/data:/var/lib/mysql \  # 数据目录映射（持久化）
  --restart unless-stopped \  # 自动重启策略
  mysql:latest
```

### 3、说明

- `-p 3306:3306`：将 MySQL 端口映射到宿主机。

  `-e MYSQL_ROOT_PASSWORD=root`：设置 root 账户密码。

  `-e MYSQL_DATABASE=testdb`：启动时创建 `testdb` 数据库。

  `-e MYSQL_USER=testuser`，`-e MYSQL_PASSWORD=testpass`：创建一个用户 `testuser` 并赋予权限。

  `-v /my/mysql/data:/var/lib/mysql`：将 MySQL 数据存储到宿主机 `/my/mysql/data` 目录，避免数据丢失。

  `--restart unless-stopped`：容器崩溃后自动重启。

### 4、进入 MySQL 容器

你可以使用以下命令进入 MySQL 容器的命令行：

```bash
docker exec -it mysql-container mysql -uroot -p
```

输入之前设置的 root 密码（`my-secret-pw`），即可进入 MySQL 命令行。

### 5. 使用 MySQL

现在你可以像平常一样使用 MySQL 了。例如，创建数据库、表等

### 6. 停止和启动容器

停止容器：

```bash
docker stop mysql-container
```

启动容器：

```bash
docker start mysql-container
```

### 7. 持久化数据

```bash
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=Nangua3234 -d -p 3306:3306 -v /path/on/host:/var/lib/mysql mysql
```

- `-v /path/on/host:/var/lib/mysql`：将主机的 `/path/on/host` 目录挂载到容器的 `/var/lib/mysql` 目录（MySQL 数据存储目录）。

### 8. 查看日志

如果需要查看 MySQL 容器的日志，可以使用以下命令：

```bash
docker logs mysql-container
```

### 9. 删除容器

如果你想删除容器，可以使用以下命令：

```bash
docker rm -f mysql-container
```



```bash
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -v E:\docker\DockerDesktopWSL\mysql:/var/lib/mysql --restart unless-stopped mysql:latest
```

