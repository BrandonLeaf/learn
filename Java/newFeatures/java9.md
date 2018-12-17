Java 9 新特性
===
更新时间:2018.12.17


目录
---
<!-- TOC depthFrom:2 updateOnSave:false -->

- [Java 9 新特性](#java-9-%E6%96%B0%E7%89%B9%E6%80%A7)
  - [目录](#%E7%9B%AE%E5%BD%95)
  - [参考文档](#%E5%8F%82%E8%80%83%E6%96%87%E6%A1%A3)
  - [1. 模块系统Jigsaw 项目](#1-%E6%A8%A1%E5%9D%97%E7%B3%BB%E7%BB%9Fjigsaw-%E9%A1%B9%E7%9B%AE)
  - [2. HTTP/2](#2-http2)
  - [3. JShell](#3-jshell)
  - [4. 不可变集合工厂方法](#4-%E4%B8%8D%E5%8F%AF%E5%8F%98%E9%9B%86%E5%90%88%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95)
  - [5. 私有接口方法](#5-%E7%A7%81%E6%9C%89%E6%8E%A5%E5%8F%A3%E6%96%B9%E6%B3%95)
  - [6. HTML5风格的Java帮助文档](#6-html5%E9%A3%8E%E6%A0%BC%E7%9A%84java%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3)
  - [7. 多版本兼容 JAR](#7-%E5%A4%9A%E7%89%88%E6%9C%AC%E5%85%BC%E5%AE%B9-jar)
  - [8. java9的垃圾收集机制](#8-java9%E7%9A%84%E5%9E%83%E5%9C%BE%E6%94%B6%E9%9B%86%E6%9C%BA%E5%88%B6)
  - [9. I/O 流新特性](#9-io-%E6%B5%81%E6%96%B0%E7%89%B9%E6%80%A7)

<!-- /TOC -->


参考文档
---

多数内容摘录于[JDK 9新特性汇总](https://zhuanlan.zhihu.com/p/29589033)，感谢所有的知识分享者。

* [OpenJDK-JDK9 更新内容](http://openjdk.java.net/projects/jdk9/)
* [Java9模块化遇坑](https://yq.aliyun.com/articles/618778#21)
* [The State of the Module System](http://openjdk.java.net/projects/jigsaw/spec/sotms/?spm=a2c4e.11153940.blogcont618778.8.2ab539ad9FxAT4#automatic-modules)
* [Java 9 逆天的十大新特性](https://blog.csdn.net/mxw2552261/article/details/79080678 )
* [JDK 9新特性汇总](https://zhuanlan.zhihu.com/p/29589033)
* [Java9新特性之——JShell](https://www.cnblogs.com/kode/p/7581943.html)



## 1. 模块系统[Jigsaw 项目](Modularity System)

Java 9中主要的变化是已经实现的模块化系统。

Modularity提供了类似于OSGI框架的功能，模块之间存在相互的依赖关系，可以导出一个公共的API，并且隐藏实现的细节，Java提供该功能的主要的动机在于，减少内存的开销，在JVM启动的时候，至少会有30～60MB的内存加载，主要原因是JVM需要加载rt.jar，不管其中的类是否被classloader加载，第一步整个jar都会被JVM加载到内存当中去，模块化可以根据模块的需要加载程序运行需要的class。

在引入了模块系统之后，JDK 被重新组织成 94 个模块。Java 应用可以通过新增的 jlink 工具，创建出只包含所依赖的 JDK 模块的自定义运行时镜像。这样可以极大的减少 Java 运行时环境的大小。使得JDK可以在更小的设备中使用。采用模块化系统的应用程序只需要这些应用程序所需的那部分JDK模块，而非是整个JDK框架了。

## 2. HTTP/2

JDK9之前提供HttpURLConnection API来实现Http访问功能，但是这个类基本很少使用，一般都会选择Apache的Http Client，此次在Java 9的版本中引入了一个新的package:java.net.http，里面提供了对Http访问很好的支持，不仅支持Http1.1而且还支持HTTP2（什么是HTTP2？请参见HTTP2的时代来了...），以及WebSocket，据说性能特别好。

注意：新的 HttpClient API 在 Java 9 中以所谓的孵化器模块交付。也就是说，这套 API 不能保证 100% 完成。

## 3. JShell

JShell的目标是提供一个交互工具，通过它来运行和计算java中的表达式。开发者可以轻松地与JShell交互，其中包括：编辑历史，tab键代码补全，自动添加分号，可配置的imports和definitions。其他的很多主流编程语言如python都已经提供了console，便于编写一些简单的代码用于测试。值得一提的是，JShell并不是提供了新的一个交互语言，在JShell中编写的所有代码都必须符合java语言规范；图形界面和调试支持也没有，JShell的一个目标是可以在IDE中使用JShell交互，而不是实现IDE实现的功能。

## 4. 不可变集合工厂方法


Java 9增加了List.of()、Set.of()、Map.of()和Map.ofEntries()等工厂方法来创建不可变集合。
除了更短和更好阅读之外，这些方法也可以避免您选择特定的集合实现。在创建后，继续添加元素到这些集合会导致 “UnsupportedOperationException” 。

```java
 List strs = List.of("Hello", "World");
 List strs List.of(1, 2, 3);
 Set strs = Set.of("Hello", "World");
 Set ints = Set.of(1, 2, 3);
 Map maps = Map.of("Hello", 1, "World", 2);
```

## 5. 私有接口方法

Java 8 为我们提供了接口的默认方法和静态方法，接口也可以包含行为，而不仅仅是方法定义。

默认方法和静态方法可以共享接口中的私有方法，因此避免了代码冗余，这也使代码更加清晰。如果私有方法是静态的，那这个方法就属于这个接口的。并且没有静态的私有方法只能被在接口中的实例调用。

```java

interface InterfaceWithPrivateMethods {

    private static String staticPrivate() {

        return "static private";

    }

    private String instancePrivate() {

        return "instance private";

    }

    default void check() {    

        String result = staticPrivate();

        InterfaceWithPrivateMethods pvt = new InterfaceWithPrivateMethods() {

            // anonymous class 匿名类

        };

        result = pvt.instancePrivate();

    }

}

```

## 6. HTML5风格的Java帮助文档


## 7. 多版本兼容 JAR

当一个新版本的 Java 出现的时候，你的库用户要花费很长时间才会切换到这个新的版本。这就意味着库要去向后兼容你想要支持的最老的 Java 版本 (许多情况下就是 Java 6 或者 7)。这实际上意味着未来的很长一段时间，你都不能在库中运用 Java 9 所提供的新特性。幸运的是，多版本兼容 JAR 功能能让你创建仅在特定版本的 Java 环境中运行库程序时选择使用的 class 版本

```
multirelease.jar

├── META-INF

│   └── versions

│       └── 9

│           └── multirelease

│               └── Helper.class

├── multirelease

├── Helper.class

└── Main.class
```

在上述场景中， multirelease.jar 可以在 Java 9 中使用, 不过 Helper 这个类使用的不是顶层的 multirelease.Helper 这个 class, 而是处在“META-INF/versions/9”下面的这个。这是特别为 Java 9 准备的 class 版本，可以运用 Java 9 所提供的特性和库。同时，在早期的 Java 诸版本中使用这个 JAR 也是能运行的，因为较老版本的 Java 只会看到顶层的这个 Helper 类。

## 8. java9的垃圾收集机制

Java 9 移除了在 Java 8 中 被废弃的垃圾回收器配置组合，同时把G1设为默认的垃圾回收器实现。替代了之前默认使用的Parallel GC，对于这个改变，evens的评论是酱紫的：这项变更是很重要的，因为相对于Parallel来说，G1会在应用线程上做更多的事情，而Parallel几乎没有在应用线程上做任何事情，它基本上完全依赖GC线程完成所有的内存管理。这意味着切换到G1将会为应用线程带来额外的工作，从而直接影响到应用的性能


## 9. I/O 流新特性


java.io.InputStream 中增加了新的方法来读取和复制 InputStream 中包含的数据。

    readAllBytes：读取 InputStream 中的所有剩余字节。

    readNBytes： 从 InputStream 中读取指定数量的字节到数组中。

    transferTo：读取 InputStream 中的全部字节并写入到指定的 OutputStream 中 。

