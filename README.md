# Docker Compose 部署 Z-BlogPHP

使用 Docker Compose 快捷部署 Z-BlogPHP + MySQL；

## 镜像

Z-BlogPHP：`https://github.com/zblogcn/zblogphp-tencent-openapp-docker`

MySQL：`mysql/mysql-server:5.7`

## 使用

1、拉取项目代码并初始化文件：

```bash
# 克隆项目文件
git clone https://github.com/wdssmq/zbp-docker-compose.git zbp-dc
# 进入项目文件夹
cd zbp-dc
# 复制配置文件
cp conf/common.env.sample conf/common.env
cp conf/site_zbp.env.sample conf/site_zbp.env
# 「可选」映射 www 为其他路径
ln -s /home/www www
```

2、配置`conf/*.env`变量

`common.env`内为数据库密码，zbp 和 MySQL 都要使用，两个变量值要一样；

`site_zbp.env`内设置 zbp 管理员的用户名和密码；

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

# 停止服务
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
