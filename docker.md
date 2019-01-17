# Docker

## About

> [Docker官网](http://www.docker.com)
>
> [Github](https://github.com/moby/moby)
>
> [Docker官方仓库](https://hub.docker.com/)
>
> [Docker菜鸟教程](http://www.runoob.com/docker/docker-tutorial.html)

## Introduce

### Docker vs Object oriented

Docker | Object oriented
:--: | :--:
Image | Class
Container | Object

## Install

...



## Command
> docker容器默认时区为UTC，若要更改可通过在服务器运行命令
>
> `echo "Asia/shanghai" > /etc/timezone;` 然后重启容器即可

### 命令帮助

#### `docker command --help`

### 查找镜像

#### `docker search tomcat`

### 拉取镜像

#### `docker pull tomcat：tag`

> 无tag默认为`latest`

### 查看所有镜像

#### `docker images [name]`

`name` 镜像名称

### 启动一个容器

#### `docker run -d -p 80:8080 --name myTomcat -v $PWD/webapp:/webapp tomcat/8 /bin/bash`

`-d` daemon，让容器在后台运行

`-p` port，端口映射，本机端口 -> 容器端口

`-P` port，端口映射，容器内部端口**随机**映射到主机的高端口

`--name` 指定名称

`-v` 挂载文件或目录，本机 -> 容器中

`-i` STDIN，标准输入

`-t` terminal，可交互终端

`name/tag` image name and tag, tag means version

> 启动MySQL，设置root密码：
>
> `docker run -p 3306:3306 --name mymysql -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6`

### 停止容器

#### `docker stop name/ID`

### 重启容器

#### `docker restart name/ID`

### 显示正在运行的容器

#### `docker ps`

`-a` all， 全部

`-l` latest，最后创建的容器

### 显示容器端口

#### `docker port name/ID`

### 移除容器
#### `docker rm name/ID`

### 删除镜像

#### `docker rmi imageID`

### [Docker命令大全](http://www.runoob.com/docker/docker-command-manual.html)

### 进入容器

#### `docker exec -it 775c7c9ee1e1 /bin/bash`

### Linux查看端口占用

#### `netstat -tunlp`



