# Docker Compose 部署 Z-BlogPHP

使用 Docker Compose 快捷部署 Z-BlogPHP + MySQL；

## 镜像

Z-BlogPHP：`https://github.com/zblogcn/zblogphp-tencent-openapp-docker`

MySQL：`mysql/mysql-server:5.7`

## 前置

```bash
# 安装 Docker Compose
sudo curl -L https://github.com/docker/compose/releases/download/v2.4.1/docker-compose-`uname -s`-`uname -m` \
 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

生成最新版 Docker Compose 安装命令：[https://demo.wdssmq.com/tools/GenShell/](https://demo.wdssmq.com/tools/GenShell/ "生成最新版 Docker Compose 安装命令")

## 使用

1、拉取项目代码并初始化文件：

```bash
# 克隆项目文件
git clone https://github.com/wdssmq/zbp-docker-compose.git zbp-dc
# 进入项目文件夹
cd zbp-dc
# 复制配置文件
cp conf/common.env.sample conf/common.env
cp conf/site_zbp_def.env.sample conf/site_zbp_def.env
# 「可选」映射 www 为其他路径
ln -s /home/wwwroot data/www
```

2、配置`conf/*.env`变量

`common.env`内为数据库密码，zbp 和 MySQL 都要使用，两个变量值要一样；

`site_zbp_def.env`内设置 zbp 管理员的用户名和密码；

3、启动

```bash
# 初始运行，会输出各种日志；
# ctrl + c 中止，可多次执行直到不报错
docker-compose up
# 正式运行（后台启动）
# docker-compose down
docker-compose up -d
```

4、其他命令

```bash
# 查看配置
docker-compose config

# 重启
docker-compose restart

# 停止
docker-compose stop

# 完全移除容器
docker-compose down

# 查看启动的容器情况
docker-compose ps

# 查看容器输出日志
docker logs $container_name

# 进入容器内部
docker exec -it $container_name /bin/bash
```

### phpMyAdmin 连接管理数据库

如果需要 phpMyAdmin 可单独配置：

```bash
# 强制删除容器
docker rm --force phpMyAdmin
docker run --name phpMyAdmin \
  --network=zbp-dc_net_web \
  -p 9100:80 \
  -e PMA_HOST=MySQL \
  -e UPLOAD_LIMIT=4096K \
  -d phpmyadmin/phpmyadmin

# 关闭（但不删除）
docker stop phpMyAdmin
# 启用
docker start phpMyAdmin
```

注：

- `-e PMA_HOST=MySQL`中`MySQL`为 docker-compose.yml 文件内定义的服务名；
- `-e UPLOAD_LIMIT=4096K`用于设置导入文件的大小限制，可以按需要设置，比如`20M`；
- `--network=zbp-dc_net_web`实际所需需要的值可以执行`docker network ls`查看；
  - 就是使用执行路径文件夹的名字作为前缀，容器名也是；

```bash
docker network ls
# NETWORK ID     NAME             DRIVER    SCOPE
# 8fe33d9c54d0   bridge           bridge    local
# 4579b81d15b4   host             host      local
# 2e9e3da577ef   none             null      local
# a620eec8f4dc   zbp-dc_net_web   bridge    local
```
