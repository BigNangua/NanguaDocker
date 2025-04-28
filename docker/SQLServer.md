# 使用 Docker 安装 SQL Server

以下是使用 Docker 安装 Microsoft SQL Server 的详细步骤：

## 准备工作

确保你已经安装了 Docker：

- 对于 Windows/macOS: [Docker Desktop](https://www.docker.com/products/docker-desktop)
- 对于 Linux: 使用系统包管理器安装 Docker

### 1、拉取 SQL Server 镜像

运行以下命令拉取最新的 SQL Server 2019 Linux 容器镜像：

```bash
docker pull mcr.microsoft.com/mssql/server:latest
```

### 2、运行 SQL Server 容器

```bash
docker run -d --name sqlserver-container \
  -p 1433:1433 \  # 映射端口，宿主机:容器
  -e 'ACCEPT_EULA=Y' \  # 接受 EULA
  -e 'MSSQL_SA_PASSWORD=Nangua@3234' \  # 设置 SA 用户密码
  -e 'MSSQL_PID=Express' \  # 设置 SQL Server 版本，这里用的是 Express 版本
  -v /my/sqlserver/data:/var/opt/mssql/data \  # 映射数据文件存储路径
  -v /my/sqlserver/log:/var/opt/mssql/log \  # 映射日志文件存储路径
  -v /my/sqlserver/secrets:/var/opt/mssql/secrets \  # 映射密钥文件存储路径
  --restart unless-stopped \  # 容器停止后自动重启
  mcr.microsoft.com/mssql/server:latest
```

### **3、说明**

- `-p 1433:1433`：将 SQL Server 端口 1433 映射到宿主机的 1433 端口。
- `-e 'ACCEPT_EULA=Y'`：接受 SQL Server 的许可协议。
- `-e 'Nangua@3234'`：设置 `sa` 用户的密码，确保密码符合 SQL Server 安全要求（长度至少 8 个字符，包含大写字母、小写字母、数字和符号）。
- `-e 'MSSQL_PID=Express'`：选择 SQL Server 版本，这里使用的是 Express 版，可以根据需要选择其他版本（例如：Developer、Standard、Enterprise）。
- `-v /my/sqlserver/data:/var/opt/mssql/data`：映射宿主机的数据库数据目录 `/my/sqlserver/data` 到容器的 `/var/opt/mssql/data`，以便数据持久化。
- `-v /my/sqlserver/log:/var/opt/mssql/log`：映射日志文件。
- `-v /my/sqlserver/secrets:/var/opt/mssql/secrets`：映射密钥文件目录（可选）。
- `--restart unless-stopped`：自动重启容器，除非手动停止。

### 4、进入 SQL Server 容器

你可以使用以下命令连接到 SQL Server 容器：

```bash
docker exec -it sqlserver-container /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'Nangua@3234'
```

## 5. 连接到 SQL Server

可以使用以下工具连接：

**sqlcmd** (容器内):

```bash
docker exec -it sql_server_container /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P "Nangua@3234"
```

- **Azure Data Studio** 或 **SQL Server Management Studio** (SSMS):

  - 服务器: `localhost,1433`
  - 认证类型: SQL Server 认证
  - 用户名: `SA`
  - 密码: `Nangua@3234`

  ## 6. 其他常用命令

  停止容器：

  ```bash
  docker stop sql_server_container
  ```

  启动已停止的容器：

  ```bash
  docker start sql_server_container
  ```

  删除容器（数据会保留在卷中）：

  ```bash
  docker rm sql_server_container
  ```

  删除数据卷：

  ```bash
  docker volume rm sql_server_data
  ```

  ## 注意事项

  1. SA 密码必须符合复杂性要求（至少8个字符，包含大小写字母、数字和符号）
  
  2. 生产环境中应考虑更安全的密码管理方式
  
  3. 默认情况下，SQL Server Express 版本有10GB的数据库大小限制
  
  4. 对于生产环境，应考虑配置持久化存储、备份策略和高可用性方案
  
  5. 确保 `ACCEPT_EULA` 环境变量格式正确
  
     尝试将命令中的 `-e` 参数的引号更改为双引号，避免可能的引号问题：
  
     ```bash
     docker run -d --name sqlserver -p 1433:1433 -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Nangua@3234" -e "MSSQL_PID=Express" -v E:\docker\DockerDesktopWSL\sqlserver\data:/var/opt/mssql/data -v E:\docker\DockerDesktopWSL\sqlserver\log:/var/opt/mssql/log -v E:\docker\DockerDesktopWSL\sqlserver\secrets:/var/opt/mssql/secrets --restart unless-stopped mcr.microsoft.com/mssql/server:latest
     ```
  
     
