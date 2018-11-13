linux 多版本JDK切换
===
更新时间：2018.10.31



# 版本配置-切换

```sh
update-alternatives --config java

-------执行结果---------

➜  idea update-alternatives --config java
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      auto mode
* 1            /opt/jdk/jdk1.8.0_171/bin/java                   700       manual mode
  2            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      manual mode
  3            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

Press <enter> to keep the current choice[*], or type selection number: 1
```


# 版本配置-添加

```sh
alternatives --install <系统bin路径> <命令> <JDK路径> <序号>
----------------
alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_171/bin/java  2
alternatives --install /usr/bin/javac javac /opt/jdk/jdk1.8.0_171/bin/javac  2
```