# Docker 学习
更新时间: 2018.06.09

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [基础命令](#基础命令)
    - [镜像](#镜像)
    - [容器](#容器)
    - [全局](#全局)
- [应用实例](#应用实例)
    - [gitlab](#gitlab)
    - [MYSQL](#mysql)

<!-- /TOC -->

## 基础命令

### 镜像

1. 查看本地镜像

    ```sh
    docker images
    ```

1. 从镜像仓库中搜索镜像

    ```sh
    docker search <镜像名称>
    ```

1. 从镜像仓库中拉取镜像

    ```sh
    docker pull <镜像名称>
    ```

1. 从本地仓库中删除镜像

    ```sh
    docker rmi <镜像名称>
    ```

1. 创建镜像

    + 方式1：在现有道镜像中经过修改创建新镜像
        ```sh
        docker commit <容器ID> <镜像名称:版本号>
        #参数
        #-m:提交的描述信息 
        #-a:指定镜像作者
        ```
        
    + 方式2：dockerfile
        ```sh
        docker build <dockerfile文件路经>
        #参数
        #-t:指定要创建的目标镜像名,注意名称只能是小写字母
        ```

        + 文本格式如下

            ```sh
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
            >**FROM** <镜像名称> :基于某linux镜像做修改   
            **MAINTAINER** <作者> <邮箱>: 作者 邮箱   
            **RUN** <命令> ：执行的命令   
            **EXPOSE** <端口号> ：对外开发的端口   
            **CMD** <命令> ：用于设置镜像运行后的启动命令

### 容器

1. 创建容器

    ```sh
    docker create <镜像名称>  
    #参数
    #-p 25010:8080：是容器内部端口绑定到指定的主机端口  
    #-v $PWD/logs:/logs：将主机当前目录下的 logs 目录挂载到容器的 /logs
    ```

1. 创建并运行容器

    ```sh
    docker run <镜像名称>  
    #参数
    #-p 25010:8080：是容器内部端口绑定到指定的主机端口  
    #-v $PWD/logs:/logs：将主机当前目录下的 logs 目录挂载到容器的 /logs
    ```

1. 删除容器

    ```sh
    docker rm <容器ID/Name>
    ```

1. 启动容器

    ```sh
    docker start <容器ID/Name>
    ```

1. 暂停容器

    ```sh
    docker pause <容器ID/Name>
    ```

1. 停止容器

    ```sh
    docker stop <容器ID/Name>
    ```

1. 进入容器

    ```sh
    docker exec -it <容器ID/Name> <命令>
    ```

1. 查看容器映射的端口

    ```sh
    docker port <容器ID/Name>
    ```

1. 查看容器ip

    ```sh
    docker inspect --format '{{ .NetworkSettings.IPAddress }}' <容器ID/Name>
    ```

1. 查看容器详细信息

    ```sh
    docker inspect <容器ID/Name>
    ```

### 全局

1. 查看容器

    ```sh
    #查看正在运行端容器
    docker ps 
    #查看所有容器
    docker ps -a
    ```

1. 查看docker资源管理器

    ```sh
    docker stats
    ```

## 应用实例

### gitlab

+ 拉取镜像

    ```sh
    docker pull gitlab/gitlab-ce
    ```

+ 配置启动

    ```sh
    sudo docker run -d \
        --hostname gitlab.example.com \
        -p 443:443 \
        -p 80:80 \
        -p 22:22 \
        --name gitlab \
        --restart always \
        -v $PWD/gitlab/conf:/etc/gitlab \
        -v $PWD/gitlab/logs:/var/log/gitlab \
        -v $PWD/gitlab/data:/var/opt/gitlab \
        gitlab/gitlab-ce:latest
    ```

### MYSQL


+ 拉取镜像

    ```sh
    docker pull mysql:5.7
    ```

+ 配置启动

    ```sh
    sudo docker run -d \
        -p 3306:3306 \
        --name mysql57_1 \
        --restart always \
        -v $PWD/mysql57_1/config:/etc/mysql \
        -v $PWD/mysql57_1/logs:/logs \
        -v $PWD/mysql57_1/data:/var/lib/mysql \
        -e MYSQL_ROOT_PASSWORD=123456 \
        mysql:5.7
    ```

+ 附
    + 进入容器，进入MySQL 密码设置
        ```sql
        set password for root@localhost = password('123');
        ```