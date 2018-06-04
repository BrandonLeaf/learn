# Docker知识



## 基础操作

### 镜像

1. 查看本地镜像

    > **docker images**

1. 从镜像仓库中搜索镜像

    > **docker search** <镜像名称>

1. 从镜像仓库中拉取镜像

    > **docker pull** <镜像名称>

1. 从本地仓库中删除镜像

    > **docker rmi** <镜像名称>

1. 创建镜像

+ 方式1：在现有道镜像中经过修改创建新镜像

    > **docker commit** <容器ID> <镜像名称:版本号>
    + 参数
        > **-m** :提交的描述信息   
        > **-a** :指定镜像作者

    + 方式2：dockerfile

        > **docker build** <dockerfile文件路经>

        + 参数
            > **-t** ：指定要创建的目标镜像名,注意名称只能是小写字母

        + 文本格式如下

        ```shell
            FROM    centos:6.7  
            MAINTAINER      Macy "five3@163.com"  
            RUN     /bin/echo 'root:123456' |chpasswd  
            RUN     useradd five3  
            RUN     /bin/echo 'five3:123456' |chpasswd  
            RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local  
            EXPOSE  22  
            EXPOSE  80  
            CMD     /usr/sbin/sshd -D

        ```
        + 说明
        > **FROM** <镜像名称> :基于某linux镜像做修改   
        > **MAINTAINER** <作者> <邮箱>: 作者 邮箱   
        > **RUN** <命令> ：执行的命令   
        > **EXPOSE** <端口号> ：对外开发的端口   
        > **CMD** <命令> ：用于设置镜像运行后的启动命令

### 容器

1. 创建容器

    > **docker create** <镜像名称>  
    + 常用参数
        > **-p 25010:8080** ：是容器内部端口绑定到指定的主机端口  
        > **-v $PWD/logs:/logs** ：将主机当前目录下的 logs 目录挂载到容器的 /logs。 
    

1. 创建并运行容器

    > **docker run** <镜像名称>  
    + 常用参数
        > **-p 25010:8080** ：是容器内部端口绑定到指定的主机端口  
        > **-v $PWD/logs:/logs** ：将主机当前目录下的 logs 目录挂载到容器的 /logs。

1. 删除容器

    > **docker rm** <容器ID/Name>

1. 启动容器

    > **docker start** <容器ID/Name>

1. 暂停容器

    > **docker pause** <容器ID/Name>

1. 停止容器

    > **docker stop** <容器ID/Name>

1. 进入容器

    > **docker exec -it** <容器ID/Name> <命令>

1. 查看容器映射的端口

    > **docker port** <容器ID/Name>

1. 查看容器ip

    > **docker inspect --format '{{ .NetworkSettings.IPAddress }}'** <容器ID/Name>

1. 查看容器详细信息

    > **docker inspect** <容器ID/Name>

### 全局

1. 查看正在运行道容器

    > **docker ps**

1. 查看所有容器

    > **docker ps -a**

1. docker资源管理器

    > **docker stats**

## 进阶

### 关于docker的网络

> 待补充

### 关于docker的架构

> 待补充

## 应用实例

### gitlab

+ 拉取镜像
```shell
docker pull gitlab/gitlab-ce
```

+ 配置启动

### MYSQL

+ 密码设置
    ```sql
    set password for root@localhost = password('123');
    ```

### TOMCAT

> 待补充

### REDIS

> 待补充

### NGINS

> 待补充

### 负载均衡

> 待补充
