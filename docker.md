# docker使用

## 基本命令

### 运行

```sh
docker run -d -p 80:80 docker/getting-started
```

- `-d` detached modo（后台运行）
- `-p 80:80` 将container的80端口映射到主机的80端口
- `docker/getting-started` 使用的image

### 创建应用

```sh
docker build -t getting-started .
```

### 更新应用

#### 重新创建

```
docker build -t getting-started .
```

#### 替换旧的container(cli)

1. 获取container的ID

   ```sh
   docker ps
   ```

2. 停止contaner

   ```sh
   docker stop <the-container-id>
   ```

3. 删除contaner

   ```sh
   docker rm <the-container-id>
   ```

后两条指令可以用一条指令完成

```
docker rm -f <the-container-id>
```

### 发布应用

#### 创建repo

 [Docker Hub](https://hub.docker.com/)

#### 推送应用

查看image

```sh
docker image ls
```

更改image名字

```sh
docker tag getting-started YOUR-USER-NAME/getting-started
```

推送

```
docker push YOUR-USER-NAME/getting-started
```

### 数据持久化

#### 使用volume

1. 创建`volume`

   ```sh
   docker volume create todo-db
   ```

2. 将`volume`挂在到container，`-v`进行指定

   ```sh
   docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
   ```

3. 查看`volume`存储位置

   ```
   docker volume inspect todo-db
   ```

#### 绑定挂载

```
docker run -dp 3000:3000 -v /path/to/data:/etc/todos getting-started
```

开发模式下的container

1. 使用以下命令

   ```powershell
   docker run -dp 3000:3000 `
       -w /app -v "$(pwd):/app" `
       node:12-alpine `
       sh -c "yarn install && yarn run dev"
   ```

   - `-w /app` 将工作目录设为app，后续命令在该位置下运行

2. 查看日志

   ```sh
   docker logs -f <container-id>
   ```

3. 修改代码

4. 重新创建应用

   ```sh
   docker build -t getting-started .
   ```

### 多应用

#### 创建MySQL

1. 创建网络

   ```sh
   docker network create todo-app
   ```

2. 创建MySQL并连接到网络

   ```sh
   docker run -d \
       --network todo-app --network-alias mysql \
       -v todo-mysql-data:/var/lib/mysql \
       -e MYSQL_ROOT_PASSWORD=secret \
       -e MYSQL_DATABASE=todos \
       mysql:5.7
   ```

   

3. 测试是否能够连接

   ```
   docker exec -it <mysql-container-id> mysql -p
   ```

   ```
   mysql> SHOW DATABASES;
   ```

#### 连接MySQL

为了container之间的网络连接，可以使用 [nicolaka/netshoot](https://github.com/nicolaka/netshoot) ，里面包含了大量的网络工具。

1. 运行netshoot应用，连接至mysql所在网络

   ```sh
   docker run -it --network todo-app nicolaka/netshoot
   ```

2. 查找mysql

   ```sh
   dig mysql
   ```

#### 运行App

1. 启动app

   ```shell
   docker run -dp 3000:3000 \
     -w /app -v "$(pwd):/app" \
     --network todo-app \
     -e MYSQL_HOST=mysql \
     -e MYSQL_USER=root \
     -e MYSQL_PASSWORD=secret \
     -e MYSQL_DB=todos \
     node:12-alpine \
     sh -c "yarn install && yarn run dev"
   ```

2. 查看日志

   ```sh
   docker logs <container-id>
   ```

3. 添加记录

4. 查看数据库

   ```sh
   docker exec -it <mysql-container-id> mysql -p todos
   ```

   ```
   mysql> select * from todo_items;
   ```

   

