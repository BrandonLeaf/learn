# Gradle 
更新时间：2018.08.23

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [初始化](#初始化)

<!-- /TOC -->

---


## 初始化

自带类型以下类型

* basic
* groovy-application
* groovy-library
* java-application
* java-library
* pom
* scala-library
  
```sh
gradle init --type java-application
```

以上面命令为例，执行后生成以下结构

```
.
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   └── java
    │       └── App.java
    └── test
        └── java
            └── AppTest.java
```
上面文件中

* **gradlew**/**gradlew.bat** 分别是linux、windows下的可执行文件，可不安装gradle的情况下直接使用gradle
* **gradle** 文件夹是gradle源文件
* **settings.gradle** 是gradle的配置文件，可配置代理上网