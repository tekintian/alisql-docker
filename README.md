# Alisql阿里SQL数据库 Docker容器
最新版alisql for docker 镜像

[AliSQL](https://github.com/alibaba/AliSQL). See [the Docker Hub page](https://hub.docker.com/r/tekintian/alisql/) for the full readme on how to use this Docker image.


## How to use this image
Because AliSQL has TokuDB storage engine statically built-in, and Transparent Huge Pages(THP) will prohibit TokuDB from starting (Why TokuDB hates Transparent HugePages) , THP must be disabled(echo never > sys/kernel/mm/transparent_hugepage/enabled).

All the usages of official mysql image are supported by AliSQL, below are just some notable examples:

## Start a alisql server instance
Starting a AliSQL instance is simple:

$ docker run --name some-alisql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d alisql/alisql:tag
Container shell access and viewing AliSQL logs
The docker exec command allows you to run commands inside a Docker container. The following command line will give you a bash shell inside your alisql container:

$ docker exec -it some-alisql bash
The AliSQL Server log is available through Docker's container log:

$ docker logs some-alisql
Using a custom MySQL configuration file
The MySQL startup configuration is specified in the file /etc/mysql/my.cnf, and that file in turn includes any files found in the /etc/mysql/conf.d directory that end with .cnf. Settings in files in this directory will augment and/or override settings in /etc/mysql/my.cnf. If you want to use a customized MySQL configuration, you can create your alternative configuration file in a directory on the host machine and then mount that directory location as /etc/mysql/conf.d inside the alisql container.

If /my/custom/config-file.cnf is the path and name of your custom configuration file, you can start your alisql container like this (note that only the directory path of the custom config file is used in this command):

$ docker run --name some-alisql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d alisql/alisql:tag
This will start a new container some-alisql where the MySQL instance uses the combined startup settings from /etc/mysql/my.cnf and /etc/mysql/conf.d/config-file.cnf, with settings from the latter taking precedence.

## Environment Variables
MYSQL_ROOT_PASSWORD
MYSQL_DATABASE
MYSQL_USER, MYSQL_PASSWORD
MYSQL_ALLOW_EMPTY_PASSWORD
MYSQL_RANDOM_ROOT_PASSWORD
MYSQL_ONETIME_PASSWORD

# AliSql for Docker 中文说明

alisql 最新版本  阿里云出品的 mysql数据库  AliSQL Docker镜像， 官方的那个有2.46G简直臃肿，为了方便大家使用，特地PUSH了这个轻巧版的， 压缩后只有一百多M， 欢迎大家使用交流！


# 使用方法：

# 1. 拉取镜像到你的本地 
$ docker push tekintian/alisql

# 2. 在你的本地的宿主机中创建共享目录
 /mywork/alisql
/mywork/alisql/data  存放数据库文件夹
/mywork/alisql/logs  数据库日志文件夹

/mywork/alisql/my.cnf  数据库配置文件，使用这个直接加载到容器中，方便使用。


# 3. 使用本地配置文件
将下面的内容拷贝到你本地的配置文件 /mywork/alisql/my.cnf 文件中

[mysqld]
basedir=/usr/local/mysql
datadir=/data/mysql/data
socket=/usr/local/mysql/mysql.sock
user=mysql
symbolic-links=0

[mysqld_safe]
log-error=/data/mysql/log/mysqld.log
pid-file=/usr/local/mysql/mysqld.pid
[client]
socket=/usr/local/mysql/mysql.sock



# 4.运行AliSQL数据库容器

docker run --name alisql -it -d \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=888888 \
    --restart=always \
    -v /mywork/alisql/my.cnf:/usr/local/mysql/etc/my.cnf \
    -v /mywork/alisql/data:/data/mysql/data \
    -v /mywork/alisql/logs:/data/mysql/log \
    tekintian/alisql


# 说明： 
上面的运行配置中  -p 3306:3306 左边的3306为容器对外暴露的数据库连接端口，如果与宿主机中的端口冲突，可自行修改， 右边的3306为Alisql数据库的默认端口。

数据库的root用户默认密码为 888888 ， 请自行修改


# 查看运行日志 
docker logs <容器ID>

# 进入容器
docker exec -it <容器ID> bash


# 交流讨论
Tekin

QQ:3316872019

http://dev.yunnan.ws





