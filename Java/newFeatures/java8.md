# JAVA 8 新特性
更新时间 2018.07.26

<!-- TOC depthFrom:2 updateOnSave:true -->

- [Lambda表达式](#lambda表达式)
- [函数式接口](#函数式接口)
    - [@FunctionalInterface](#functionalinterface)
    - [Predicate接口](#predicate接口)
    - [Function 接口](#function-接口)
    - [Supplier 接口](#supplier-接口)
    - [Consumer 接口](#consumer-接口)
    - [Comparator 接口](#comparator-接口)
    - [Optional 接口](#optional-接口)
    - [Stream 接口](#stream-接口)
        - [Filter 过滤](#filter-过滤)
        - [Sort 排序](#sort-排序)
        - [Map 映射](#map-映射)
        - [Match 匹配](#match-匹配)
        - [Count 计数](#count-计数)
        - [Reduce 规约](#reduce-规约)
        - [并行Streams](#并行streams)
        - [Map](#map)
- [接口的默认方法和静态方法](#接口的默认方法和静态方法)
    - [默认方法](#默认方法)
    - [静态方法](#静态方法)
- [方法引用](#方法引用)
    - [构造器引用](#构造器引用)
    - [静态方法引用](#静态方法引用)
    - [某个类的成员方法的引用](#某个类的成员方法的引用)
    - [某个实例对象的成员方法的引用](#某个实例对象的成员方法的引用)
    - [参考例子](#参考例子)
- [Date API](#date-api)
    - [Clock 时钟](#clock-时钟)
    - [Timezones 时区](#timezones-时区)
    - [LocalTime 本地时间](#localtime-本地时间)
    - [LocalDate 本地日期](#localdate-本地日期)
    - [LocalDateTime 本地日期时间](#localdatetime-本地日期时间)
- [Annotation 多重注解](#annotation-多重注解)

<!-- /TOC -->

---

参考文档
--

* [java5、java6、java7、java8的新特性](https://blog.csdn.net/samjustin1/article/details/52268004)


## Lambda表达式

示例
```java
//最简单的Lambda表达式可由逗号分隔的参数列表、->符号和语句块组成，例如：
Arrays.asList( "a", "b", "d" ).forEach( e -> System.out.println( e ) );
//也可以显式指定该参数的类型
Arrays.asList( "a", "b", "d" ).forEach( ( String e ) -> System.out.println( e ) );
//使用花括号将该语句块括起来
Arrays.asList( "a", "b", "d" ).forEach( e -> {
    System.out.print( e );
    System.out.print( e );
} );
```
Lambda表达式可以引用类成员和局部变量（会将这些变量隐式得转换成final的），例如下列两个代码块的效果完全相同：

```java
String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
----
final String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
```

Lambda表达式有返回值，返回值的类型也由编译器推理得出。如果Lambda表达式中的语句块只有一行，则可以不用使用return语句，下列两个代码片段效果相同：

```java
Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> e1.compareTo( e2 ) );
----
Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> {
    int result = e1.compareTo( e2 );
    return result;
} );
```

## 函数式接口

### @FunctionalInterface  
```java
@FunctionalInterface  
interface Converter<F, T> {  
    T convert(F from);  
}  
Converter<String, Integer> converter = (from) -> Integer.valueOf(from);  
Integer converted = converter.convert("123");  
System.out.println(converted);    // 123  
```

### Predicate接口

Predicate 接口只有一个参数，返回boolean类型。该接口包含多种默认方法来将Predicate组合成其他复杂的逻辑（比如：与，或，非）：

```java
Predicate<String> predicate = (s) -> s.length() > 0;    
predicate.test("foo");              // true  
predicate.negate().test("foo");     // false   
Predicate<Boolean> nonNull = Objects::nonNull;  
Predicate<Boolean> isNull = Objects::isNull;   
Predicate<String> isEmpty = String::isEmpty;  
Predicate<String> isNotEmpty = isEmpty.negate();   
```

### Function 接口

Function 接口有一个参数并且返回一个结果，并附带了一些可以和其他函数组合的默认方法（compose, andThen）：

```java
Function<String, Integer> toInteger = Integer::valueOf;  
Function<String, String> backToString = toInteger.andThen(String::valueOf);    
backToString.apply("123");     // "123"   
```

### Supplier 接口

Supplier 接口返回一个任意范型的值，和Function接口不同的是该接口没有任何参数
```java
Supplier<Person> personSupplier = Person::new;  
personSupplier.get();   // new Person  
```

### Consumer 接口

Consumer 接口表示执行在单个参数上的操作。
```java
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);  
greeter.accept(new Person("Luke", "Skywalker"));  
```

### Comparator 接口

Comparator 是老Java中的经典接口， Java 8在此之上添加了多种默认方法：
```java
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);    
Person p1 = new Person("John", "Doe");  
Person p2 = new Person("Alice", "Wonderland");   
comparator.compare(p1, p2);             // > 0  
comparator.reversed().compare(p1, p2);  // < 0   
```

### Optional 接口

Optional 不是函数是接口，这是个用来防止NullPointerException异常的辅助类型，这是下一届中将要用到的重要概念

Optional 被定义为一个简单的容器，其值可能是null或者不是null。在Java 8之前一般某个函数应该返回非空对象但是偶尔却可能返回了null，而在Java 8中，不推荐你返回null而是返回Optional。

```java
Optional<String> optional = Optional.of("bam");    
optional.isPresent();           // true  
optional.get();                 // "bam"  
optional.orElse("fallback");    // "bam"   
optional.ifPresent((s) -> System.out.println(s.charAt(0)));     // "b" 
```

### Stream 接口

java.util.Stream 表示能应用在一组元素上一次执行的操作序列。Stream 操作分为**中间操作**或者**最终操作**两种，最终操作返回一特定类型的计算结果，而中间操作返回Stream本身，这样你就可以将多个操作依次串起来。Stream 的创建需要指定一个数据源，比如 java.util.Collection的子类，List或者Set， Map不支持。Stream的操作可以**串行执行**或者**并行执行**。

Java 8扩展了集合类，可以通过以下方式创建一个Stream。
* 串行执行：`Collection.stream()`  
* 并行执行：`Collection.parallelStream()` 

实例
```java
List<String> stringCollection = new ArrayList<>();  
stringCollection.add("ddd2");  
stringCollection.add("aaa2");  
stringCollection.add("bbb1");  
stringCollection.add("aaa1");  
stringCollection.add("bbb3");  
stringCollection.add("ccc");  
stringCollection.add("bbb2");  
stringCollection.add("ddd1"); 
```

#### Filter 过滤

_中间操作_

过滤通过一个predicate接口来过滤并只保留符合条件的元素，该操作属于中间操作，所以我们可以在过滤后的结果来应用其他Stream操作（比如forEach）。forEach需要一个函数来对过滤后的元素依次执行。forEach是一个最终操作，所以我们不能在forEach之后来执行其他Stream操作。

```java
stringCollection  
    .stream()  
    .filter((s) -> s.startsWith("a"))  
    .forEach(System.out::println);    
 // "aaa2", "aaa1"  
```

#### Sort 排序

_中间操作_

返回的是排序好后的Stream。如果你不指定一个自定义的Comparator则会使用默认排序。

```java
stringCollection  
    .stream()  
    .sorted()  
    .filter((s) -> s.startsWith("a"))  
    .forEach(System.out::println);    
 // "aaa1", "aaa2"   
```


#### Map 映射

_中间操作_

map会将元素根据指定的Function接口来依次将元素转成另外的对象，下面的示例展示了将字符串转换为大写字符串。你也可以通过map来讲对象转换成其他类型，map返回的Stream类型是根据你map传递进去的函数的返回值决定的。

```java
stringCollection  
    .stream()  
    .map(String::toUpperCase)  
    .sorted((a, b) -> b.compareTo(a))  
    .forEach(System.out::println);    
 // "DDD2", "DDD1", "CCC", "BBB3", "BBB2", "AAA2", "AAA1"  
```

#### Match 匹配

_最终操作_

Stream提供了多种匹配操作，允许检测指定的Predicate是否匹配整个Stream。所有的匹配操作都是最终操作，并返回一个boolean类型的值。

```java
boolean anyStartsWithA =   
    stringCollection  
        .stream()  
        .anyMatch((s) -> s.startsWith("a"));    
 System.out.println(anyStartsWithA);      // true   
 boolean allStartsWithA =   
    stringCollection  
        .stream()  
        .allMatch((s) -> s.startsWith("a"));   
 System.out.println(allStartsWithA);      // false   
 boolean noneStartsWithZ =   
    stringCollection  
        .stream()  
        .noneMatch((s) -> s.startsWith("z"));   
 System.out.println(noneStartsWithZ);      // true
```

#### Count 计数

_最终操作_

返回Stream中元素的个数，返回值类型是long

```java
long startsWithB =   
    stringCollection  
        .stream()  
        .filter((s) -> s.startsWith("b"))  
        .count();    
 System.out.println(startsWithB);    // 3   
```

#### Reduce 规约

允许通过指定的函数来讲stream中的多个元素规约为一个元素，规越后的结果是通过Optional接口表示的

```java
Optional<String> reduced =  
    stringCollection  
        .stream()  
        .sorted()  
        .reduce((s1, s2) -> s1 + "#" + s2);    
 reduced.ifPresent(System.out::println);  
// "aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2" 
```
>ps：证书加密属性拼接中可运用

#### 并行Streams

串行Stream上的操作是在一个线程中依次完成，而并行Stream则是在多个线程上同时执行。

下面的例子展示了是如何通过并行Stream来提升性能：

创建一个没有重复元素的大表：

```java
int max = 1000000;  
List<String> values = new ArrayList<>(max);  
for (int i = 0; i < max; i++) {  
    UUID uuid = UUID.randomUUID();  
    values.add(uuid.toString());  
}  
```

串行排序：

```java
long t0 = System.nanoTime();    
long count = values.stream().sorted().count();  
System.out.println(count);   
long t1 = System.nanoTime();   
long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);  
System.out.println(String.format("sequential sort took: %d ms", millis));  
//串行耗时: 899 ms
```

并行排序：

```java
long t0 = System.nanoTime();    
long count = values.parallelStream().sorted().count();  
System.out.println(count);   
long t1 = System.nanoTime();   
long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);  
System.out.println(String.format("parallel sort took: %d ms", millis));
// 并行排序耗时: 472 ms
```

上面两个代码几乎是一样的，但是并行版的快了50%之多，唯一需要做的改动就是将stream()改为parallelStream()

#### Map

Map类型不支持stream，不过Map提供了一些新的有用的方法来处理一些日常任务。

用例
```java
Map<Integer, String> map = new HashMap<>();    
 for (int i = 0; i < 10; i++) {  
    map.putIfAbsent(i, "val" + i);  
}
```

遍历Map

```java
map.forEach((id, val) -> System.out.println(val));
```

其他有用的函数
```java
map.computeIfPresent(3, (num, val) -> val + num);  
map.get(3);             // val33    
map.computeIfPresent(9, (num, val) -> null);  
map.containsKey(9);     // false   
map.computeIfAbsent(23, num -> "val" + num);  
map.containsKey(23);    // true   
map.computeIfAbsent(3, num -> "bam");  
map.get(3);             // val33   
map.getOrDefault(42, "not found");  // not found 
```

删除一个键值全都匹配的项

```java
map.remove(3, "val3");  
map.get(3);             // val33    
map.remove(3, "val33");  
map.get(3);             // null  
```

Map的元素做合并
```java
map.merge(9, "val9", (value, newValue) -> value.concat(newValue));  
map.get(9);             // val9    
map.merge(9, "concat", (value, newValue) -> value.concat(newValue));  
map.get(9);             // val9concat   
```

## 接口的默认方法和静态方法

### 默认方法
```java
private interface Defaulable {
    // Interfaces now allow default methods, the implementer may or 
    // may not implement (override) them.
    default String notRequired() { 
        return "Default implementation"; 
    }        
}
----
private static class DefaultableImpl implements Defaulable {
}
----
private static class OverridableImpl implements Defaulable {
    @Override
    public String notRequired() {
        return "Overridden implementation";
    }
}
```

### 静态方法
```java
private interface DefaulableFactory {
    // Interfaces now allow static methods
    static Defaulable create( Supplier< Defaulable > supplier ) {
        return supplier.get();
    }
}
```

## 方法引用

参照实例

```java
public static class Car {
    public static Car create( final Supplier< Car > supplier ) {
        return supplier.get();
    }              

    public static void collide( final Car car ) {
        System.out.println( "Collided " + car.toString() );
    }

    public void follow( final Car another ) {
        System.out.println( "Following the " + another.toString() );
    }

    public void repair() {   
        System.out.println( "Repaired " + this.toString() );
    }
}
```

### 构造器引用

语法：`Class::new` 或 `Class<T>::new`

无参数构造器

```java
final Car car = Car.create( Car::new );
```

### 静态方法引用

语法：`Class::static_method`

接受对应方法参数
```java
Arrays.sort(stringsArray,(s1,s2)->s1.compareToIgnoreCase(s2));
----
Arrays.sort(stringsArray, String::compareToIgnoreCase);
----
cars.forEach( Car::collide );
```

### 某个类的成员方法的引用

语法：`Class::method`

无参方法
```java
cars.forEach( Car::repair );
```

### 某个实例对象的成员方法的引用

语法：`instance::method`

接受对应方法的参数
```java
final Car police = Car.create( Car::new );
cars.forEach( police::follow );
```

### 参考例子

实体类
```java
class Person {  
    String firstName;  
    String lastName;    
     Person() {}   
     Person(String firstName, String lastName) {  
        this.firstName = firstName;  
        this.lastName = lastName;  
    }  
}   
```

接下来我们指定一个用来创建Person对象的对象工厂接口：
```java
interface PersonFactory<P extends Person> {  
    P create(String firstName, String lastName);  
}
```

这里我们使用构造函数引用来将他们关联起来，而不是实现一个完整的工厂：
```java
PersonFactory<Person> personFactory = Person::new;  
Person person = personFactory.create("Peter", "Parker"); 
```
我们只需要使用 Person::new 来获取Person类构造函数的引用，Java编译器会自动根据PersonFactory.create方法的签名来选择合适的构造函数。

## Date API

### Clock 时钟

Clock类提供了访问当前日期和时间的方法，Clock是时区敏感的，可以用来取代 System.currentTimeMillis() 来获取当前的微秒数。某一个特定的时间点也可以使用Instant类来表示，Instant类也可以用来创建老的java.util.Date对象。
```java
Clock clock = Clock.systemDefaultZone();  
long millis = clock.millis();    
Instant instant = clock.instant();  
Date legacyDate = Date.from(instant);   // legacy java.util.Date 
```

### Timezones 时区

在新API中时区使用ZoneId来表示。时区可以很方便的使用静态方法of来获取到。 时区定义了到UTS时间的时间差，在Instant时间点对象到本地日期对象之间转换的时候是极其重要的。

```java
System.out.println(ZoneId.getAvailableZoneIds());  
// prints all available timezone ids    
ZoneId zone1 = ZoneId.of("Europe/Berlin");  
ZoneId zone2 = ZoneId.of("Brazil/East");  
System.out.println(zone1.getRules());  
System.out.println(zone2.getRules());   
// ZoneRules[currentStandardOffset=+01:00]  
// ZoneRules[currentStandardOffset=-03:00] 
```

### LocalTime 本地时间

LocalTime 定义了一个没有时区信息的时间，例如 晚上10点，或者 17:30:15。下面的例子使用前面代码创建的时区创建了两个本地时间。之后比较时间并以小时和分钟为单位计算两个时间的时间差：
```java
LocalTime now1 = LocalTime.now(zone1);  
LocalTime now2 = LocalTime.now(zone2);    
 System.out.println(now1.isBefore(now2));  // false   
 long hoursBetween = ChronoUnit.HOURS.between(now1, now2);  
long minutesBetween = ChronoUnit.MINUTES.between(now1, now2);   
 System.out.println(hoursBetween);       // -3  
System.out.println(minutesBetween);     // -239  
```

LocalTime 提供了多种工厂方法来简化对象的创建，包括解析时间字符串。
```java
LocalTime late = LocalTime.of(23, 59, 59);  
System.out.println(late);       // 23:59:59    
DateTimeFormatter germanFormatter = 
    DateTimeFormatter  
        .ofLocalizedTime(FormatStyle.SHORT)  
        .withLocale(Locale.GERMAN);   
LocalTime leetTime = LocalTime.parse("13:37", germanFormatter);  
System.out.println(leetTime);   // 13:37   
```

### LocalDate 本地日期

LocalDate 表示了一个确切的日期，比如 2014-03-11。该对象值是不可变的，用起来和LocalTime基本一致。下面的例子展示了如何给Date对象加减天/月/年。另外要注意的是这些对象是不可变的，操作返回的总是一个新实例。

```java
LocalDate today = LocalDate.now();  
LocalDate tomorrow = today.plus(1, ChronoUnit.DAYS);  
LocalDate yesterday = tomorrow.minusDays(2);    
LocalDate independenceDay = LocalDate.of(2014, Month.JULY, 4);  
DayOfWeek dayOfWeek = independenceDay.getDayOfWeek();   
```

System.out.println(dayOfWeek);    // FRIDAY
从字符串解析一个LocalDate类型和解析LocalTime一样简单：
```java
DateTimeFormatter germanFormatter =  
    DateTimeFormatter  
        .ofLocalizedDate(FormatStyle.MEDIUM)  
        .withLocale(Locale.GERMAN);    
 LocalDate xmas = LocalDate.parse("24.12.2014", germanFormatter);  
System.out.println(xmas);   // 2014-12-24   
```

### LocalDateTime 本地日期时间

LocalDateTime 同时表示了时间和日期，相当于前两节内容合并到一个对象上了。LocalDateTime和LocalTime还有LocalDate一样，都是不可变的。LocalDateTime提供了一些能访问具体字段的方法。
```java
LocalDateTime sylvester = LocalDateTime.of(2014, Month.DECEMBER, 31, 23, 59, 59);    
DayOfWeek dayOfWeek = sylvester.getDayOfWeek();  
System.out.println(dayOfWeek);      // WEDNESDAY   
Month month = sylvester.getMonth();  
System.out.println(month);          // DECEMBER   
long minuteOfDay = sylvester.getLong(ChronoField.MINUTE_OF_DAY);  
System.out.println(minuteOfDay);    // 1439   
```

只要附加上时区信息，就可以将其转换为一个时间点Instant对象，Instant时间点对象可以很容易的转换为老式的java.util.Date。
```java
Instant instant = sylvester  
        .atZone(ZoneId.systemDefault())  
        .toInstant();    
 Date legacyDate = Date.from(instant);  
System.out.println(legacyDate);     // Wed Dec 31 23:59:59 CET 2014 
```

格式化LocalDateTime和格式化时间和日期一样的，除了使用预定义好的格式外，我们也可以自己定义格式：
```java
DateTimeFormatter formatter =  
    DateTimeFormatter  
        .ofPattern("MMM dd, yyyy - HH:mm");    
 LocalDateTime parsed = LocalDateTime.parse("Nov 03, 2014 - 07:13", formatter);  
String string = formatter.format(parsed);  
System.out.println(string);     // Nov 03, 2014 - 07:13   
```

## Annotation 多重注解

用例
```java
@interface Hints {  
    Hint[] value();  
}    
 @Repeatable(Hints.class)  
@interface Hint {  
    String value();  
}   
```

Java 8允许我们把同一个类型的注解使用多次，只需要给该注解标注一下@Repeatable即可（老方法）
```java
@Hints({@Hint("hint1"), @Hint("hint2")})  
class Person {}  
```
使用多重注解（新方法）
```java
@Hint("hint1")  
@Hint("hint2")  
class Person {}  
```

java编译器会隐性的帮你定义好@Hints注解，了解这一点有助于你用反射来获取这些信息
```java
Hint hint = Person.class.getAnnotation(Hint.class);  
System.out.println(hint);                   // null    
Hints hints1 = Person.class.getAnnotation(Hints.class);  
System.out.println(hints1.value().length);  // 2   
Hint[] hints2 = Person.class.getAnnotationsByType(Hint.class);  
System.out.println(hints2.length);          // 2
```

Java 8的注解还增加到两种新的target上
```java
@Target({ElementType.TYPE_PARAMETER, ElementType.TYPE_USE})  
@interface MyAnnotation {}  
```



