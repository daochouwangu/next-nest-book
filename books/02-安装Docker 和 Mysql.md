# 03-安装Docker 和 Mysql

安装数据库之前我们要先安装Docker ，相信Docker大家都很熟了，我也不介绍了，不懂得可以自行搜索。

安装完成后我们使用Docker来开启一个MySQL服务

注意：下面的docker名字、密码、数据卷、IP都是可以定制的。

1.  打开终端（或命令提示符），并运行以下命令以下载并启动 MySQL Docker 容器：

    {% code fullWidth="true" %}
    ```bash
    docker run --name mysql-server -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 -d mysql:latest
    ```
    {% endcode %}

    这个命令会执行以下操作：

    * `--name mysql-server`：为容器指定一个名称，这里我们使用 `mysql-server`。
    * `-e MYSQL_ROOT_PASSWORD=my-secret-pw`：设置环境变量 `MYSQL_ROOT_PASSWORD`，这将成为 MySQL `root` 用户的密码。请将 `my-secret-pw` 替换为您自己的密码。
    * `-p 3306:3306`：将容器的 3306 端口映射到宿主机的 3306 端口，以便在本地访问 MySQL 服务。
    * `-d mysql:latest`：从 Docker Hub 下载并运行最新版本的 MySQL 镜像。
    * `-v /my/own/datadir:/var/lib/mysql` ：指定本地数据盘位置
2. MySQL 服务现已在 Docker 容器中运行。您可以使用任何 MySQL 客户端（如 MySQL Workbench，DBeaver 等）连接到本地的 3306 端口，并使用 `root` 用户名和您设置的密码进行访问。

如需停止 MySQL 服务，请运行：

```bash
docker stop mysql-server
```

如需再次启动 MySQL 服务，请运行：

```bash
docker start mysql-server
```

运行一下应该成功了，如果端口被占用可以换一个或者杀一下占用的。

然后将这个脚本写在 `scripts` 里

```json
"start:mysql": "docker start mysql-server"
```

#### 创建数据库

要创建数据库，首先进入我们刚才开启的 `MySQL` 服务

```
docker exec -it mysql-server mysql -u root -p
```

然后输入刚才设置的密码，就进入了 MySQL 控制台。

接着我们创建一个数据库。

```
CREATE DATABASE aligiegie;
```

这样就创建好了，然后输入 `quit;` 就可以退出了。

### 连接MySQL 和 prisma

然后打开我们刚才的 .env 文件，把里面的连接改为

```
DATABASE_URL="mysql://root:aligiegie@localhost:3306/aligiegie"
```

然后还有 prisma 文件夹里的 schema.prisma 文件

```
provider = "mysql"
```

### 用prisma创建表

还是这个 schema.prisma 文件，我们先加入 user 表试试

```prisma
model user {
  id Int @default(autoincrement()) @id
  name String
  email String
  password String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

然后运行

```
npx prisma migrate dev --name init
```

如果控制台没报错，那么你已经成功了，我们来验证一下。

依旧是上方的进入 MySQL 控制台的指令，然后使用

```
USE aligiegie; 
SHOW TABLES;
DESCRIBE user;
```

这样就能看到我们的表结构了。

\
