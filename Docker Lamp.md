---
title: Docker Lamp
date: 2021-08-06 18:50:36
tags:
---

1. #### 查询并获取mysql镜像(如果docker版本比较新也可以直接尝试 pull mysql5.7,这里我用的5.7)

   ```
   docker search mysq:5.7
   docker pull 镜像名
   ```
   
2. #### 查询并获取php-apache镜像

   ```
   docker pull php:7.3-apache
   ```

3. #### 查看镜像(这里面会展示镜像各种信息)

   ```
   docker images
   ```

4. #### 创建自定义网络(可选)

   ```
   docker network create lamp  #创建lamp网络
   docker network ls			#查看所有网络
   ```

   

5. #### 创建一个脚本用来运行

   ```
   vim lamp.sh
   ```

6. #### 编辑脚本函数(win直接输入命令即可,这里是为了后续使用)

   --name	 为命名 (非必填只是为了方便后续不用每次都用id)

   --net	 为使用的网络 (非必填,这里我使用的自定义lamp网络,默认为桥接)

   -p 		 为端口映射 (必填,docker虚拟端口:宿主机端口)

   -v		 挂载目录 (非必填,但是如果不挂载,没办法保存数据,主机目录:docker虚拟机目录)

   -d 		 以daemon启动(必填,所有服务类必填)

   !!!注意下面的格式,不要随意变动

   ```
   #!/bin/bash
   mysql()
   {
   	docker run --name my_mysql --net lamp -p 3306:3306 \
   	-v ~/DockerVendor/mysql/data:/var/lib/mysql \
   	-v ~/DockerVendor/mysql/conf:/etc/mysql/conf.d \
   	-v ~/DockerVendor/mysql/logs:/logs \
   	-e MYSQL_ROOT_PASSWORD=ppnn13% \		   #设置root默认密码
   	-d mysql:5.7 --character-set-server=utf8   #使用utf8编码(选填)
   }
   apache()
   {
   	docker run --name my_apache --net lamp -p 80:80 \
   	-v ~/DockerVendor/lamp/httpd/conf:/etc/apache2/sites-enabled \
   	-v ~/DockerVendor/lamp/www:/var/www/html \
   	-v ~/DockerVendor/lamp/httpd/logs:/var/log/apache2 \
   	-d php:7.3-apache
   }
   $1
   
   ```

7. #### 现在就可以运行脚本了

   ```
   sh lamp.sh apache
   sh lamp.sh mysql
   ```

8. #### 页面会输出运行的容器id|名称如果想进入容器可以使用下面的命令

   ```
   docker exec -it 容器id|自定义名称 /bin/bash
   ```

9. #### 镜像有时候未安装拓展,如何安装?

   进入容器

   进入/usr/local/bin查看一下安装配置工具,这里可以看到docker-php-*,这些就是docker管理php的

   官方拓展可以这么安装

   ```
   docker-php-ext-install 拓展名
   #ocker-php-ext-install -j$(nproc) bcmath calendar exif gettext sockets dba mysqli pcntl pdo_mysql shmop sysvmsg sysvsem sysvshm
   ```

   这一步已经安装完了,并且已经启动,如果未启动可以执行下面的命令

   ```
   docker-php-ext-enable 拓展名
   ```

   如果想安装其他拓展,参考linux安装php拓展

   注: docker-php-source extract可以生成php源码包		

   卸载拓展要这样

   ```
   rm -rf /usr/local/etc/php/conf.d/docker-php-ext-拓展名.ini
   ```

10. mysql部分就很简单了,直接参考上面命令(docker运行的都是虚拟机,一个服务就是一个虚拟机)
