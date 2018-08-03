# Java IO 编程
更新时间：2018.08.03

目录
---
<!-- TOC depthFrom:2 updateOnSave:false -->


- [Java IO 编程](#java-io-%E7%BC%96%E7%A8%8B)
    - [目录](#%E7%9B%AE%E5%BD%95)
    - [Java IO 模型](#java-io-%E6%A8%A1%E5%9E%8B)
    - [IO流](#io%E6%B5%81)
        - [字节流](#%E5%AD%97%E8%8A%82%E6%B5%81)
            - [InputStream](#inputstream)
                - [FileInputStream](#fileinputstream)
                - [FilterInputStream](#filterinputstream)
                    - [BufferedInputStream](#bufferedinputstream)
                    - [DataInputStream](#datainputstream)
                    - [PushbakInputStream](#pushbakinputstream)
                - [ObjectInputStream](#objectinputstream)
                - [PipedInputStream](#pipedinputstream)
                - [SequenceInputStream](#sequenceinputstream)
                - [StringBufferInputStream](#stringbufferinputstream)
                - [ByteArrayInputStream](#bytearrayinputstream)
            - [OutputStream](#outputstream)
                - [FileOutputStream](#fileoutputstream)
                - [FilterOutputStream](#filteroutputstream)
                    - [BufferedOutputStream](#bufferedoutputstream)
                    - [DataOutputStream](#dataoutputstream)
                    - [PrintStream](#printstream)
                - [ObjectOutputStream](#objectoutputstream)
                - [PipedOutputStream](#pipedoutputstream)
                - [ByteArrayOutputStream](#bytearrayoutputstream)
        - [字符流](#%E5%AD%97%E7%AC%A6%E6%B5%81)
            - [Reader](#reader)
                - [BufferedReader](#bufferedreader)
                - [InputStreamReader](#inputstreamreader)
                - [StringReader](#stringreader)
                - [PipedReader](#pipedreader)
                - [ByteArrayReader](#bytearrayreader)
                - [FilterReader](#filterreader)
                    - [PushbackReader](#pushbackreader)
            - [Writer](#writer)
                - [BufferedWriter](#bufferedwriter)
                - [OutputStreamWriter](#outputstreamwriter)
                    - [FileWriter](#filewriter)
                - [PrinterWriter](#printerwriter)
                - [StringWriter](#stringwriter)
                - [PipedWriter](#pipedwriter)
                - [CharArrayReader](#chararrayreader)
                - [FilterWriter](#filterwriter)
    - [非流式](#%E9%9D%9E%E6%B5%81%E5%BC%8F)
        - [File](#file)
    - [其他](#%E5%85%B6%E4%BB%96)
        - [RandomAccessFile](#randomaccessfile)

<!-- /TOC -->

---

## Java IO 模型

Java的IO模型设计非常优秀，它使用Decorator(装饰者)模式，按功能划分Stream，您可以动态装配这些Stream，以便获得您需要的功能。

## IO流

* 输入流

    程序从输入流读取数据源。数据源包括外界(键盘、文件、网络…)，即是将数据源读入到程序的通信通道

    输入流是**只读操作**

* 输出流

    程序向输出流写入数据。将程序中的数据输出到外界（显示器、打印机、文件、网络…）的通信通道。

    输出流是**只写操作**

### 字节流

数据流中最小的数据单元是字节

#### InputStream

InputStream是所有的输入字节流的父类，它是一个抽象类。

##### FileInputStream

##### FilterInputStream

###### BufferedInputStream

###### DataInputStream

###### PushbakInputStream

##### ObjectInputStream

##### PipedInputStream

##### SequenceInputStream

##### StringBufferInputStream

##### ByteArrayInputStream


#### OutputStream

##### FileOutputStream

##### FilterOutputStream

###### BufferedOutputStream

###### DataOutputStream

###### PrintStream

##### ObjectOutputStream

##### PipedOutputStream

##### ByteArrayOutputStream

### 字符流

数据流中最小的数据单元是字符， Java中的字符是Unicode编码，一个字符占用两个字节。

字符流的由来： Java中字符是采用Unicode标准，一个字符是16位，即一个字符使用两个字节来表示。为此，JAVA中引入了处理字符的流。因为数据编码的不同，而有了对字符进行高效操作的流对象。本质其实就是基于字节流读取时，去查了指定的码表。

#### Reader

##### BufferedReader

##### InputStreamReader

##### StringReader

##### PipedReader

##### ByteArrayReader

##### FilterReader

###### PushbackReader

#### Writer

##### BufferedWriter

##### OutputStreamWriter

###### FileWriter

##### PrinterWriter

##### StringWriter

##### PipedWriter

##### CharArrayReader

##### FilterWriter

## 非流式

### File

## 其他

### RandomAccessFile