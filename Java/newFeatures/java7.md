# JAVA 7 新特性
更新时间 2018.07.26

<!-- TOC depthFrom:2 updateOnSave:true -->

- [switch中添加对String类型的支持](#switch中添加对string类型的支持)
- [泛型实例化类型自动推断](#泛型实例化类型自动推断)
- [集合初始化语法](#集合初始化语法)
- [新增一些取环境信息的工具方法](#新增一些取环境信息的工具方法)
- [Boolean类型反转，空指针安全,参与位运算](#boolean类型反转空指针安全参与位运算)
- [char对比](#char对比)
- [安全的加减乘除](#安全的加减乘除)
- [二进制字面量](#二进制字面量)
- [在数字字面量使用下划线](#在数字字面量使用下划线)
- [try-with-resources 资源的自动管理](#try-with-resources-资源的自动管理)
- [catch块处理多个异常](#catch块处理多个异常)

<!-- /TOC -->

## switch中添加对String类型的支持

```java
switch (gender) {  
    case "男":  
        title = name + " 先生";  
        break;  
    case "女":  
        title = name + " 女士";  
        break;  
    default:  
        title = name;  
}  
```

## 泛型实例化类型自动推断

```java
List<String> tempList = new ArrayList<>();
```

## 集合初始化语法

```java
List<Integer> piDigits = [ 1,2,3,4,5,8 ]; 
String item = list[0];

Set<String> set = {"item"};

Map<String, Integer> map = {"key" : 1};
int value = map["key"];
```

## 新增一些取环境信息的工具方法

```java
File System.getJavaIoTempDir() // IO临时文件夹  
File System.getJavaHomeDir() // JRE的安装目录  
File System.getUserHomeDir() // 当前用户目录  
File System.getUserDir() // 启动java进程时所在的目录5  
```

## Boolean类型反转，空指针安全,参与位运算

```java
Boolean Booleans.negate(Boolean booleanObj)  
True => False , False => True, Null => Null  
boolean Booleans.and(boolean[] array)  
boolean Booleans.or(boolean[] array)  
boolean Booleans.xor(boolean[] array)  
boolean Booleans.and(Boolean[] array)  
boolean Booleans.or(Boolean[] array)  
boolean Booleans.xor(Boolean[] array) 
```

## char对比

```java
boolean Character.equalsIgnoreCase(char ch1, char ch2)
```

## 安全的加减乘除 

```java
int Math.safeToInt(long value)  
int Math.safeNegate(int value)  
long Math.safeSubtract(long value1, int value2) 
long Math.safeSubtract(long value1, long value2)
int Math.safeMultiply(int value1, int value2)  
long Math.safeMultiply(long value1, int value2)
long Math.safeMultiply(long value1, long value2)
long Math.safeNegate(long value)  
int Math.safeAdd(int value1, int value2)  
long Math.safeAdd(long value1, int value2)  
long Math.safeAdd(long value1, long value2)  
int Math.safeSubtract(int value1, int value2)  
```

## 二进制字面量

```java
//一个8位'byte'值
byte aByte = (byte)0b00100001;
//一个16位'short'值
short aShort = (short)0b1010000101000101;
//一些32位'int'值
int anInt1 = 0b10100001010001011010000101000101;
int anInt2 = 0b101;
int anInt3 = 0B101; // B可以是大写也可以是小写.

//一个64位的'long'值. 注意"L"结尾:
long aLong = 0b1010000101000101101000010100010110100001010001011010000101000101L;
```

## 在数字字面量使用下划线

```java
long creditCardNumber = 1234_5678_9012_3456L;
long socialSecurityNumber = 999_99_9999L;
float pi =  3.14_15F;
long hexBytes = 0xFF_EC_DE_5E;
long hexWords = 0xCAFE_BABE;
long maxLong = 0x7fff_ffff_ffff_ffffL;
byte nybbles = 0b0010_0101;
long bytes = 0b11010010_01101001_10010100_10010010;
```

下划线只能出现在数字之间，下面的情形不能出现下划线

* 数字的开头和结尾
* 在浮点数中与小数点相邻
* F或者L后缀之前
* 在预期数字串的位置

```java
float pi1 = 3_.1415F;      // 无效; 不能和小数点相邻
float pi2 = 3._1415F;      // 无效; 不能和小数点相邻

long socialSecurityNumber1 = 999_99_9999_L;// 无效; 不能放在L后缀之前

int x1 = _52;              // 无效；这是个标识符，不是数字的字面量
int x2 = 5_2;              // OK
int x3 = 52_;              // 无效; 不能放在数字的结尾
int x4 = 5_______2;        // OK

int x5 = 0_x52;            // 无效; 不能放在 0x 中间 
int x6 = 0x_52;            // 无效; 不能放在数字的开头
int x7 = 0x5_2;            // OK
int x8 = 0x52_;            // 无效; 不能放在数字的结尾

int x9 = 0_52;             // OK 
int x10 = 05_2;            // OK
int x11 = 052_;            // Invalid; 不能放在数字的结尾
```

## try-with-resources 资源的自动管理

try结束后执行关闭括号中的资源

实现了**java.lang.AutoCloseable**或**java.io.Closeable**接口，视为一个资源

```java
try (
    BufferedReader br = new BufferedReader(new FileReader(path));
    ZipFile zf = new ZipFile(zipFileName);
    BufferedWriter writer = newBufferedWriter(outputFilePath, charset)
    ) {
    return br.readLine();
}
```

异常抛出

* readFirstLineFromFileWithFinallyBlock：由finally块抛出的异常
* readFirstLineFromFile：由try块抛出的异常

## catch块处理多个异常

```java
catch (IOException|SQLException ex) {
    logger.log(ex);
    throw ex;
}
```