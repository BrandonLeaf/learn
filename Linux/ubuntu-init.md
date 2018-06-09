# Ubuntu 18.04 安装后
更新时间: 2018.06.06

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [推荐文章](#推荐文章)
- [设置](#设置)
- [安装必要软件](#安装必要软件)
- [常用软件](#常用软件)
    - [git](#git)
    - [jdk](#jdk)
    - [gradle](#gradle)
    - [SQL Developer安装](#sql-developer安装)
        - [方法一](#方法一)
        - [方法二](#方法二)

<!-- /TOC -->

## 推荐文章

+ [Ubuntu18.04 新特性，装好需要做的11件事情](https://blog.csdn.net/sinat_35121480/article/details/80141383)


## 设置

+ 更新软件
```sh
sudo update
sudo upgrade
```

+ 最小化/最大化
```sh
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

## 安装必要软件

```sh
# vim编辑器
sudo apt install vim
# 中文输入法
sudo apt install ibus-pinyin
```

## 常用软件

+ 官网下载
    1. [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    2. [gradle](http://services.gradle.org/distributions/)
    3. [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)
    4. [VSCode](https://code.visualstudio.com/)
    5. [idea](https://www.jetbrains.com/idea/)

### git

```sh
sudo apt install git
```

### jdk


+ 文件存放路经
```sh
#建议存放在/opt目录下，并使用软链接
ln -s jdk-1.8/ default_jdk
```
> 如：/opt/jdk/default_jdk

+ 环境变量配置

```sh
sudo vi /etc/profile
#添加以下内容
#-------------jdk--------------
export JAVA_HOME=/opt/jdk/default_jdk
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

### gradle

+ 文件存放路经
```sh
#建议存放在/opt目录下，并使用软链接
ln -s gradle-4.8/ default_gradle
```
> 如：/opt/gradle/default_gradle


+ 环境变量配置

```sh
sudo vi /etc/profile
#添加以下内容
#-------------gradle-----------
export GRADLE_HOME=/opt/gradle/default_gradle
export PATH=$GRADLE_HOME/bin:$PATH
```

### SQL Developer安装

#### 方法一
+ [下载rpm包](http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html)
    + 安装依赖
        ```sh
        sudo apt-get install tar
        sudo apt-get install alien
        ```
    + 转换rpm,并安装deb
        ```sh
        sudo alien -cv sqldeveloper-2.1.1.64.45-1.noarch.rpm
        sudo dpkg -i sqldeveloper_2.1.1.64.45-2_all.deb
        ```
#### 方法二

+ [下载OracleSQL Developer for other platforms](http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html)
    + 解压
        ```sh
        unzip
        ```
    + 添加JAVA_HOME路径
        ```sh
        vi sqldeveloper/ide/bin/launcher.sh
        if["X$APP_JAVA_HOME"!="X" ]
        then
            if [!-d${APP_JAVA_HOME} ]
            then
            APP_JAVA_HOME="/home/xxx/jdk1.8.0_45"
            fi
        fi
        ```