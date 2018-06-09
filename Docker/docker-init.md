# Docker 安装与使用
更新时间:2018.06.08

+ 参考官网安装 
    + [点击查看官网](https://docs.docker.com/install/)

+ 配置国内镜像

    ```sh
    sudo vi /etc/docker
    { 
    “registry-mirrors”: [“https://registry.docker-cn.com“] 
    }
    ```
    
+ 重启

    ```sh
    systemctl daemon-reload 
    systemctl restart docker
    ```

+ 加入docker 用户组

    ```sh
    sudo gpasswd -a ${USER} docker
    ```