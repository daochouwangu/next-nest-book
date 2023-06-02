要安装 Docker 并使用它开启一个 MySQL 服务，请按照以下步骤操作：

1. 安装 Docker：

   - 对于 Windows，请访问 [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) 下载并安装 Docker Desktop。
   - 对于 macOS，请访问 [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) 下载并安装 Docker Desktop。
   - 对于 Linux，请参考 [Docker 官方文档](https://docs.docker.com/engine/install/) 中的指南，根据您的 Linux 发行版进行安装。

2. 安装完成后，启动 Docker Desktop（对于 Windows 和 macOS）。对于 Linux，根据安装指南启动 Docker 服务。

3. 打开终端（或命令提示符），并运行以下命令以下载并启动 MySQL Docker 容器：

   ```bash
   docker run --name mysql-server -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 -d mysql:latest
   ```

   这个命令会执行以下操作：
   - `--name mysql-server`：为容器指定一个名称，这里我们使用 `mysql-server`。
   - `-e MYSQL_ROOT_PASSWORD=my-secret-pw`：设置环境变量 `MYSQL_ROOT_PASSWORD`，这将成为 MySQL `root` 用户的密码。请将 `my-secret-pw` 替换为您自己的密码。
   - `-p 3306:3306`：将容器的 3306 端口映射到宿主机的 3306 端口，以便在本地访问 MySQL 服务。
   - `-d mysql:latest`：从 Docker Hub 下载并运行最新版本的 MySQL 镜像。

4. MySQL 服务现已在 Docker 容器中运行。您可以使用任何 MySQL 客户端（如 MySQL Workbench，DBeaver 等）连接到本地的 3306 端口，并使用 `root` 用户名和您设置的密码进行访问。

如需停止 MySQL 服务，请运行：

```bash
docker stop mysql-server
```

如需再次启动 MySQL 服务，请运行：

```bash
docker start mysql-server
```

如需删除 MySQL 容器，请先停止它，然后运行：

```bash
docker rm mysql-server
```

注意，删除容器后，您需要再次运行 `docker run` 命令来创建新的容器。在删除之前，请确保已备份所有重要数据。