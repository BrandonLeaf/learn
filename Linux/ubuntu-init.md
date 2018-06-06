# Ubuntu 18.04 安装后
更新时间: 2018.06.06

## 更新软件
```sh
sudo update
sudo upgrade
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
    1. JDK
    2. gradle
    3. MySQL Workbench
    4. VSCode
    5. idea

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