Tomcat 配置HTTP2.0 与 APR运行模式
===
更新时间:2018.11.13

目录
---
<!-- TOC depthFrom:2 updateOnSave:true -->

- [参考文章](#参考文章)
- [证书生成](#证书生成)
- [https & http2.0 配置 默认nio处理模式](#https--http20-配置-默认nio处理模式)
- [开启apr处理模式](#开启apr处理模式)
    - [安装apr,native](#安装aprnative)
    - [修改tomcat](#修改tomcat)
- [http 自动跳转 https](#http-自动跳转-https)

<!-- /TOC -->

## 参考文章

* [Tomcat9 配置HTTPS连接](https://blog.csdn.net/u013360850/article/details/70665312)
* [Tomcat9配置HTTP2](https://blog.csdn.net/mn960mn/article/details/51602529)
* [tomcat bio nio apr 模式性能测试与个人看法](https://blog.csdn.net/wanglei_storage/article/details/50225779)
* [JDK自带工具keytool生成ssl证书](https://www.cnblogs.com/zhangzb/p/5200418.html)
* [Java 9 和Spring Boot 2.0纷纷宣布支持的HTTP/2到底是什么？](http://ju.outofmemory.cn/entry/346601)
* [Spring Boot 2.0中嵌入式Web容器（如Tomcat）对HTTP2的支持详解](https://blog.csdn.net/taiyangdao/article/details/80977910)
* [HTTP2.0 介绍以及ngnix、tomcat下如何配置](https://blog.csdn.net/qq_16320025/article/details/79495469)
* [Tomcat Connector三种运行模式（BIO, NIO, APR）的比较和优化](https://www.cnblogs.com/nb-blog/p/5278933.html)


## 证书生成

使用openssl生成证书
```sh
cd apache-tomcat-9.0.8/conf
mkdir cert
cd cert
openssl genrsa -out ca.key 2048  
openssl rsa -in ca.key -out ca.key
openssl req -new -x509 -key ca.key -out ca.crt -days 3650 
```

## https & http2.0 配置 默认nio处理模式
vi apache-tomcat-9.0.8/conf/server.xml
```xml
#--------------修改以下内容-----------
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true" >
        <!-- http2.0 配置  tomcat9支持，不使用注释即可-->
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
                <Certificate certificateKeyFile="conf/cert/ca.key"
                        certificateFile="conf/cert/ca.crt"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
```


## 开启apr处理模式

### 安装apr,native

安装编译工具
```sh
# ---ubuntu----
sudo apt-get install libapr1-dev libssl-dev apt-file ant
```

编译&安装native
```sh
./configure \ 
--with-apr=/usr/bin/apr-config \ 
--with-java-home=/usr/lib/jvm/java-11-openjdk-amd64 \ 
--with-ssl=yes \ 
--prefix=/home/cpicapp/Workspace/Cpic/tomcatWar/apache-tomcat-9.0.8

sudo make && sudo make install
```
### 修改tomcat

vi apache-tomcat-9.0.8/bin/catalina.sh

```sh
# ---------------linux----------------------
# ---------------添加配置------------------------
JAVA_OPTS="$JAVA_OPTS -Djava.library.path=/usr/local/apr/lib"
# OS specific support.  $var _must_ be set to either true or false.
```

vi apache-tomcat-9.0.8/conf/server.xml
```xml
#--------------修改以下内容-----------
<Connector port="8443" protocol="org.apache.coyote.http11.Http11AprProtocol"
               maxThreads="150" SSLEnabled="true" >
        <!-- http2.0 配置-->
        <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" />
        <SSLHostConfig>
                <Certificate certificateKeyFile="conf/cert/ca.key"
                        certificateFile="conf/cert/ca.crt"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
```

## http 自动跳转 https

```xml
# ----web.xml文件，在该文件</welcome-file-list>后面加上这样一段：
<security-constraint>
       <web-resource-collection >
              <web-resource-name >SSL</web-resource-name>
              <url-pattern>/*</url-pattern>
       </web-resource-collection>

       <user-data-constraint>
              <transport-guarantee>CONFIDENTIAL</transport-guarantee>
       </user-data-constraint>
</security-constraint>
```