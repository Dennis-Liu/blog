### Java 8
#### Lambda 和 函数式接口
Lambda 表达式相信不用再过多的介绍，终于在 Java 8 引入了，可以极大的减少代码量，代码看起来更清爽。

函数式接口就是有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。可以隐式转化为 Lambda 表达式。

我们定义一个函数式接口如下：
```java
@FunctionalInterface
interface Operation {
    int operation(int a, int b);
}
```
再定义一个 Class 用来操作 Operation 接口。
```java
class Test {
    private int operate(int a, int b, Operation operation) {
        return operation.operation(a, b);
    }
}

Test test = new Test();
```
在 Java 8 之前，我们想要实现 Operation 接口并传给 Test.operate() 方法使用，我们要定义一个匿名类，实现 Operation 方法。
```java
test.operate(1, 2, new Operation() {
    @Override
    public int operation(int a, int b) {
        return a + b;
    }
});
```
而使用 Lambda 表达式，我们就可以这样写了：
```java
test.operate(1, 2, (a, b) -> a + b);
```
#### 方法引用
通过方法引用，可以使用方法的名字来指向一个方法。使用一对冒号来引 "::" 用方法。还是以上面的例子来看，我们再添加几个方法：
```java
@FunctionalInterface
interface Operation {
    int operation(int a, int b);
}

interface Creater<T> {
    T get();
}

interface TestInt {
    int cp(Test test1, Test test2);
}

class Test {
    public static Test create(Creater<Test> creater) {
        return creater.get();
    }

    private int operate(int a, int b, Operation operation) {
        return operation.operation(a, b);
    }

    private static int add(int a, int b) {
        return a + b;
    }

    private int sub(int a, int b) {
        return a - b;
    }

    public int testM(Test test) {
        return 0;
    }

    public void test(TestInt testInt) {
        Test t1 = Test.create(Test::new); 
        Test t2 = Test.create(Test::new);
        testInt.cp(t1, t2);
    }

}
```
那么对应的方法引用有四种：

构造方法引用

使用方式：Class::new
```java
Test test = Test.create(Test::new);
```
静态方法引用

使用方式：Class::staticMethod
```java
test.operate(1, 2, Test::add);
```
对象的实例方法引用

使用方式：instance::method
```java
test.operate(1, 2, test::sub);
```
类的实例方法引用

使用方式：Class::method
```java
test.test(Test::testM);
```
其实上面三种方法引用都好理解，最后类的实例方法引用，有两个条件：

首先要满足实例方法，而不是静态方法

Lambda 表达式的第一个参数会成为调用实例方法的对象

根据这两点我们看上面的例子，test 方法接受一个 TestInt 实例，用 Lambda 表达式表示就是 (Test t1, Test t2) -> res，而我们调用 test 方法时传入的方法引用是 Test::testM，其参数也是一个 Test 实例，最终 test.test(Test::testM) 的调用效果就是 t1.testM(t2)

#### 接口默认方法和静态方法
Java 8 新增了接口的默认实现，通过 default 关键字表示。同时也可以提供静态默认方法。
```java
public interface TestInterface {
    String test();

    // 接口默认方法
    default String defaultTest() {
        return "default";
    }

    static String staticTest() {
        return "static";
    }
}
```
#### 重复注解
Java 8 支持了重复注解。在 Java 8 之前想实现重复注解，需要用一些方法来绕过限制。比如下面的代码。
```java
@interface Author {
    String name();
}

@interface Authors {
    Author[] value();
}

@Authors({@Author(name="a"), @Author(name = "b")})
class Article {
}
```
而在 Java 8 中，可以直接用下面的方式。
```java
@Repeatable(Authors.class)
@interface Author {
    String name();
}

@interface Authors {
    Author[] value();
}

@Author(name = "a")
@Author(name = "b")
class Article {
}
```
在解析注解的时候，Java 8 也提供了新的 API。
```java
AnnotatedElement.getAnnotationsByType(Class<T>)
```
#### 类型注解
Java 8 之前注解只能用在声明中，在 Java 8 中，注解可以使用在 任何地方。
```java
@Author(name="a")
private Object name = "";
private String author = (@Author(name="a")String) name;
```
#### 更好的类型推断
Java 8 对于类型推断做了改进。

比如在 Java 7 中下面的写法：
```java
List<String> stringList = new ArrayList<>();
stringList.add("A");
stringList.addAll(Arrays.<String>asList());
```
在 Java 8 中改进后的写法，可以自动做类型推断。
```java
List<String> stringList = new ArrayList<>();
stringList.add("A");
stringList.addAll(Arrays.asList());
```
#### Optional
Java 8 中新增了 Optional 类用来解决空指针异常。Optional 是一个可以保存 null 的容器对象。通过 isPresent() 方法检测值是否存在，通过 get() 方法返回对象。
除此之外，Optional 还提供了很多其他有用的方法，具体可以查看文档。下面是一些示例代码。
```java
// 创建一个 String 类型的容器
Optional<String> str = Optional.of("str");
// 值是否存在
boolean pre = str.isPresent();
// 值如果存在就调用 println 方法，这里传入的是 println 的方法引用
str.ifPresent(System.out::println);
// 获取值
String res = str.get();
// 传入空值
str = Optional.ofNullable(null);
// 如果值存在，返回值，否则返回传入的参数
res = str.orElse("aa");
str = Optional.of("str");
// 如果有值，对其调用映射函数得到返回值，对返回值进行 Optional 包装并返回
res = str.map(s -> "aa" + s).get();
// 返回一个带有映射函数的 Optional 对象
res = str.flatMap(s -> Optional.of(s + "bb")).flatMap(s -> Optional.of(s + "cc")).get();
```
#### Stream
Java 8 中新增的 Stream 类提供了一种新的数据处理方式。这种方式将元素集合看做一种流，在管道中传输，经过一系列处理节点，最终输出结果。

关于 Stream 提供的具体方法，可以参照 API。下面是一些示例代码。
```java
List<String> list = Arrays.asList("maa", "a", "ab", "c");
list.stream()
        .filter(s -> s.contains("a"))
        .map(s -> s + "aa")
        .sorted()
        .forEach(System.out::println);

System.out.println("####");
list.parallelStream().forEach(System.out::println);

List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
int res = numbers.stream().map(i -> i + 1).mapToInt(i -> i).summaryStatistics().getMax();
System.out.println(res);
```
#### 日期时间 API
Java 8 中新增了日期时间 API 用来加强对日期时间的处理，其中包括了 LocalDate，LocalTime，LocalDateTime，ZonedDateTime 等等，关于 API 可以参照官方文档以及这篇博客，写的很详细。

https://www.cnblogs.com/muscleape/p/9956754.html

下面是示例代码。
```java
LocalDate now = LocalDate.now();
System.out.println(now);
System.out.println(now.getYear());
System.out.println(now.getMonth());
System.out.println(now.getDayOfMonth());

LocalTime localTime = LocalTime.now();
System.out.println(localTime);
LocalDateTime localDateTime = now.atTime(localTime);
System.out.println(localDateTime);
```
#### Base64 支持
Java 8 标准库中提供了对 Base 64 编码的支持。具体 API 见可参照文档。下面是示例代码。
```java
String base64 = Base64.getEncoder().encodeToString("aaa".getBytes());
System.out.println(base64);
byte[] bytes = Base64.getDecoder().decode(base64);
System.out.println(new String(bytes));
```
#### 并行数组 ParallelSort
Java 8 中提供了对数组的并行操作，包括 parallelSort 等等，具体可参照 API。
```java
Arrays.parallelSort(new int[] {1, 2, 3, 4, 5});
```
#### 其他新特性
对并发的增强

在java.util.concurrent.atomic包中还增加了下面这些类：
```java
DoubleAccumulator
DoubleAdder
LongAccumulator
LongAdder
```
提供了新的 Nashorn javascript 引擎

提供了 jjs，是一个给予 Nashorn 的命令行工具，可以用来执行 JavaScript 源码

提供了新的类依赖分析工具 jdeps

JVM 的新特性
JVM内存永久区已经被metaspace替换（JEP 122）。JVM参数 -XX:PermSize 和 –XX:MaxPermSize被XX:MetaSpaceSize 和 -XX:MaxMetaspaceSize代替。

可以看到，Java 8 整体上的改进是很大的，最重要的是引入 Lambda 表达式，简化代码。

其他一些改进可参照:

https://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html
