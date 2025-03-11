docker简化命令

```bash
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"
alias dis='docker images' 
```

docker启动容器

```bash
# 第一次运行
docker run -d --name redis_demo01 -p 6379:6379 registry.openanolis.cn/openanolis/redis:5.0.3-8.6
# 停止
docker stop redis_demo01
# 重启启动
docker start redis_demo01
# 查看容器日志(-f 代表一直查看)
docker logs -f redis_demo01
# 进入容器执行
docker exec -it redis_demo01 bash
# 进入容器并执行命令
docker exec -it redis_demo01 redis-cli
```

#### **一、数据卷简介**

- **数据卷（Volume）**：用于持久化容器数据，独立于容器生命周期，避免容器删除后数据丢失。
- **特点**：
  - 数据卷可在容器间共享和重用。
  - 数据修改实时生效（无需重启容器）。
  - 数据卷默认存储在宿主机的 `/var/lib/docker/volumes/` 目录下。

#### **二、核心操作命令**

##### **1. 创建数据卷**

```bash
# 创建名为 `myvol` 的数据卷
docker volume create myvol

# 指定驱动和标签（可选）
docker volume create --driver local --label "env=prod" myvol
```

##### **2. 查看数据卷**

bash

```bash
# 列出所有数据卷
docker volume ls

# 查看详细信息（如存储路径）
docker volume inspect myvol

# 按名称过滤
docker volume ls -q | grep "myvol"
```

##### **3. 挂载数据卷到容器**

- **方式一：`-v` 参数（自动创建卷）**

  bash

  ```bash
  # 挂载数据卷到容器路径（卷不存在则自动创建）
  docker run -d -v myvol:/container/path nginx:latest
  
  # 指定读写权限（默认rw，可设为ro只读）
  docker run -d -v myvol:/container/path:ro nginx:latest
  ```

- **方式二：`--mount` 参数（更详细配置）**

  ```bash
  docker run -d \
    --name mynginx \
    --mount type=volume,source=myvol,target=/container/path,readonly \
    nginx:latest
  ```

##### **4. 删除数据卷**

```bash
# 删除指定数据卷
docker volume rm myvol

# 强制删除（若数据卷正在被使用）
docker volume rm -f myvol

# 清理所有未使用的数据卷（谨慎操作！）
docker volume prune
```

##### **5. 数据卷备份与恢复**

- **备份到宿主机**：

  

  ```bash
  # 启动临时容器，打包数据卷内容到宿主机
  docker run --rm \
    -v myvol:/data \
    -v $(pwd):/backup \
    busybox \
    tar czf /backup/myvol_backup.tar.gz /data
  ```

- **从备份恢复**：

  

  ```bash
  # 创建新数据卷并解压备份
  docker volume create newvol
  docker run --rm \
    -v newvol:/data \
    -v $(pwd):/backup \
    busybox \
    tar xzf /backup/myvol_backup.tar.gz -C /data
  ```

------

#### **三、其他实用技巧**

##### **1. 直接挂载宿主机目录（非数据卷）**

```bash
# 将宿主机 `/host/path` 挂载到容器 `/container/path`
docker run -d -v /host/path:/container/path nginx:latest
```

##### **2. 共享数据卷（多容器间共享数据）**

```bash
# 容器1：写入数据
docker run -d --name writer -v sharedvol:/data alpine sh -c "echo 'Hello' > /data/file.txt"

# 容器2：读取数据
docker run -d --name reader -v sharedvol:/data alpine cat /data/file.txt
```

##### **3. 查看数据卷内容**

```bash
# 进入临时容器浏览数据卷
docker run --rm -it -v myvol:/data busybox sh
ls /data
```

------

#### **四、注意事项**

1. **数据卷与绑定挂载的区别**：
   - 数据卷由Docker管理，绑定挂载直接操作宿主机文件系统。
2. **权限问题**：容器内进程需有权限访问挂载路径。
3. **数据安全**：删除容器时默认保留数据卷，需手动清理。

------

#### **五、常见用例**

- **数据库持久化（如MySQL）**：

   

  ```bash
  docker run -d --name mysql_db \
    -v mysql_data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    mysql:latest
  ```
