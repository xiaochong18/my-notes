

熟悉Java特性，如集合类、异常处理、Lambda编程、自定义注解等；

熟悉Java常用类库，如Hutool工具库、Jsoup爬虫、Lombok注解、Logback日志框架等;

熟练使用Spring Boot框架，能整合MyBatis-Plus和MyBatis X，实现高效的数据访问层开发;

熟悉MySQL库表设计，能编写高性能SQL，有索引设计、性能优化、ShardingSphere动态分表经验；

熟悉UDP、TCP、HTTP等常用协议；熟悉并实践过多种设计模式，了解AOP及IOC原理；





# 熟悉Java特性

## 封装继承多态

**封装**是把数据和方法包装在一起，通过访问控制隐藏内部细节，只暴露必要接口，保证数据安全；
**继承**是子类复用父类的属性和方法，并可扩展自己的功能，实现代码复用；
**多态**是同一接口在不同对象上有不同实现，运行时动态绑定，让代码更灵活、易扩展。

三者关系：封装保证安全，继承实现复用，多态提供灵活性。

## 基础数据类型

Java 有 8 种基本数据类型字节数及说明如下：

| **数据类型** | **字节数**             | **包装类** |
| ------------ | ---------------------- | ---------- |
| byte         | 1                      | Byte       |
| short        | 2                      | Short      |
| int          | 4                      | Integer    |
| long         | 8                      | Long       |
| float        | 4                      | Float      |
| double       | 8                      | Double     |
| char         | 2                      | Character  |
| boolean      | 未明确指定（JVM 规范） | Boolean    |

### String

1. 一旦创建好String字符串，**长度固定、内容永远不能改**。

2. **可变长字符串**要用到下面两类

   - **`StringBuffer`**（线程安全，多线程）
   - **`StringBuilder`**（推荐，单线程）

   | **维度** | **StringBuffer**                                           | **StringBuilder**          |
   | -------- | ---------------------------------------------------------- | -------------------------- |
   | 线程安全 | 安全（方法加 synchronized 修饰）                           | 不安全（无同步锁）         |
   | 性能     | 较低（同步开销）                                           | 较高（无锁竞争）           |
   | 使用场景 | 多线程环境下操作字符串缓冲区                               | 单线程环境下频繁拼接字符串 |
   | 继承结构 | 继承 AbstractStringBuilder，实现 Serializable/CharSequence | 同 StringBuffer            |

### 为什么日常用 String 而非 StringBuilder

**设计定位不同**：String 是**不可变**字符串，适用于**固定值场景**（如常量、少量拼接）；StringBuilder 是**可变**字符串缓冲区，适用于**频繁拼接** **/** **修改**场景。

**不可变的优势**：

o 线程安全，可共享；

o 保证字符串常量池复用（常量字符串）；

o 作为 Map 键时哈希值稳定。

 若简单定义字符串（如 String s = "abc"），用 String 更简洁且符合语义；若需动态拼接（如循环拼接），才优先用 StringBuilder 避免产生大量临时对象。

## 接口(Interface)和抽象类(Abstract Class)的主要区别?

接口多继承,方法默认抽象;

抽象类单继承,可有具体方法



## 集合类

Java 集合，也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 、 `Queue`。

Java 集合框架如下图所示：

![Java 集合框架概览](./java-collection-hierarchy.png)

### List, Set, Queue, Map 四者的区别？

- `List`列表: 存储的元素是**有序的、可重复**的。
- `Set`集合: 存储的元素是**无序的、不可重复**的。
- `Queue`队列: 按特定的**排队规则**来确定先后顺序，存储的元素是**有序的、可重复**的。是一种**先进先出**（FIFO，First In First Out）的数据结构，Queue只允许从一端插入元素，从另一端移除元素。
- `Map`: 使用**键值对（key-value）**存储，类似于数学上的函数 y=f(x)，"x" 代表 key，"y" 代表 value，**key 是无序的、不可重复的，value 是无序的、可重复的**，每个键最多映射到一个值。

### 常见集合☆

ArrayList： **动态数组**，实现了List接口，支持动态增长。**查询快**

LinkedList： **双向链表**，也实现了List接口，支持快速的插入和删除操作。**增删快**

HashMap： **数组+链表/红黑树**，存储键值对，通过键快速查找值。   达到0.75阈值就 2 倍扩容，初始大小16

HashSet： **基于HashMap实现的Set集合**，用于存储唯一元素。 

TreeMap： **基于红黑树实现的有序Map集合**，可以按照键的顺序进行排序。 

LinkedHashMap： **基于哈希表和双向链表实现的Map集合**，保持插入顺序或访问顺序。 

PriorityQueue： 优先队列，可以按照比较器或元素的自然顺序进行排序。

### HashMap、HashTable和ConcurrentHashMap的区别?

| 特性      | HashMap                | HashTable                                 | ConcurrentHashMap                                          |
| --------- | ---------------------- | ----------------------------------------- | ---------------------------------------------------------- |
| 线程安全  | **非线程安全**         | 线程安全（synchronized 修饰方法，全表锁） | 线程安全                                                   |
| 允许 null | 允许 key/value 为 null | 不允许 key/value 为 null                  | 不允许 null key/value                                      |
| 效率      | 高（无锁）             | 低（锁粒度粗，并发差）                    | 支持高并发读写，读操作无锁，写操作锁粒度小，适合多线程场景 |
| 继承      | 继承 AbstractMap       | 继承 Dictionary                           |                                                            |

1.第一代线程安全集合类
**Vector、Hashtable**
是怎么保证线程安排的：使用synchronized修饰方法
缺点：效率低下
2.第二代线程非安全集合类
ArrayList、HashMap
线程不安全，但是性能好，用来替代Vector、Hashtable
使用ArrayList、HashMap,需要线程安全怎么办呢？
Collections.synchronizedList(list);Collections.synchronizedMap(m);
底层使用synchronized代码块锁虽然也是锁住了所有的代码，但是锁在方法里边，并所在方法外边性能可以理解
为稍有提高吧。毕竟进方法本身就要分配资源的
3.第三代线程安全集合类
在**大量并发**情况下如何提高集合的效率和安全呢？
**java.util.concurrent.***
ConcurrentHashMap:
CopyOnWriteArrayList
CopyOnWriteArraySet:注意不是CopyOnWriteHashSet*
底层大都采用Lock锁(1.8的ConcurrentHashMap不使用Lock锁)，保证安全的同时，性能也很高。




## 异常处理

![1720683900898-1d0ce69d-4b5d-41a6-a5df-022e42f8f4c5](./1720683900898-1d0ce69d-4b5d-41a6-a5df-022e42f8f4c5-1773055976692-5.webp)

1.`try-catch`：**捕获并处理异常**

`finally`：**必须执行的收尾代码**

2.`throw`：**手动抛出一个异常**

```java
throw new 异常类名(异常信息);
```

3.`throws`：**声明方法可能抛出异常**

```java
返回值类型 方法名(参数) throws 异常1, 异常2 {
    // 方法体
}
```



## Lambda编程

Lambda 表达式是 **Java 8**引入的函数式编程语法，核心作用是**简化匿名内部类**（仅适用于**函数式接口**），让代码更简洁。

1. **必须基于函数式接口**：接口中**有且仅有一个抽象方法**（可加 `@FunctionalInterface` 注解校验）。
2. 本质：**将函数作为参数传递**，替代匿名内部类的冗余写法。

语法结构

```java
(参数列表) -> { 执行语句 }

// 匿名内部类  匿名内部类就是不用写实现类，直接临时创建一个接口 / 抽象类的对象，专门用来简化一次性使用的类
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("线程运行中~");
    }
}).start();
//创建线程时用
// Lambda 简化后（极简）
new Thread(() -> System.out.println("线程运行中~")).start();
```

### 6 大简化规则（必记）

规则 1：参数类型可**全部省略**（不能只省略部分）

```java
// 完整写法
(int a, int b) -> a + b
// 简化写法
(a, b) -> a + b
```

规则 2：单个参数可**省略小括号**

```java
// 完整写法
(String s) -> System.out.println(s)
// 简化写法
s -> System.out.println(s)
```

规则 3：方法体只有**一行语句**，可省略大括号

```java
// 完整写法
() -> { System.out.println("Hello"); }
// 简化写法
() -> System.out.println("Hello")
```

规则 4：方法体只有**一行 return 语句**，可省略大括号 + return

```java
// 完整写法
(a, b) -> { return a > b; }
// 简化写法
(a, b) -> a > b
```

规则 5：无参数时**必须保留小括号**

```java
() -> System.out.println("无参数")
```

规则 6：多个参数必须保留小括号

```java
(a, b, c) -> a + b + c
```

### 常见使用场景

1. #### 集合遍历

   ```java
   List<String> list = Arrays.asList("A", "B");
   list.forEach(s -> System.out.println(s));
   ```

2. #### 线程创建

   ```java
   new Thread(() -> System.out.println("线程运行")).start();
   ```

3. #### Stream 流操作

   ```java
   list.stream().filter(s -> s.equals("A")).forEach(System.out::println);
   ```

## 注解

注解就是代码中的特殊标记，这些标记可以在编译、类加载、运行时被读取，并执行相对应的处理

### 分类及应用

- Java原生

- **Spring框架的一套**

  `@SpringBootApplication` 是 Spring Boot 应用的核心注解，通常用于标注主启动类。`@SpringBootApplication`看作是下面三个注解的组合：

  - **`@EnableAutoConfiguration`**：启用 Spring Boot 的自动配置机制。
  - **`@ComponentScan`**：扫描 `@Component`、`@Service`、`@Repository`、`@Controller` 等注解的类。
  - **`@Configuration`**：允许注册额外的 Spring Bean 或导入其他配置类。

  `@Autowired` 用于自动注入依赖项（即其他 Spring Bean）。它可以标注在构造器、字段、Setter 方法或配置方法上，Spring 容器会自动查找匹配类型的 Bean 并将其注入。

  `@Component`：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。

  `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。

  `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。

  `@Controller` : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。

  `@RestController`：一个组合注解，等效于 `@Controller` + `@ResponseBody`。它专门用于构建 RESTful Web 服务的控制器。标注了 `@RestController` 的类，其所有处理器方法（handler methods）的返回值都会被自动序列化（通常为 JSON）并写入 HTTP 响应体，而不是被解析为视图名称。

- **lombok的注解**

- **元注解（Annotation）：修饰注解的**

​	**@Target**：限定注解能用在哪（类 / 方法 / 字段）

​	**@Retention**：指定注解活多久（源码 / 字节码 / 运行时）

​	**@Documented**：生成文档

​	**@Inherited**：允许注解被继承

​	**@Repeatable**：允许注解重复使用

- **自定义注解**

**应用**

- **数据校验**
- 规则定义
- **AOP（事务，回滚）**
- 标识，用于比类范围更大的通用设计
- 其他

### Lombok注解

Lombok 是一个 Java 库，通过注解在编译时生成常用样板代码，如构造方法、`getter`/`setter`、`toString`等，减少代码冗余，提升开发效率。本文详细介绍 Lombok 的主要注解并附代码示例。

1. **`@Data`（全能王，最常用）**

**作用**：自动生成以下所有方法，一个顶 N 个：

- `getter` / `setter`
- `toString()`
- `equals()` / `hashCode()`
- `无参构造器`
- `final` 字段的构造方法

**使用**：直接加在类上

```
import lombok.Data;

@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
}
```

✅ 不用手写任何 getter/setter，直接调用 `user.setName()`、`user.getName()`。

**2. `@Getter / @Setter`**

**作用**：只生成 getter /setter 方法

- 加在**类上**：为所有字段生成
- 加在**字段上**：只为当前字段生成

```
@Getter
@Setter
public class User {
    private String name;
}
```

3. **`@ToString`**

**作用**：自动生成 `toString()` 方法，打印对象时清晰不乱码

```
@ToString
public class User { ... }
```

4. `@NoArgsConstructor`

**作用**：生成**无参构造器**

5. `@AllArgsConstructor`

**作用**：生成**全参构造器**（所有字段作为参数）

6. `@Builder`（建造者模式，超好用）

**作用**：生成链式调用的建造者模式，优雅创建对象

```
import lombok.Builder;
import lombok.Data;

@Data
@Builder
public class User {
    private Long id;
    private String name;
}

// 使用方式（链式赋值，代码极简洁）
User user = User.builder()
        .id(1L)
        .name("张三")
        .build();
```

###  `@Slf4j`（日志注解）

**作用**：自动生成日志对象 `log`，不用手动 `LoggerFactory.getLogger()`

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class TestService {
    public void test() {
        log.info("日志输出"); // 直接用
    }
}
```

### 自定义注解的核心步骤

1. **定义注解接口**：使用 **`@interface` 关键字**声明注解。

2. **添加元注解**：指定注解的适用范围、生命周期等（必须）。

   ```java
   import java.lang.annotation.*;
   
   // 2. 两个元注解
   // 可以用在：类、方法、字段上
   @Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})
   // 注解保留到运行时（反射必备），指定注解存活时间
   @Retention(RetentionPolicy.RUNTIME)
   // 1.
   public @interface MyAnnotation {
       String value() default "默认值";
   }
   ```

3. **使用注解**：在类、方法等元素上标记自定义注解。直接把自定义注解*@MyAnnotation*加到**类、方法、字段**上：

4. **实现注解处理器**：通过反射获取注解信息并执行逻辑。

   ```java
   import java.lang.reflect.Method;
   
   public class AnnotationTest {
       public static void main(String[] args) throws Exception {
           // 1. 获取 Class 对象
           Class<TestClass> clazz = TestClass.class;
   
           // 2. 判断类上是否有该注解
           if (clazz.isAnnotationPresent(MyAnnotation.class)) {
               // 3. 获取注解实例
               MyAnnotation anno = clazz.getAnnotation(MyAnnotation.class);
               // 4. 获取注解属性值
               System.out.println("类注解值：" + anno.value()); 
               System.out.println("年龄：" + anno.age());
           }
   
           // 获取方法上的注解
           Method method = clazz.getMethod("test");
           if (method.isAnnotationPresent(MyAnnotation.class)) {
               MyAnnotation methodAnno = method.getAnnotation(MyAnnotation.class);
               System.out.println("方法注解值：" + methodAnno.value());
           }
       }
   }
   ```

## [JVM vs JDK vs JRE](https://javaguide.cn/java/basis/java-basic-questions-01.html#⭐️jvm-vs-jdk-vs-jre)

### JVM（Java 虚拟机）

**JVM 是一个运行在操作系统上的虚拟计算机，负责把 Java 字节码翻译成机器码并执行。**

它解决的问题很简单——跨平台。程序员写一次 `.java`，编译成 `.class`（字节码），JVM 在不同操作系统上负责适配底层差异。

#### **核心三件套：**

- **类加载器（ClassLoader）**：把 `.class` 文件加载进内存
- **运行时数据区**：JVM 管理的内存空间，是调优的主战场
- **执行引擎**：负责执行字节码，包含 JIT 编译器、垃圾回收器等

#### **JVM 启动参数及自定义方法**：

启动参数分三类，标准参数（- 开头，如 - version）、非标准参数（-X 开头，如 - Xint）、不稳定参数（-XX 开头，是调优核心，如 - XX:+UseG1GC）；自定义参数可通过 - Dproperty=value 设置系统属性，在程序中用 System.getProperty 获取。

### JDK

是一个功能齐全的 **Java 开发工具包**，供开发者使用，用于创建和编译 Java 程序。它**包含了 JRE**（Java Runtime Environment），以及编译器 javac 和其他工具，如 javadoc（文档生成器）、jdb（调试器）、jconsole（监控工具）、javap（反编译工具）等。

### JRE 

是**运行已编译 Java 程序所需的环境**，主要包含以下两个部分：

1. **JVM** : 也就是我们上面提到的 Java 虚拟机。
2. **Java 基础类库（Class Library）**：一组标准的类库，提供常用的功能和 API（如 I/O 操作、网络通信、数据结构等）。

## JVM和GC（垃圾回收）

### java内存区域

线程私有的：程序计数器 、虚拟机栈 、本地方法栈。

线程共享的：堆 、方法区。

![Java 运行时数据区域（JDK1.8 ）](https://oss.javaguide.cn/github/javaguide/java/jvm/java-runtime-data-areas-jdk1.8.png)

### GC

**堆和方法区使GC的主要作用域。**

Java 自动内存管理最核心的功能是 **堆** 内存中对象的分配与回收。

Java 堆是垃圾收集器管理的主要区域，因此也被称作 **GC 堆（Garbage Collected Heap）**。



存储对象实例，包括程序中创建的对象以及 Java 虚拟机自动创建的对象。

**堆空间可以分为新生代和老年代，还包括持久代（JDK 7 及之前版本）或元空间（JDK 8 及之后版本）。**

#### **总结：**

针对 HotSpot VM 的实现，它里面的 GC 其实准确分类只有两大种：

部分收集 (Partial GC)：

- 新生代收集（Minor GC / Young GC）：只对新生代进行垃圾收集；
- 老年代收集（Major GC / Old GC）：只对老年代进行垃圾收集。需要注意的是 Major GC 在有的语境中也用于指代整堆收集；
- 混合收集（Mixed GC）：对整个新生代和部分老年代进行垃圾收集。

整堆收集 (Full GC)：收集整个 Java 堆和方法区。

#### **对象回收判断方法**：

一是**引用计数法**，通过计数器统计对象引用数，计数为 0 则标记回收，缺点是无法解决循环引用；二是**可达性分析法**，以 GC Root 为起点遍历引用链，无引用链的对象可回收，为虚拟机主流使用方法。

#### **常见垃圾回收算法及优劣**：

**标记清除算法**实现简单，但效率低、产生内存碎片；**标记复制算法**运行高效、无碎片，却会使可用内存减半；标记整理算法无内存碎片，不过因涉及对象移动，效率低于复制算法。



# 熟悉Java常用类库

## 如Hutool工具库、

Hutool是一个小而全的Java工具类库，通过静态方法封装，降低相关API的学习成本，提高工作效率。Hutool是项目中“util”包友好的替代。

一个Java基础工具类，**对文件、流、加密解密、转码、正则、线程、XML等JDK方法进行封装**，组成各种Util工具类，同时提供以下组件：

| 模块               | 介绍                                                         |
| ------------------ | ------------------------------------------------------------ |
| hutool-aop         | JDK动态代理封装，提供非IOC下的切面支持                       |
| hutool-bloomFilter | 布隆过滤，提供一些Hash算法的布隆过滤                         |
| hutool-cache       | 简单缓存实现                                                 |
| hutool-core        | 核心，包括Bean操作、日期、各种Util等                         |
| hutool-cron        | 定时任务模块，提供类Crontab表达式的定时任务                  |
| hutool-crypto      | 加密解密模块，提供对称、非对称和摘要算法封装                 |
| hutool-db          | JDBC封装后的数据操作，基于ActiveRecord思想                   |
| hutool-dfa         | 基于DFA模型的多关键字查找                                    |
| hutool-extra       | 扩展模块，对第三方封装（模板引擎、邮件、Servlet、二维码、Emoji、FTP、分词等） |
| hutool-http        | 基于HttpUrlConnection的Http客户端封装                        |
| hutool-log         | 自动识别日志实现的日志门面                                   |
| hutool-script      | 脚本执行封装，例如Javascript                                 |
| hutool-setting     | 功能更强大的Setting配置文件和Properties封装                  |
| hutool-system      | 系统参数调用封装（JVM信息等）                                |
| hutool-json        | JSON实现                                                     |
| hutool-captcha     | 图片验证码实现                                               |
| hutool-poi         | 针对POI中Excel和Word的封装                                   |
| hutool-socket      | 基于Java的NIO和AIO的Socket封装                               |
| hutool-jwt         | JSON Web Token (JWT)封装实现                                 |
| hutool-ai          | AI大模型封装                                                 |

以下是实际用过的

| 工具类     | 最常用场景                         |
| ---------- | ---------------------------------- |
| StrUtil    | **接口参数判空校验、字符串脱敏**   |
| ObjUtil    | 对象非空判断、安全相等比较         |
| CollUtil   | 集合判空、集合分页、List 转 Map    |
| RandomUtil | **生成验证码、随机盐值、随机抽奖** |
| JSONUtil   | 接口 JSON 序列化 / 反序列化        |
| FileUtil   | 文件上传下载、本地文件读写         |
| Alias      | 实体类字段别名映射、JSON 序列化    |

## Jsoup爬虫（java工具库，[官网](https://jsoup.org/)）

jsoup 是一个用于处理 HTML 的 Java 库。它提供了一些非常方便的 API，用于提取和操作 HTML 页面数据，比如 [DOM](https://zhida.zhihu.com/search?content_id=226934499&content_type=Article&match_order=1&q=DOM&zhida_source=entity)，[CSS](https://zhida.zhihu.com/search?content_id=226934499&content_type=Article&match_order=1&q=CSS&zhida_source=entity) 等元素。

由于 jsoup 的 API 方法使用上与 [jQuery](https://zhida.zhihu.com/search?content_id=226934499&content_type=Article&match_order=1&q=jQuery&zhida_source=entity) 极其接近，因此如果你了解过 jQuery，那么可以轻而易举地上手这款框架。

```html
<!--添加依赖包-->
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.2</version>
</dependency>
```

jsoup 对 HTML 文档的解析，有三种实现方式。

- 第一种：直接通过 url 连接获取 HTML 文档，然后解析；
- 第二种：通过导入文件的方式，对 HTML 文档进行解析；
- 第三种：通过直接加载 HTML 字符串，对文档进行解析；

### 1、直接载入 url

```java
// 直接加载百度连接，获取百度首页页面信息
Document document = Jsoup.connect("https://www.baidu.com").get();
System.out.println(document.toString());

// 举例：抓取 id 为 xxx 的元素
Element element = document.getElementById("xxx");
if (element != null) { // 还可以用循环遍历输出
    System.out.println(element.text());
}
```

### 2、通过文件加载文档

创建一个`demo.html`文件，内容如下：

```html
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
 </head>
 <body>
  Hello World!
 </body>
</html>
```

然后通过文件加载文档，获取文档信息

```java
// 指定 HTML 文件地址，加载文档
String fileUrl = "/html/demo.html";
Document document = Jsoup.parse(new File(fileUrl), "utf-8" );
System.out.println(document.toString());
```

### 3、通过字符串加载文档

```java
// 定义一个 html 页面的字符串信息
String html = "<html><head><title></title></head><body>Hello World</body></html>";
Document document = Jsoup.parse(html);
System.out.println(document.toString());
```

通过以上任意的一种方式，都可以实现 HTML 页面文档的加载，获取`Document`对象。

### 具体应用

当获取到`Document`对象之后，我们就可以轻松抓取 HTML 页面中所有的元素信息了，包括 CSS 信息都可以解析出来

## Logback日志框架

由 Log4j 创始人开发的**新一代日志框架**

**SpringBoot 默认内置**，无需额外引入依赖

结构：**logback-core（核心）+ logback-classic（实现）+ logback-access（访问日志）**

- logback-core：其他俩模块基础模块，作为通用模块 其中没有 logger 的概念
- logback-classic：日志模块，完整实现了 SLF4J API
- logback-access：配合Servlet容器，提供 http 访问日志功能

### 日志级别（从高到低）

- **ERROR**：错误、异常，系统无法正常运行
- **WARN**：警告，不影响运行但需要注意
- **INFO**：关键流程信息（生产环境默认）
- **DEBUG**：调试信息（开发环境用）
- **TRACE**：最详细跟踪信息（极少用）



# 熟练使用Spring Boot框架

## Spring

Spring 三件套指的是 Spring 框架的三个核心模块，分别是 **Spring Core、Spring AOP 和 Spring MVC**。 **Spring Core：提供了 IoC（Inverse of Control）容器，**用于对象之间的解耦，通过容器自动将对象之间的依赖注入**。**同时，Spring Core 还提供了对 AspectJ 的集成，以及对基于注解的 Spring Bean 的支持。** **Spring AOP：提供了基于 AOP（Aspect Oriented Programming）的编程方式**，能够在不修改源代码的情况下，通过代理机制对对象进行增强，比如添加事务、日志、安全检查等功能。 **Spring MVC：是 Spring 框架的 Web 模块**，提供了 MVC（Model-View-Controller）模式的支持，用于处理 Web 请求和响应。Spring MVC 通过 DispatcherServlet、HandlerMapping、Controller 和 ViewResolver 等组件构成了完整的 MVC 模式的实现。

**MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。**

![1712650311366-b499469c-5afd-4be9-bad3-d787de86bf98](./1712650311366-b499469c-5afd-4be9-bad3-d787de86bf98-1773057929383-8.png)

**Spring 提供的核心功能主要是 IoC 和 AOP。**

Spring框架核心特性包括：

- IoC容器：Spring通过控制反转实现了对象的创建和对象间的依赖关系管理。开发者只需要定义好Bean及其依赖关系，Spring容器负责创建和组装这些对象。 
- AOP：面向切面编程，允许开发者定义横切关注点，例如事务管理、安全控制等，独立于业务逻辑的代码。通过AOP，可以将这些关注点模块化，提高代码的可维护性和可重用性。 
- 事务管理：Spring提供了一致的事务管理接口，支持声明式和编程式事务。开发者可以轻松地进行事务管理，而无需关心具体的事务API。 
- MVC框架：Spring MVC是一个基于Servlet API构建的Web框架，采用了模型-视图-控制器（MVC）架构。它支持灵活的URL到页面控制器的映射，以及多种视图技术。

| 核心思想 | 解决的问题                 | 实现手段                   | 典型应用场景                  |
| -------- | -------------------------- | -------------------------- | ----------------------------- |
| IOC      | 对象创建与依赖管理的高耦合 | 容器管理 Bean 生命周期     | 动态替换数据库实现、服务组装  |
| DI       | 依赖关系的硬编码问题       | Setter / 构造器 / 注解注入 | 注入数据源、服务层依赖 DAO 层 |
| AOP      | 横切逻辑分散在业务代码中   | 动态代理与切面配置         | 日志、事务、权限校验统一处理  |



**Spring Boot是一个构建在Spring框架顶部的项目。**它提供了一种简便，快捷的方式来设置，配置和运行基于Web的简单应用程序。

它是一个Spring模块，提供了 **RAD(快速应用程序开发)**功能。它用于创建独立的基于Spring的应用程序，因为它需要最少的Spring配置，因此可以运行。

## MyBatis

### 两级缓存

MyBatis 内置了两级缓存，都是基于 HashMap 实现的本地缓存，区别在于作用范围和生效条件。

1.一级缓存（SqlSession 级别）

**默认开启，无法关闭。**

作用域是单个 SqlSession。执行查询时，结果会存入当前 SqlSession 内部的缓存区域（ PerpetualCache，本质是个 HashMap）。同一个 SqlSession 再次查询同一条 SQL 时，直接从缓存中取，不会打数据库。

2.二级缓存（Mapper 级别 / SqlSessionFactory 级别）

**默认关闭，需要手动开启。**

作用域是同一个 MapperNamespace（即同一个 Mapper 接口/ XML 文件下的所有 SQL）。多个 SqlSession 可以共享同一个二级缓存，缓存在 `SqlSessionFactory` 层面管理。

## MyBatis-Plus和MyBatis X

Mybatis-plus简介：Mybatis增强工具，只做增强，不作改变，简化开发，提高效率。

### Mybatis-plus特点

1、无侵入：Mybatis-Plus 在 Mybatis 的基础上进行扩展，只做增强不做改变，引入 Mybatis-Plus 不会对您现有的 Mybatis 构架产生任何影响，而且 MP 支持所有 Mybatis 原生的特性

2、依赖少：仅仅依赖 Mybatis 以及 Mybatis-Spring

3、损耗小：启动即会自动注入基本CRUD，性能基本无损耗，直接面向对象操作

4、通用CRUD操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求

5、多种主键策略：支持多达4种主键策略（内含分布式唯一ID生成器），可自由配置，完美解决主键问题

6、支持ActiveRecord：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可实现基本 CRUD 操作

7、支持代码生成：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（P.S. 比 Mybatis 官方的 Generator 更加强大！）
支持自定义全局通用操作：支持全局通用方法注入( Write once, use anywhere )

8、内置分页插件：基于Mybatis物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于写基本List查询

9、内置性能分析插件：可输出Sql语句以及其执行时间，建议开发测试时启用该功能，能有效解决慢查询

10、内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，预防误操作

### MyBatis Plus操作数据库

MyBatis Plus 是对MyBatis的增强工具,提供了丰富的**CRUD接口**、分页插件、动态查询构造器等,能大幅减少编写SQL的工作量并提升开发效率。在我的云图库项目里,我首先在pom.xml中**引入MyBatis Plus的依赖**,配置了**@MapperScan 注解以扫描mapper包**,之后就可以在代码中直接使用内置的 BaseMapper 方法(如 **selectById、insert** 等)快速实现数据增删改查。借助MyBatis Plus的**QueryWrapper**或SqlRunner,还可以更灵活地构建动态查询。比如,我会在图片模块使用 **lambdaQuery().eq(Picture :: getSpaceId,spaceId)**的方式查询特定空间的图片列表。为了进一步提高构建MyBatis Plus查询的效率,我**封装了一些getQueryWrapper的方法**,可以根据查询请求对象转化为QueryWrapper。

### MybatisX

MybatisX是一款基于IDEA的快速开发插件，由MyBatis-Plus团队开发维护，为效率而生。

**它的主要功能如下：**

支持mapper.xml和Mapper接口之间方法的互相导航跳转； 内置代码生成器，通过使用GUI的形式，能根据[数据库](https://cloud.tencent.com/product/tencentdb-catalog?from_column=20065&from=20065)来生成Domain、mapper.xml、Mapper、Service和Service实现类代码； 可以自定义代码生成器模板； 可以通过类似JPA的方式，直接根据方法名称在mapper.xml中生成查询实现，同时支持提示。

# 数据库

##  规范化

第一范式（1NF）：确保每列都是不可分割的**原子项。**
第二范式（2NF）：在满足1NF的基础上，确保**非主键列完全依赖于主键列。**
第三范式（3NF）：在满足2NF的基础上，确保**非主键列之间不存在传递依赖关系。**
规范化有助于消除数据冗余，提高数据完整性和一致性。但过度的规范化可能会导致查询性能下降，因此在实际应用中需要权衡利弊。

## 字段设计

选择合适的数据类型：根据字段的取值范围和特点选择合适的数据类型，如INT、VARCHAR、DATE等。
设置合适的**字段长度**：避免使用过长的字段长度，以节省存储空间和提高查询性能。
**使用默认值**：为字段设置默认值可以简化数据插入操作，并减少数据冗余。
**避免使用NULL**：尽量避免在字段中使用NULL值，因为NULL值在查询和计算中可能会带来麻烦。可以使用NOT NULL约束和默认值来替代。

## 有索引设计、性能优化、

选择合适的索引类型：MySQL支持多种索引类型，如B-Tree索引、哈希索引等。根据查询需求和数据特点选择合适的索引类型。
**避免过度索引**：过多的索引会占用额外的存储空间并降低写操作的性能。因此，在设计索引时要权衡利弊，选择必要的索引。
**使用复合索引**：当查询条件涉及多个字段时，可以考虑使用复合索引来提高查询性能。但需要注意复合索引的列顺序和查询条件的匹配度。

## 主键设计

- **使用自增主键**：自增主键可以确保数据的**唯一性**，并简化插入操作。但需要注意自增主键的溢出问题。
- **避免使用业务字段作为主键**：业务字段的值可能会发生变化，如果将其作为主键可能会导致数据更新和删除操作的复杂性增加。

## ShardingSphere动态分表经验

### 分库分表

在水平方向（即数据方向）上，`分库`和`分表`的作用，其实是有区别的，不能混为一谈。

- `分库`：是为了解决数据库连接资源不足问题，和磁盘IO的性能瓶颈问题。
- `分表`：是为了解决单表数据量太大，sql语句查询数据时，即使走了索引也非常耗时问题。此外还可以解决消耗cpu资源问题。
- `分库分表`：可以解决 数据库连接资源不足、磁盘IO的性能瓶颈、检索数据耗时 和 消耗cpu资源等问题。

### ShardingSphere 

ShardingSphere 是 Apache **开源的分布式数据库中间件生态系统**，支持 JDBC 层嵌入和独立代理部署，拥有强大的**分库分表、读写分离、数据加密、弹性扩容、分布式事务**等能力。相比传统中间件，它更灵活、社区更活跃，适合大多数 Spring Boot 应用快速集成。

ShardingSphere 是一个开源的分布式数据库中间件生态，最早由当当网开源（Sharding-JDBC），目前由 Apache 基金会孵化。

### 包含 3 大组件：

| 组件                          | 作用说明                                      |              |
| ----------------------------- | --------------------------------------------- | ------------ |
| **ShardingSphere-JDBC  首选** | 轻量级 Java JDBC 层分库分表工具（无需 Proxy） | 只适用java   |
| ShardingSphere-Proxy          | 基于 MySQL/PostgreSQL 协议的中间件代理层      | 支持任意语言 |
| ShardingSphere-Sidecar        | 面向 Kubernetes 的数据库 Mesh Sidecar         |              |

### ShardingSphere 的核心能力

| 能力项              | 说明                                  |
| ------------------- | ------------------------------------- |
| ✅ 分库分表          | 支持标准、绑定表、广播表、Hint 路由等 |
| ✅ 读写分离          | 支持一主多从，负载均衡策略可配置      |
| ✅ 弹性扩容          | 数据迁移过程中支持双写、多活等策略    |
| ✅ 分布式事务        | 支持 Seata/XA/BASE 模式               |
| ✅ 数据加密          | 支持字段级加密与脱敏                  |
| ✅ 容器/K8s 支持     | Proxy 和 Sidecar 支持云原生部署       |
| ✅ SQL 显示/日志跟踪 | 方便调试分片路由结果                  |

![img](./8178299ef1db4582b7f6dd064cb61f1d.png)

（[Spring Boot + ShardingSphere 分库分表实战 - 技术栈](https://jishuzhan.net/article/1952701461506338817)）

### **分片策略**

- **分片键**：`order_id`（订单 ID）。图片id
- **分片算法**：哈希分片，将订单数据均匀分布到 4 个库中，每个库包含 8 张表。

### [配置 ShardingSphere](https://zhuanlan.zhihu.com/p/32714543753)

以下是基于 Sharding-JDBC 的配置示例：

#### **Maven 依赖**

```text
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
    <version>5.1.0</version>
</dependency>
```

#### **application.yml 配置**

```yml
spring:
  shardingsphere:
    datasource:
      names: ds0,ds1,ds2,ds3
      ds0:
        type: com.zaxxer.hikari.HikariDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        jdbc-url: jdbc:mysql://localhost:3306/ds0
        username: root
        password: root
      ds1:
        type: com.zaxxer.hikari.HikariDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        jdbc-url: jdbc:mysql://localhost:3306/ds1
        username: root
        password: root
      ds2:
        type: com.zaxxer.hikari.HikariDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        jdbc-url: jdbc:mysql://localhost:3306/ds2
        username: root
        password: root
      ds3:
        type: com.zaxxer.hikari.HikariDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        jdbc-url: jdbc:mysql://localhost:3306/ds3
        username: root
        password: root
    rules:
      sharding:
        tables:
          t_order:
            actual-data-nodes: ds$->{0..3}.t_order_$->{0..7}
            table-strategy:
              standard:
                sharding-column: order_id
                sharding-algorithm-name: t_order-inline
            key-generate-strategy:
              column: order_id
              key-generator-name: snowflake
        sharding-algorithms:
          t_order-inline:
            type: INLINE
            props:
              algorithm-expression: t_order_$->{order_id % 8}
        key-generators:
          snowflake:
            type: SNOWFLAKE
```

#### **关键点解析**

1. **数据节点**：`actual-data-nodes` 定义了数据分布规则，`ds$->{0..3}` 表示 4 个库，`t_order_$->{0..7}` 表示每个库有 8 张表。
2. **分片算法**：`INLINE` 算法通过 `order_id % 8` 计算分片位置。
3. **主键生成**：使用 [Snowflake 算法](https://zhida.zhihu.com/search?content_id=255565219&content_type=Article&match_order=1&q=Snowflake+算法&zhida_source=entity)生成全局唯一 ID。

## SQL注入问题

### 1.MyBatis 有两种传参方式：

① **${} 字符串拼接（危险 = 会注入）**

```sql
WHERE username = '张三' OR '1'='1'
```

② **#{} 预编译（安全 = 防注入）**

```sql
-- '张三' OR '1'='1' = ? 会预编译，只会传入普通字符串
WHERE username = ?
```

**解决：优先使用 `#{}` 占位符，尽量避免直接使用 `${}`**

### 2.MyBatis-Plus 做了什么？

**它所有查询方法 eq ()、like ()、in () 等，底层全部自动用 #{}**

## 经验

- 查询 SQL 尽量不要使用`select *`，而是`select`具体字段
- 如果知道查询结果只有一条或者只要最大/最小一条记录，建议用`limit 1`
- 应尽量避免在`where`子句中使用`or`来连接条件
- 注意优化`limit`深分页问题
- 使用`where`条件限定要查询的数据，避免返回多余的行
- 尽量避免在索引列上使用`mysql`的内置函数
- 应尽量避免在 `where`子句中对字段进行表达式操作
- 应尽量避免在`where`子句中使用`!=`或`<>`操作符
- 使用联合索引时，注意索引列的顺序，一般遵循最左匹配原则。
- 对查询进行优化，应考虑在`where 及 order by`涉及的列上建立索引
- 如果插入数据过多，考虑批量插入
- 在适当的时候，使用覆盖索引
- 使用 explain 分析你 SQL 的计划

## Redis-高并发场景

Redis可以看作java的服务，就是个Map<String，Object> 。有ip、host、用户、密码

### **Redis 为何运行速度快**：

核心有三点，一是**纯内存存储**，读写速度远高于硬盘；二是**单线程模型**，避免了多线程上下文切换和锁竞争的开销；三是采用 **IO 多路复用机制**，大幅提升网络处理效率。

### 5 种基本数据类型：

String（字符串）、List（列表）、Set（集合）、Hash（散列）、Zset（有序集合）。

1. **String**：存储文本、数字或二进制数据。用于缓存、计数器、分布式锁
2. **List**：双向链表，支持队列和栈操作。做简单消息队列、最新消息排行榜
3. **Hash**：字段-值映射，适合存储对象。存储用户信息、购物车等对象数据
4. **Set**：无序唯一集合，支持交并差运算。用于抽奖名单、点赞用户集合等场景
5. **Sorted Set**：按分数排序的有序集合。专门实现各类排行榜

### **RDB 和 AOF 持久化机制的区别**：

二者均为 Redis 的数备份手段，RDB 是定时对内存数据做全量快照，**恢复速度快**，但宕机可能丢失最后一次快照后的数；AOF 记录每一条写命令，**数据安全性更高**，却存在文件体积大、恢复慢的问题；生产环境通常混合使用，**RDB 做全量备份，AOF 做增量记录**。

### 多级缓存

多级缓存核心是**分层存储**，按 “**访问速度从快到慢、容量从大到小**” 分层，减少底层存储（如数据库）的访问压力，典型设计如下：

1. **L1** **本地缓存**：应用内存级缓存（如 Caffeine、Guava Cache），访问速度最快，容量最小，存储热点数据；
2. **L2** **分布式缓存**：如 Redis 集群，跨应用共享，容量大于本地缓存，存储高频访问数据；
3. **L3** **数据库**：MySQL/PostgreSQL 等，作为最终数据存储，容量最大，访问速度最慢。

**数据流转规则**：

·     读请求：优先查 L1 → 未命中查 L2 → 再未命中查数据库，查到后反向填充 L1/L2；

·     写请求：先写数据库，再淘汰 / 更新 L1/L2 中的对应数据（避免缓存与数据库不一致）。

**设计原则**：热点数据放 L1，次热点放 L2，冷数据落库；控制缓存粒度，避免缓存穿透 / 击穿 / 雪崩。

#### 本项目实现

**Redis**是一种高性能的分布式KV存储,支持丰富的数据结构和高并发访问。它能把缓存数据存储在内存中,单节点读写QPS可达10万,常用于做分布式缓
存或分布式锁。

**Caffeine**是Java主流的本地缓存库,运行在JVM内部,访问速度比Redis更快,但仅能在单个服务实例内使用,无法在多台服务器之间共享数据。

我在云图库项目中将二者结合,形成多级缓存:

用户请求先查询Caffeine本地缓存
若本地缓存未命中,则查询Redis缓存
若Redis也未命中,则回源到数据库,并将查询结果写回Redis与Caffeine 

### **缓存穿透、击穿、雪崩**：

**穿透是查询不存在的数据，请求绕开缓存直接打数据库**，解决用缓存空对象或布隆过滤器；

**击穿是热点 key 突然失效，大量请求瞬间冲击数据库**，解决可设置热点数据永不过期或加互斥锁；

**雪崩是大面积 key 同时失效或 Redis 宕机**，解决需给 key 过期时间加随机值，同时搭建 Redis 高可用集群。



缓存穿透定义指查询一个**不存在的数据**（如不存在的商品 ID），请求绕过缓存直接穿透到数据库，导致数据库压力激增。

核心解决方案

1. **布隆过滤器**：

o 原理：通过哈希算法将数据映射到二进制数组，判断数据是否存在；

o 作用：预加载数据库有效键，请求先经布隆过滤器过滤，不存在的键直接返回 null，避免穿透到数据库。

2. **缓存空值**：

o 对查询返回 null 的数据，缓存中存储空值并设置短过期时间（避免短时间内重复穿透）；

o 注意：需控制空值缓存的过期时间，避免占用过多缓存空间。

3. **接口校验**：在网关 / 服务层对非法参数（如负数 ID、空值）直接拦截，过滤无效请求。

### 布隆过滤器解决缓存穿透时存储的内容

布隆过滤器中**存储的是数据库中有效数据的键的哈希值**（而非原始键或数据值）。

·     初始化加载：系统启动时，将数据库中所有有效键（如商品 ID、用户 ID）通过多个哈希函数计算哈希值，映射到二进制位数组中，标记对应位置为 1；

·     查询判断：请求到来时，对请求的键同样计算哈希值，检查位数组对应位置是否全为 1：

o 若存在 0，说明键**不存在**，直接返回 null；

o 若全为 1，说明键**可能存在**（存在哈希冲突），需继续查询缓存 / 数据库。

### **如何保证 Redis 与数据库的数据一致性**：

成熟方案为延时双删或**先更新数据库再删除缓存**（推荐后者），选择删除而非更新缓存，是因为后续查询会从数据库读取新数据并回填缓存，保证数据最新；若有极高的一致性需求，可通过 Canal 监听 binlog 或加读写锁实现，该方式会牺牲部分性能。

缓存和数据库的一致性有 **4 种** 常见方案：

| 方案                               | 操作顺序                | 一致性                   | 推荐程度         |
| ---------------------------------- | ----------------------- | ------------------------ | ---------------- |
| 先更新数据库，再更新缓存           | DB → Cache              | 并发更新会覆盖，数据错乱 | 不推荐           |
| 先删缓存，再更新数据库             | Del → DB → Read → Cache | 并发读会写回旧值         | 需配合延迟双删   |
| **先更新数据库，再删缓存（推荐）** | DB → Del                | 极端情况不一致，概率极低 | Cache Aside 模式 |
| 订阅 Binlog 异步更新（最可靠）     | DB → Binlog → Cache     | 最终一致性               | 生产首选         |

**一句话结论**：推荐 **Cache Aside 模式**（先更新 DB，再删缓存），搭配 TTL 兜底。如果对一致性要求更高，用 **Canal 订阅 MySQL Binlog** 异步更新缓存。不要追求强一致性，接受 **最终一致性**。

### **Redis 分布式锁的实现及坑点解决**：

**简单实现可使用 setnx 指令**，生产环境一般用 Redis 框架（如 Redisson），解决三大核心坑点：给锁加过期时间，避免程序挂掉导致的死锁；通过 Lua 脚本保证**加锁和设置过期时间的原子性**；支持锁的可重入性，允许同一线程多次获取同一把锁。

## 并发

### **volatile 关键字**：

- **作用**：保证**可见性** + **有序性**
- **不能保证原子性**（如`i++`操作使用该关键字仍**非线程安全**，需加锁实现）
- **原理**：写完后立刻刷新到主存 + 禁止指令重排

### **synchronized 和 Lock 的区别**：

二者均为同步锁，核心差异有三点，一是出身不同，`synchronized`是 Java 关键字，基于 JVM 层面实现；Lock 是 Java 类，基于 API 层面实现。二是用法不同，`synchronized`自动释放锁，Lock 需手动解锁且要在`finally`中执行，否则易造成死锁。三是功能不同，Lock 功能更强大，支持`tryLock`尝试获取锁、实现公平锁，这些是`synchronized`不具备的。

### **CAS 的原理及问题**：

CAS 是乐观锁的实现方式，核心逻辑为**对比 - 更新 - 重试**，即先判断数据是否与预期一致，一致则更新，不一致则重试，无需主线程阻塞，执行效率高；存在三大问题，分别是重试失败会导致 CPU 开销飙升、仅能保证单个变量的原子性、存在 ABA 问题（数据从 A 变 B 再变回 A，CAS 会判定为无变化），ABA 问题可通过**加版本号**解决。

### **线程池七大参数及四种默认拒绝策略**：

用银行网点做通俗类比理解七大参数，`corePoolSize`核心线程数 = 常驻柜员，`workQueue`工作队列 = 候客区，`maximumPoolSize`最大线程数 = 常驻 + 临时柜员总数，`keepAliveTime`存活时间 = 临时柜员空闲辞退时长，`unit`时间单位 = 存活时间的单位，`threadFactory`线程工厂 = 给线程命名的规则，`handler`拒绝策略 = 候客区和临时窗口都满后的处理方式。

JDK 默认四种拒绝策略：默认策略直接报错；CallerRunsPolicy 让提交任务的线程自行执行任务；DiscardPolicy 直接丢弃新任务，无报错无处理；DiscardOldestPolicy 丢弃排队最久的任务，加入新任务。

### **AQS 的核心理解**：

AQS 的全称为 `AbstractQueuedSynchronizer` ，翻译过来的意思就是抽象队列同步器。这个类在 `java.util.concurrent.locks` 包下面。

AQS 是 JUC 的基础，众多锁均基于其实现，核心由两部分组成，**一是`state`变量（**表示锁的状态，如 0 为无锁、1 为有锁），二是 **CLH 双向链表队列**（线程抢不到锁时会被封装为节点，进入队列排队等待唤醒），本质就是信号量 + 排队系统。

### **并发编程AVO三大特性：**

**原子性、可见性、有序性**。

原子性（Atomicity）：操作不可分割，要么全成，要么全败。

可见性（Visibility）：一个线程改了共享变量，其他线程立刻能看到。

有序性（Ordering）：程序执行顺序，符合代码的逻辑顺序

## 线程池

### Java 线程状态表

| 线程状态      | 解释                                                       |
| ------------- | ---------------------------------------------------------- |
| NEW           | 尚未启动的线程状态，即线程创建，还未调用`start`方法        |
| RUNNABLE      | 就绪状态（调用`start`，等待调度）+ 正在运行                |
| BLOCKED       | 等待监视器锁时，陷入阻塞状态                               |
| WAITING       | 等待状态的线程正在等待另一线程执行特定的操作（如`notify`） |
| TIMED_WAITING | 具有指定等待时间的等待状态                                 |
| TERMINATED    | 线程完成执行，终止状态                                     |

### Java 创建线程的 3种核心方法

(继承Thread类,实现Runnable接口,实现Callable接口)

Java 中创建线程**本质只有 1 种**：**新建 `Thread` 对象**，区别在于**任务（run 方法）的实现方式不同**。

------

#### 继承 Thread 类（最基础）

**步骤**：继承 `Thread`类 → 重写 `run()` → 调用 `start()` 启动

```java
// 1. 继承 Thread 类
class MyThread extends Thread {
    // 2. 重写 run 方法（线程执行体）
    @Override
    public void run() {
        System.out.println("线程1：继承Thread方式");
    }
}

public class Test {
    public static void main(String[] args) {
        // 3. 创建线程对象
        MyThread thread = new MyThread();
        // 4. 启动线程（必须调用 start()，不能直接调用 run()）
        thread.start(); 
    }
}
```

**缺点**：Java 单继承，**无法再继承其他类**，**不推荐**。

------

#### 实现 Runnable 接口（最常用）

**步骤**：实现 `Runnable`接口 → 重写 `run()` → 传入 Thread → 启动

```java
// 1. 实现 Runnable 接口
class MyRunnable implements Runnable {
    // 2. 重写 run 方法
    @Override
    public void run() {
        System.out.println("线程2：实现Runnable方式");
    }
}

public class Test {
    public static void main(String[] args) {
        // 3. 创建任务对象
        MyRunnable runnable = new MyRunnable();
        // 4. 放入 Thread 构造方法
        Thread thread = new Thread(runnable);
        // 5. 启动线程
        thread.start();
    }
}
```

**优点**：无单继承限制，任务与线程解耦，**推荐**。

------

#### 实现 Callable 接口（带返回值 + 抛异常）

`Runnable` 无返回值、不能抛异常；**`Callable` 可以**，配合 `FutureTask` 使用：

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

// 1. 实现 Callable 接口，指定返回值类型
class MyCallable implements Callable<String> {
    // 2. 重写 call 方法（有返回值+可抛异常）
    @Override
    public String call() throws Exception {
        return "线程5：Callable带返回值";
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        // 3. 创建 Callable 对象
        MyCallable callable = new MyCallable();
        // 4. 包装为 FutureTask（既是 Runnable，又能存返回值）
        FutureTask<String> task = new FutureTask<>(callable);
        // 5. 放入 Thread 启动
        new Thread(task).start();
        
        // 6. 获取返回值（阻塞等待线程执行完）
        String result = task.get();
        System.out.println(result);
    }
}
```

**适用场景**：需要线程执行结果的场景。

- 缺点:因为线程类已经继承了Thread类，所以不能再继承其他的父类

### 使用线程池（Executor框架）

```java
class Task implements Runnable {
    @Override
    public void run() {
    	// 线程执行的代码

    }

    public static void main(String[] args) {
        ExecutorService executor=Executors.newFixedThreadPool(10);//创建固定大小的线程池
        for (int i = 0; i < 100; i++) {
        	executor.submit(new Task());//提交任务到线程池执行
        }
        executor. shutdown ( );//关闭线程池

    }
```

复杂

避免创建和销毁开销



### start () 和 run () 的区别

- **`start()`**：真正**启动线程**，JVM 会在新线程中执行 `run()`，**异步执行**；
- **`run()`**：只是普通方法调用，**在当前线程（main）执行**，不会开启新线程。

![img](https://cdn.xiaolincoding.com//picgo/1712648206670-824228d1-be28-449d-8509-fd4df4ff63d3.webp)

### 

------

### `sleep()` 与 `wait()` 区别对比表

| 特性     | `sleep()`                  | `wait()`                             |
| -------- | -------------------------- | ------------------------------------ |
| 所属类   | `Thread` 类（静态方法）    | `Object` 类（实例方法）              |
| 锁释放   | ❌ 不释放锁                 | ✅ 释放锁                             |
| 使用前提 | 任意位置调用               | 必须在同步块内（持有锁）             |
| 唤醒机制 | 超时自动恢复               | 需 `notify()` / `notifyAll()` 或超时 |
| 设计用途 | 暂停线程执行，不涉及锁协作 | 线程间协调，释放锁让其他线程工作     |

### 线程停止

| 方法              | 适用场景                                                     | 注意事项                                                     |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 循环检测标志位    | 简单无阻塞的逻辑（如循环计算、遍历）                         | 标志位必须用 `volatile` 修饰，或通过锁保证内存可见性，避免主线程修改后工作线程感知不到 |
| 中断机制          | 可中断的阻塞操作（如`sleep()`、`wait()`、`join()`、`Lock.lockInterruptibly()`） | 必须正确处理 `InterruptedException`，**不能吞异常**；处理后需恢复中断状态（`Thread.currentThread().interrupt()`），避免中断标志被清除导致后续逻辑失效 |
| `Future.cancel()` | 线程池管理任务（`ExecutorService.submit()` 提交的异步任务）  | `cancel(true)` 会发送中断信号，仅对支持中断的任务有效；若任务不响应中断，仅标记任务为取消状态，线程仍会继续执行 |
| 资源关闭          | 不可中断的阻塞操作（如`Socket` I/O、`InputStream.read()`、文件锁） | 显式关闭底层资源（如`Socket.close()`），触发`IOException`，在异常处理中结合中断状态完成线程退出与数据回滚 |

### **ThreadLocal **：

- **作用**：线程本地存储，每个线程有独立副本，互不干扰
- **内存泄露原因**：`ThreadLocalMap` 中 key 是弱引用（可被回收），value 是强引用。如果线程复用（如线程池），key 变 null 后，value 仍然可达但无法访问，造成泄露
- **解决**：用完手动调用 `remove()`

### volatile vs synchronized

| 对比项               | volatile                                 | synchronized                                       |
| -------------------- | ---------------------------------------- | -------------------------------------------------- |
| **修饰对象**         | 仅能修饰**成员变量 / 静态变量**          | 可修饰**方法**（普通 / 静态）、**代码块**          |
| **并发三大特性保证** | 保证**可见性、有序性**，**不保证原子性** | 保证**原子性、可见性、有序性**（全量保证）         |
| **锁特性**           | 无锁，基于**内存屏障**实现，非阻塞       | 独占锁（可升级：偏向锁→轻量级锁→重量级锁），阻塞式 |
| **实现层面**         | JVM 层面的**内存语义**优化               | JVM 层面的 **Monitor 监视器锁** 实现               |
| **使用开销**         | 极小，无线程上下文切换开销               | 相对较高，阻塞时存在上下文切换、锁竞争开销         |
| **同步范围**         | 仅针对**单个变量**的读写                 | 针对**一段代码 / 方法**的临界区操作                |
| **线程阻塞**         | 不会导致线程阻塞 / 唤醒                  | 线程抢锁失败会进入阻塞状态                         |

synchronized 是锁，解决的是**原子性**问题，通过排队保证同一时刻只有一个线程操作；
volatile 是修饰符，解决的是**可见性**问题，保证一个线程的修改立刻对其他线程可见，但不保证原子性；
ThreadLocal 是线程本地存储，从根本上**避免共享**，每个线程持有独立副本，不存在竞争问题。
三者不在一个维度，synchronized 和 volatile 是处理共享变量的两种不同策略，ThreadLocal 则是直接消灭共享。

## 事务

事务是一组操作的集合，要么全部成功执行，要么全部失败回滚，保证数据的一致性与可靠性。

### 事务ACID四大特性：

| 特性         | 全称        | 保障机制              | 核心作用                                                     |
| ------------ | ----------- | --------------------- | ------------------------------------------------------------ |
| **A 原子性** | Atomicity   | **Undo Log 回滚日志** | 事务要么全成功，要么全失败；失败则通过 Undo 回滚所有操作     |
| **C 一致性** | Consistency | **AID + 数据库约束**  | 事务执行前后数据合法完整，是**最终目标**，由另外三大特性共同保证 |
| **I 隔离性** | Isolation   | **锁机制 + MVCC**     | 并发事务之间互不干扰，解决脏读 / 不可重复读 / 幻读           |
| **D 持久性** | Durability  | **Redo Log 重做日志** | 事务提交后数据永久保存，宕机也不丢失                         |

### 隔离级别

Spring 事务隔离级别（基于 SQL 标准 + Spring 封装），共 **5 种**，从低到高：

| 级别                         | 脏读 | 不可重复读 | 幻读                             |
| :--------------------------- | :--- | :--------- | :------------------------------- |
| READ_UNCOMMITTED读未提交     | ❌    | ❌          | ❌                                |
| READ_COMMITTED读已提交       | ✅    | ❌          | ❌                                |
| REPEATABLE_READ可重复读      | ✅    | ✅          | ❌（MySQL InnoDB 通过间隙锁解决） |
| SERIALIZABLE串行化：最高级别 | ✅    | ✅          | ✅                                |

快速记忆

- 脏读：读到别人未提交的数据
- 不可重复读：同一事务内两次查询结果不一样
- 幻读：查询时发现多出 / 少了行

# 其他

## UDP、TCP、HTTP等常用协议

### UDP、TCP

| 特性     | TCP                        | UDP                                 |
| -------- | -------------------------- | ----------------------------------- |
| 连接性   | 面向连接                   | 无连接                              |
| 可靠性   | 可靠                       | 不可靠 (尽力而为)                   |
| 状态维护 | 有状态                     | 无状态                              |
| 传输效率 | 较低                       | 较高                                |
| 传输形式 | 面向字节流                 | 面向数据报 (报文)                   |
| 头部开销 | 20 - 60 字节               | 8 字节                              |
| 通信模式 | 点对点 (单播)              | 单播、多播、广播                    |
| 常见应用 | HTTP/HTTPS, FTP, SMTP, SSH | DNS, DHCP, SNMP, TFTP, VoIP, 视频流 |

建立一个 TCP 连接需要“三次握手”，缺一不可：

- **一次握手**:客户端发送带有 SYN（SEQ=x） 标志的数据包 -> 服务端，然后客户端进入 **SYN_SEND** 状态，等待服务端的确认；

- **二次握手**:服务端发送带有 SYN+ACK(SEQ=y,ACK=x+1) 标志的数据包 –> 客户端,然后服务端进入 **SYN_RECV** 状态；

- **三次握手**:客户端发送带有 ACK(ACK=y+1) 标志的数据包 –> 服务端，然后客户端和服务端都进入**ESTABLISHED** （已建立的）状态，完成 TCP 三次握手。

  **确认双方的「发送能力」和「接收能力」都正常**。

  **1.防止历史连接被错误建立，2.在建立连接阶段同步双方初始序列号。**

![tcp-shakes-hands-three-times](./tcp-shakes-hands-three-times.png)

断开一个 TCP 连接则需要“四次挥手”，缺一不可：

1. **第一次挥手**：客户端发送一个 FIN（SEQ=x） 标志的数据包->服务端，用来关闭客户端到服务端的数据传送。然后客户端进入 **FIN-WAIT-1** 状态。

2. **第二次挥手**：服务端收到这个 FIN（SEQ=X） 标志的数据包，它发送一个 ACK （ACK=x+1）标志的数据包->客户端 。然后服务端进入 **CLOSE-WAIT** 状态，客户端进入 **FIN-WAIT-2** 状态。

3. **第三次挥手**：服务端发送一个 FIN (SEQ=y)标志的数据包->客户端，请求关闭连接，然后服务端进入 **LAST-ACK** 状态。

4. **第四次挥手**：客户端发送 ACK (ACK=y+1)标志的数据包->服务端，然后客户端进入**TIME-WAIT**状态，服务端在收到 ACK (ACK=y+1)标志的数据包后进入 CLOSE 状态。此时如果客户端等待 **2MSL** 后依然没有收到回复，就证明服务端已正常关闭，随后客户端也可以关闭连接了。

![tcp-waves-four-times](./tcp-waves-four-times.png)

**HTTP/3.0 之前是基于 TCP 协议的**，而 HTTP/3.0 将弃用 TCP，改用 基于 UDP 的 QUIC 协议 。

### websocket

WebSocket 是一种基于 TCP 连接的全双工通信协议，即客户端和服务器可以同时发送和接收数据。

WebSocket 协议本质上是应用层的协议，用于弥补 HTTP 协议在持久通信能力上的不足。客户端和服务器**仅需一次握手**，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

#### 区别

**快毫秒级（没有多次握手和挥手）、数据开销小（有状态的，握手成功后，请求头只需要两个字节）**

WebSocket 是一种**双向实时通信协议**，而 HTTP 是一种单向通信协议。并且，HTTP 协议下的通信只能由客户端发起，服务器无法主动通知客户端。

WebSocket 使用 ws:// 或 wss://（使用 SSL/TLS 加密后的协议，类似于 HTTP 和 HTTPS 的关系） 作为协议前缀，HTTP 使用 http:// 或 https:// 作为协议前缀。

WebSocket 可以支持扩展，用户可以扩展协议，实现部分自定义的子协议，如支持压缩、加密等。

WebSocket 通信数据格式比较轻量，用于协议控制的**数据包头部相对较小**，网络开销小，而 HTTP 通信每次都要携带完整的头部，网络开销较大（HTTP/2.0 使用二进制帧进行数据传输，还支持头部压缩，减少了网络开销）。

**HTTP**：**一问一答、短连接**。客户端主动问，服务器才回答，问完就断开。

**WebSocket**：**全双工、长连接**。客户端和服务器**一直连着**，两边都能主动发消息，像打电话。

#### 工作过程

**客户端向服务器发送一个 HTTP 请求**，请求头中包含 `Upgrade: websocket` 和 `Sec-WebSocket-Key` 等字段，表示要求**升级协议为 WebSocket**；

服务器收到这个请求后，会进行升级协议的操作，如果支持 WebSocket，它将**回复一个 HTTP 101 状态码**，响应头中包含 ，`Connection: Upgrade`和 `Sec-WebSocket-Accept: xxx` 等字段、表示成功升级到 WebSocket 协议。

客户端和服务器之间**建立了一个 WebSocket 连接，可以进行双向的数据传输**。数据以帧（frames）的形式进行传送，WebSocket 的每条消息可能会被切分成多个数据帧（最小单位）。发送端会将消息切割成多个帧发送给接收端，接收端接收消息帧，并将关联的帧重新组装成完整的消息。

客户端或服务器可以主动发送一个关闭帧，表示要断开连接。另一方收到后，也会回复一个关闭帧，然后双方关闭 TCP 连接。

### 协同编辑

#### Websocket 拦截器-权限校验

在 WebSocket 连接前需要进行权限校验，如果发现用户没有团队空间内编辑图片的权限，则拒绝握手，可以通过定义一个 WebSocket 拦截器实现这个能力。
此外，由于 HTTP 和 WebSocket 的区别，我们不能在后续收到前端消息时直接从request 对象中获取到登录用户信息，因此也需要通过 WebSocket 拦截器，**为即将建立连接的 WebSocket 会话指定一些属性**，比如登录用户信息、编辑的图片id等。
编写拦截器的代码，需要实现 **HandshakeInterceptor** 接口:

#### Websocket 处理器

我们需要定义 WebSocket 处理器类，**在连接成功、连接关闭、接收到客户端消息时进行相应的处理**。
可以实现 **TextWebSocketHandler** 接口，这样就能以字符串的方式发送和接受消息了:

首先在处理器类中定义2个Map常量，key是图片id，value分别为:
保存当前正在编辑的用户 id，执行编辑操作、进入或退出编辑时都会校验。
保存参与编辑图片的用户 WebSocket 会话的**集合**。

注意，由于可能同时有多个 WebSocket 客户端建立连接和发送消息，集合要使用并发包(JUC)中的 **concurrentHashmap** ，来保证线程安全。
2)由于接下来很多消息都需要传递给所有协作者，所以先编写一个 广播消息 的方法。该方法会根据 pictureld，将响应消息发送给编辑该图片的所有会话。考虑到可能会有消息不需要发送给编辑者本人的情况，该方法还可以接受 excludeSession 参数，支持排除掉向某个会话发送消息。

**三个步骤**

1. 建立连接（广播）

2. 接收客户端消息，执行不同处理：**进入编辑、执行编辑、推出编辑**（包含广播给其他空间成员）

3. 关闭连接，移除当前用户编辑状态，移除会话（广播）


最后配置websocket启动的接口

```java
@Configuration
@EnableWebSocket //启动
public class WebSocketConfig implements WebSocketConfigurer {

    @Resource
    private PictureEditHandler pictureEditHandler;

    @Resource
    private WsHandshakeInterceptor wsHandshakeInterceptor;

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        // websocket
        registry.addHandler(pictureEditHandler, "/ws/picture/edit")
                .addInterceptors(wsHandshakeInterceptor)
                .setAllowedOrigins("*");
    }
}

```

以上只能顺序执行，不能并发执行

tomcat默认200个连接，请求线程

异步，请求放到任务队列后可以继续取，异步执行任务队列中的任务  Disruptor无锁队列

### HTTP 状态码

分五大类，核心按请求结果划分： 2xx 成功、3xx 重定向、4xx 客户端错、5xx 服务端错，1xx 很少用。 高频的比如 200（请求成功带数据）、204（成功无数据）； 301 永久重定向（域名迁移）、302 临时重定向（未登录跳登录）、304 用缓存； 400 参数错、401 未登录、403 无权限、404 资源不存在； 500 服务端 bug、502 网关收无效响应（如后端崩了）、504 网关超时（后端处理慢）。 开发中查问题先看状态码：4xx 查客户端，5xx 查服务端

## 熟悉并实践过多种设计模式

### Spring 框架中用到了哪些设计模式

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` **创建 bean 对象。**
- **代理设计模式** : **Spring AOP 功能的实现。**
- **单例设计模式** : **Spring 中的 Bean 默认都是单例的**。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller

### 单例模式⭐️

单例模式需要保证一个类只有一个实例，并提供一个全局访问的方法调用这个实例。

构造方法只能是private，并且拥有一个当前类的静态成员变量/静态实例。

单例模式提供了两种方式，懒汉模式和饿汉模式

- 懒汉模式：**首次使用单例实例的时候创建**，之后使用时再**判断单例实例是否已创建**，如果没有则创建实例。在多线程使用中，需要考虑线程安全问题。双重检查锁是一细分
- 饿汉模式：在该**类初始化的时候就创建实例对象**，线程是安全的。

**注意：**

1. 单例模式中，不能使用反射模式创建单例，反射会创建一个新的实例对象。
2. 懒汉模式，需要考虑线程安全问题。
3. 单例模式中，构造方法时私有的，不能被继承。

### 工厂模式⭐️

工厂模式中由工厂提供创建对象的接口来代替new创建对象实例，实现调用者与创建者的分离，降低程序耦合。工厂模式又分为简单工厂模式、工厂方法模式和抽象工厂模式。

#### 简单工厂模式

简单工厂模式相当于现实中的一个工厂，我们可以通过工厂生产产品，这里是通过工厂创建对象实例。用户只需要知道对象的名称，工厂就可以返回对应名称的对象实例。

#### 工厂方法模式

在简单工厂模式中，存在这样一个缺点，**如果需要增加工厂的产品，那么就需要修改工厂类方法**，一个工厂生产多种产品。**工厂方法模式就是一个工厂（工厂子类）只生产一种产品**，后期扩展产品需要创建新工厂。

扩展优于修改。

最常见的集合类就是工厂模式。

#### 抽象工厂模式

就工厂模式而言，一个工厂只能生产一种产品，如果要求一个工厂同时兼生产多种产品，这种需求就不能满足。

在抽象工厂模式中，抽象工厂类中定义了多种产品，即定义其子类应该生产哪几种产品。但后期抽象工厂再增加产品时，需要对所有具体工厂进行修改。

#### 区别

- 简单工厂：所有的产品都由一个工厂生产。如果后期需要增加产品，就需要修改工厂代码。
- 工厂方法：可以有多个工厂，但一个工厂只生产一种产品。如果后期需要增加产品，那就需要增加工厂类。
- 抽象工厂：可以有多个工厂，每个工厂可以生产多类产品。如果后期需要增加产品，那么就需要在抽象工厂里加抽象方法，然后修改所有的子类。**相当于比工厂模式多了一个产品体系**

## AOP及IOC原理

AOP面向切面编程是将非业务代码，如日志记录、事务管理、权限控制等从核心业务逻辑中分离出来单独封装，减少系统的重复代码，降低模块间的耦合度。Spring AOP 就是基于<font color='red'>动态代理</font>实现，可通过五种通知方式来触发。

IoC控制反转是一种把**对象的创建、管理、生命周期** 从程序员手里，**反转交给 Spring 容器**的技术思想，**<font color='red'>依赖注入(DI)</font>**是实现这种技术的一种方式。传统开发过程中，我们需要通过new关键字来创建对象。使用IoC思想开发方式的话，我们不通过new关键字创建对象，而是通过IoC容器来帮我们实例化对象。 通过IoC的方式，可以大大降低对象之间的耦合度。

## AOP

AOP（Aspect Oriented Programming）即面向切面编程，AOP 是 OOP（面向对象编程）的一种延续，二者互补，并不对立。

AOP 的目的是将横切关注点（如日志记录、事务管理、权限控制、接口限流、接口幂等等）从核心业务逻辑中分离出来，通过动态代理、字节码操作等技术，实现代码的复用和解耦，提高代码的可维护性和可扩展性。OOP 的目的是将业务逻辑按照对象的属性和行为进行封装，通过类、对象、继承、多态等概念，实现代码的模块化和层次化（也能实现代码的复用），提高代码的可读性和可维护性。

AOP 之所以叫面向切面编程，是因为它的**<font color='red'>核心思想就是将横切关注点从核心业务逻辑中分离出来</font>**，形成一个个的**切面（Aspect）**。

实现代码的**复用和解耦**，提高代码的可维护性和可扩展性

### 实现方式

<font  color=red>AOP 的常见实现方式有动态代理。</font>动态代理是在运行时动态生成代理对象，而不是在编译时。它允许开发者在运行时指定要代理的接口和行为，从而实现在不修改源码的情况下增强方法的功能。

JDK 动态代理

不用任何框架，手写实现 AOP，帮你看透本质！

#### 1. 业务接口（目标对象）

```java
// 用户业务接口
public interface UserService {
    void addUser();
}
```

#### 2. 业务实现类

```java
// 业务实现（核心代码，不改它！）
public class UserServiceImpl implements UserService {
    @Override
    public void addUser() {
        System.out.println("执行业务：添加用户");
    }
}
```

#### 3. AOP 切面功能（日志 + 耗时统计）

```java
// 切面：统一增强功能
public class AopLog {
    // 前置通知：方法执行前
    public void before() {
        System.out.println("【AOP前置】方法开始执行...");
    }

    // 后置通知：方法执行后
    public void after() {
        System.out.println("【AOP后置】方法执行结束...");
    }
}
```

#### 4. 动态代理工厂（AOP 核心）

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

// AOP 动态代理工厂
public class AopProxyFactory {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                new InvocationHandler() {
                    AopLog aopLog = new AopLog();

                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        // AOP 前置增强
                        aopLog.before();
                        
                        // 执行目标方法（业务代码）
                        Object result = method.invoke(target, args);
                        
                        // AOP 后置增强
                        aopLog.after();
                        
                        return result;
                    }
                }
        );
    }
}
```

#### 5. 测试运行

```java
public class TestAop {
    public static void main(String[] args) {
        // 目标对象
        UserService userService = new UserServiceImpl();
        
        // 获取代理对象（AOP增强）
        UserService proxy = (UserService) AopProxyFactory.getProxy(userService);
        
        // 执行方法 → 自动带上AOP功能！
        proxy.addUser();
    }
}
```

#### 运行结果

```java
【AOP前置】方法开始执行...
执行业务：添加用户
【AOP后置】方法执行结束...
```

### Spring AOP 注解版

#### 1. 依赖（SpringBoot 环境）

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

#### 2. 编写 AOP 切面类

```java
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

// 切面
@Aspect
@Component
public class LogAop {

    // 前置通知：执行addUser前打印日志
    @Before("execution(* com.example.service.UserService.addUser(..))")
    public void beforeLog() {
        System.out.println("【AOP】方法开始执行");
    }

    // 后置通知：执行addUser后打印日志
    @After("execution(* com.example.service.UserService.addUser(..))")
    public void afterLog() {
        System.out.println("【AOP】方法执行完毕");
    }
}
```

#### 3. 业务类

```java
@Service
public class UserService {
    public void addUser() {
        System.out.println("添加用户成功");
    }
}
```

#### 4. 运行结果

```
【AOP】方法开始执行
添加用户成功
【AOP】方法执行完毕
```

### 通知类型

![img](./aspectj-advice-types.jpg)

- **Before**（前置通知）：目标对象的方法调用之前触发
- **After** （后置通知）：目标对象的方法调用之后触发

- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发

- **AfterThrowing**（异常通知）：目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。

- **Around** （环绕通知）：编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方

### 解决了什么问题

OOP 面向对象编程不能很好地处理一些分散在多个类或对象中的公共行为（如日志记录、事务管理、权限控制、接口限流、接口幂等等），这些行为通常被称为 **横切关注点（cross-cutting concerns）** 。如果我们在每个类或对象中都重复实现这些行为，那么会导致代码的冗余、复杂和难以维护。

AOP 可以将横切关注点（如日志记录、事务管理、权限控制、接口限流、接口幂等等）从 **核心业务逻辑（core concerns，核心关注点）** 中分离出来，实现关注点的分离。

### 应用场景：

- <font color='red'>日志记录</font>：自定义日志记录注解，利用 AOP，一行代码即可实现日志记录。
- 性能统计：利用 AOP 在目标方法的执行前后统计方法的执行时间，方便优化和分析。
- <font color='red'>事务管理</font>：**`@Transactional`** 注解可以让 Spring 为我们进行事务管理比如回滚异常操作，免去了重复的事务管理逻辑。`@Transactional`注解就是基于 AOP 实现的。
- <font color='red'>权限控制</font>：利用 AOP 在目标方法执行前判断用户是否具备所需要的权限，如果具备，就执行目标方法，否则就不执行。例如，SpringSecurity 利用`@PreAuthorize` 注解一行代码即可自定义权限校验。
- 接口限流：利用 AOP 在目标方法执行前通过具体的限流算法和实现对请求进行限流处理。
- 缓存管理：利用 AOP 在目标方法执行前后进行缓存的读取和更新

## IOC

IoC（Inverse of Control:控制反转）是一种设计思想或者说是某种模式。这个设计思想就是 **将原本在程序中手动创建对象的控制权交给第三方比如 IoC 容器。** 对于我们常用的 Spring 框架来说， **IoC 容器实际上就是个 Map（key，value）,Map 中存放的是各种对象。**不过，IoC 在其他语言中也有应用，并非 Spring 特有。

**传统的开发方式** ：往往是在类 A 中手动通过 new 关键字来 new 一个 B 的对象出来

**使用 IoC 思想的开发方式** ：**不通过 new 关键字来创建对象**，而是通过 IoC 容器(Spring 框架) 来帮助我们实例化对象。我们需要哪个对象，直接从 IoC 容器里面去取即可。

**为什么叫控制反转?**

- **控制** ：指的是对象创建（实例化、管理）的权力
- **反转** ：控制权交给外部环境（IoC 容器）

### 实现方式

<font color='red'>依赖注入</font>：IOC的核心概念是依赖注入，即容器负责管理应用程序组件之间的依赖关系。Spring通过**构造函数注入、属性注入或方法注入**，将组件之间的依赖关系描述在配置文件中或使用注解。 

设计模式 - **工厂模式**：Spring IOC容器通常采用工厂模式来管理对象的创建和生命周期。容器作为工厂负责实例化Bean并管理它们的生命周期，将Bean的实例化过程交给容器来管理。 

容器实现：Spring IOC容器是实现IOC的核心，通常使用BeanFactory或ApplicationContext来管理Bean。BeanFactory是IOC容器的基本形式，提供基本的IOC功能；ApplicationContext是BeanFactory的扩展，并提供更多企业级功能。

 **接口 + 实现类（解耦）**

```java
// UserDao 接口
public interface UserDao {
    void save();
}

// UserDao 实现
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("保存用户成功！");
    }
}
```

```java
// UserService
public class UserService {
    // 不用 new！IOC 会注入进来
    private UserDao userDao;

    // 提供 set 方法，让 IOC 容器注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void saveUser() {
        userDao.save();
    }
}
```

例如：现有类 A 依赖于类 B

- **传统的开发方式** ：往往是在类 A 中手动通过 new 关键字来 new 一个 B 的对象出来
- **使用 IoC 思想的开发方式** ：不通过 new 关键字来创建对象，而是通过 IoC 容器(Spring 框架) 来帮助我们实例化对象。我们需要哪个对象，直接从 IoC 容器里面去取即可。

从以上两种开发方式的对比来看：我们 “丧失了一个权力” (创建、管理对象的权力)，从而也得到了一个好处（不用再考虑对象的创建、管理等一系列的事情）

### 解决了什么问题

IoC 的思想就是两方之间不互相依赖，由第三方容器来管理相关资源。这样有什么好处呢？

1. 对象之间的<font color='red'>耦合度</font>或者说依赖程度降低；
2. <font color='red'>资源变的容易管理</font>；比如你用 Spring 容器提供的话很容易就可以实现一个单例。

方便单元测试、对象创建、对象依赖、单例

![img](./IoC&Aop-ioc-illustration-dao.png)

### @Resource、@Autowired注解

| 特性         | `@Resource`                          | `@Autowired`                                         |
| ------------ | ------------------------------------ | ---------------------------------------------------- |
| **来源**     | Java 官方规范（JSR-250），跨框架通用 | Spring 框架**专属**注解                              |
| **注入规则** | 默认按名称注入，再按**类型**匹配     | 默认按类型注入，必须配合 `@Qualifier` 才能按名称匹配 |

```java
// 按名称注入：匹配名为 userService 的 Bean
@Resource
private UserService userService;
// 强制指定名称
@Resource(name = "userServiceImpl")
private UserService userService;

// 按类型注入
@Autowired
private UserService userService;
// 按名称注入（必须加@Qualifier）
@Autowired
@Qualifier("userServiceImpl")
private UserService userService;
```

## 反射

**Spring IOC容器利用Java的反射机制动态地加载类、创建对象实例及调用对象方法，反射允许在运行时检查类、方法、属性等信息，从而实现灵活的对象实例化和管理。**

```java
//第一步：获取 Class 对象（反射的入口，3 种方式）
// 1. Class.forName("全类名") → 最常用（配置文件、框架底层）
Class<?> clazz1 = Class.forName("com.example.User");

// 2. 对象.getClass() → 已有对象实例时使用
User user = new User();
Class<?> clazz2 = user.getClass();

// 3. 类名.class → 编译期确定，性能最高
Class<?> clazz3 = User.class;

//第二步：反射核心操作（通用实现流程）
//通过 Class 对象，动态操作构造方法、成员变量、成员方法
```



## java bean

java Bean 的作用域决定了 Bean 实例的生命周期和可见范围，主要在 Spring 框架中使用。常见的作用域有：

**常用作用域（4 种）**

| 作用域        | 说明                                           | 默认        |
| :------------ | :--------------------------------------------- | :---------- |
| **singleton** | 整个 IoC 容器中只有一个实例，所有请求共享      | ✅           |
| **prototype** | 每次请求都创建一个新实例                       |             |
| **request**   | 每个 HTTP 请求创建一个实例，请求结束即销毁     | 仅 Web 应用 |
| **session**   | 每个 HTTP Session 创建一个实例，会话过期即销毁 | 仅 Web 应用 |

**少用的作用域（2 种）**

| 作用域          | 说明                                                   |
| :-------------- | :----------------------------------------------------- |
| **application** | 整个 ServletContext 生命周期内只有一个实例，相当于全局 |
| **websocket**   | 每个 WebSocket 连接创建一个实例                        |

## 微服务

### **一、为什么会有 Spring Cloud？**

在讲 Spring Cloud 之前，我们先看一个问题：

一个传统单体项目长什么样？

用户模块 + 订单模块 + 商品模块 + 支付模块 = 一个大项目

优点：

- 简单、开发快

缺点：

- 修改一个模块要重新部署整个项目
- 一旦某个模块崩了，整个系统一起挂
- 难以扩展（并发一高就容易出现性能瓶颈）

### **微服务架构的出现**

为了解决这些问题，微服务架构应运而生：

把一个大系统拆成多个“小服务”

用户服务 订单服务 商品服务 支付服务

每个服务：

- 独立开发
- 独立部署
- 独立扩展

听起来很美好，但问题也随之而来。

### 解决

当服务一多，就会遇到：

- 服务之间怎么通信？
- 服务挂了怎么办？
- 怎么做负载均衡？
- 怎么统一网关入口？
- 怎么监控这些服务？

于是，Spring Cloud 就诞生了。

| 问题           | 解决组件                                                     |
| -------------- | ------------------------------------------------------------ |
| 服务注册与发现 | Eureka / [Nacos](https://zhida.zhihu.com/search?content_id=272304180&content_type=Article&match_order=1&q=Nacos&zhida_source=entity) |
| 服务调用       | Feign                                                        |
| 网关           | Gateway                                                      |
| 负载均衡       | LoadBalancer                                                 |
| 熔断降级       | [Circuit Breaker](https://zhida.zhihu.com/search?content_id=272304180&content_type=Article&match_order=1&q=Circuit+Breaker&zhida_source=entity) |
| 配置管理       | [Config](https://zhida.zhihu.com/search?content_id=272304180&content_type=Article&match_order=1&q=Config&zhida_source=entity) |
| 分布式链路追踪 | [Sleuth](https://zhida.zhihu.com/search?content_id=272304180&content_type=Article&match_order=1&q=Sleuth&zhida_source=entity) |

# 面试

## 接口和抽象类区别

**抽象类**：对**类本身**的抽象，体现 **is-a（是一种）** 关系，包含**属性、普通方法、构造方法**，是**不完整的类**。

**接口**：对**行为 / 能力**的抽象，体现 **can-do（能做什么）** 关系，仅定义规范，**无状态、无构造方法**。

## 抽象类可以继承抽象类吗？

✅ **可以**。Java 中抽象类可以继承另一个抽象类，无需实现父类的抽象方法。



## 深分页查询越往后翻页越慢？如何解决？

MySQL 执行 `LIMIT offset, size` 深分页时：

1. 必须**全量扫描并加载 `offset + size` 条数据**到内存；
2. 再**丢弃前面 `offset` 条无用数据**，只返回最后 `size` 条；
3. **offset 越大（越往后翻页），扫描的数据越多，IO/CPU 开销越大，速度越慢**。

解决方案（按推荐优先级排序）

**方案 1：主键 ID 连续分页（最优、最常用）**

**原理**：放弃 `offset`，利用**自增主键 / 唯一有序 ID**直接定位起始位置，无冗余扫描。

**适用**：只支持 **上一页 / 下一页**（电商 / 列表主流方案），不支持跳页。

```sql
# 深分页慢查询
SELECT * FROM user LIMIT 10000,10;

# 优化后（极速）
SELECT * FROM user WHERE id > 10000 LIMIT 10;
```

**方案 2：延迟关联（子查询优化）**

**原理**：先通过索引快速查询**主键 ID**，再通过主键回表查询完整数据，大幅减少扫描量。

**适用**：需要少量跳页、无法使用 ID 连续分页的场景。

```sql
# 优化后
SELECT u.* FROM user u
JOIN (SELECT id FROM user ORDER BY id LIMIT 10000,10) AS temp
ON u.id = temp.id;
```

**方案 3：覆盖索引优化**

**原理**：让查询字段**全部包含在索引中**（覆盖索引），避免回表，减少 IO 开销。

**适用**：查询字段固定、可建联合索引的场景。

```sql
# 建立覆盖索引
CREATE INDEX idx_user ON user(id,name,age);

# 直接走索引，无需回表
SELECT id,name,age FROM user LIMIT 10000,10;
```

## MySQL 中`timestamp`与`datetime`的区别

用`timestamp`：记录需**随时区变化**的时间（如日志、操作时间），但需注意 2038 年上限。

用`datetime`：记录固定日期时间（如生日、预约时间），**范围更广且无溢出风险**。

## 

## InnoDB 的日志类型及各自作用

| 日志类型 | 所属层级        | 日志性质 | 核心目标                 |
| -------- | --------------- | -------- | ------------------------ |
| Redo Log | InnoDB 存储引擎 | 物理日志 | 保证事务持久性，崩溃恢复 |
| Undo Log | InnoDB 存储引擎 | 逻辑日志 | 事务回滚，实现 MVCC      |
| Binlog   | MySQL Server 层 | 逻辑日志 | 主从复制，数据恢复，审计 |

## 百万级消息中间件Rocketmq（阿里）

服务端客户端启动类一大堆线程池初始化方法，单独建立线程池

核心入口`@SpringBootApplication`注解 → 自动配置核心流程：开启 `@EnableAutoConfiguration` → 导入 `AutoConfigurationImportSelector` → 读取 SPI 配置类列表 → 条件过滤 → 注册符合条件的 Bean 到容器。

## 跳跃表（SkipList） 

跳跃表是一种**多层有序链表**结构，是 **Redis 有序集合（Sorted Set/zset）** 的**底层核心实现**。它通过**空间换时间**的方式，实现了**接近平衡树**的查询效率，且代码实现比红黑树简单得多。

核心原理：**空间换时间**：通过建立多层索引，避免链表从头遍历的低效问题，将链表 O (N) 的查询效率优化为 **O (logN)时间复杂度**。**空间复杂度O (N)**

## Redis 用跳表实现 zset，**不用红黑树**的原因：

1. **实现更简单**：无旋转、变色等复杂平衡逻辑
2. **范围查询更快**：平衡树不擅长区间查询，跳表天然支持
3. **插入 / 删除更快**：无需调整树的平衡，开销更小

## SQL优化

### 一、总原则（开篇必说）

1. **减少数据扫描量**：**避免全表扫描，优先走索引**
2. **减少数据返回量**：只查需要的字段，不冗余查询
3. **减少计算 / 排序**：避免文件排序、临时表
4. **提前过滤数据**：**`WHERE` 条件优先缩小数据集**

### 二、核心：索引优化（面试占比 70%）

#### 1. 建索引原则

- **高频查询字段**建索引（`WHERE`/`JOIN`/`ORDER BY`/`GROUP BY` 字段）
- **联合索引**遵循**最左前缀原则**
- 区分度低的字段（性别、状态）**不建索引**
- 禁止建**冗余 / 重复索引**（如单独建`a`索引 + 联合`(a,b)`索引）

#### 2. 索引失效场景（必背！）

- 索引列上做**运算、函数、类型转换**
- 模糊查询**左模糊 / 全模糊**（`%abc`）
- 使用 `!=` / `IS NOT NULL` / `NOT IN`（部分场景失效）
- 联合索引**违反最左前缀**
- 隐式类型转换（字符串不加引号）

#### 3. 索引高级优化

- **覆盖索引**：查询列全部在索引中，**避免回表**
- **索引下推**：减少回表次数，MySQL 5.6 + 默认开启
- 禁止在**频繁更新**的字段建索引（维护索引开销大）

### 三、SQL 语句优化（高频写法）

1. **禁止 `SELECT \*`**，只查询业务需要的字段
2. **深分页优化**：用`主键ID定位`代替`LIMIT大偏移量`
3. **子查询优化**：优先用`JOIN`代替`IN/EXISTS`
4. 索引列**不做任何计算**（`age+1=18` → `age=17`）
5. **提前过滤**：`WHERE` 优先过滤，减少关联数据量
6. **排序优化**：`ORDER BY` 字段走索引，避免`Using filesort`
7. **去重优化**：能用`UNION ALL`不用`UNION`（避免去重开销）
8. **批量操作**：批量`INSERT/UPDATE`代替循环单条执行

### 四、表结构设计优化

1. **数据类型最小化**：能用`tinyint`不用`int`，能用`varchar(16)`不用`varchar(255)`
2. 字段**尽量 NOT NULL**，用默认值代替`NULL`
3. 大字段（`text/blob`）**拆分出主表**
4. 合理**冗余字段**（空间换时间，减少关联）
5. 海量数据：**分库分表**（垂直拆分 + 水平拆分）

### 五、执行计划 EXPLAIN 核心判断（面试加分）

通过`EXPLAIN`分析 SQL，重点看 4 个字段：

1. type：访问类型（优先级）

   system > const > eq_ref > ref > range > index > ALL

   ✅ 要求：至少达到range级别，禁止ALL（全表扫描）

2. **key**：实际命中的索引（`NULL`= 未命中索引）

3. **rows**：扫描行数（**越小越好**）

4. Extra：禁止出现两个高危值

   - `Using filesort`：文件排序（需优化）
   - `Using temporary`：临时表（需优化）

### 六、关联查询优化

1. **小表驱动大表**：小数据集关联大数据集，提升效率
2. **关联字段必须建索引**
3. 禁止**多表过度关联**（建议不超过 3 张表）

### 七、面试高频总结（一句话速记）

1. 索引是 SQL 优化的核心，**最左前缀、避免失效**是关键
2. 禁止全表扫描、禁止文件排序、禁止临时表
3. 少查字段、早过滤、少计算、少关联
4. 用覆盖索引减少回表，用主键优化深分页

## MySQL 锁分类 

### 一、按**锁粒度**划分（最常考）

1. **全局锁**
   - 锁整个数据库实例
   - 使用：`FLUSH TABLES WITH READ LOCK`
   - 场景：全库逻辑备份
   - 缺点：整个库只读，业务阻塞
2. **表级锁**
   - 锁整张表
   - 分：表共享读锁、表排他写锁
   - MyISAM 只支持表锁；InnoDB 也支持表锁
   - 优点：开销小、加锁快
   - 缺点：并发度差
3. **行级锁**
   - 锁单行数据
   - InnoDB 特有
   - 优点：并发度极高
   - 缺点：开销大、加锁慢，会出现死锁

### 二、按**锁兼容性**划分

1. 共享锁（S 锁 / 读锁）
   - 加锁：`SELECT ... LOCK IN SHARE MODE`
   - 特性：多个事务可同时加读锁，互斥写锁
2. 排他锁（X 锁 / 写锁）
   - 加锁：`INSERT/UPDATE/DELETE`、`SELECT ... FOR UPDATE`
   - 特性：独占，与任何锁都互斥

### 三、InnoDB 行锁细分（面试核心）

1. 记录锁（Record Lock）
   - 锁**单行记录**
   - 锁在索引记录上
2. 间隙锁（Gap Lock）
   - 锁索引之间的**间隙**
   - 目的：防止幻读
3. 临键锁（Next-Key Lock）
   - 记录锁 + 间隙锁
   - InnoDB 默认行锁算法
   - 可解决**幻读问题**（RR 隔离级别）

### 四、意向锁（表锁，协调用）

- 意向共享锁（IS）、意向排他锁（IX）
- 作用：快速判断表内是否有行被锁，避免表锁无脑全表加锁
- 意向锁之间**互不冲突**

### 五、面试一句话总结

- 大并发用 **InnoDB 行锁**
- 读多写少、无高并发可用 **表锁**
- **Next-Key Lock** 解决幻读
- 行锁基于**索引**，索引失效会退化为表锁





# 知识点

## Java 线程顺序执行方案（线程）

直接按顺序调用线程 start () 无法保证执行顺序，因为 **start () 仅使线程进入就绪状态**，实际执行由操作系统调度决定，结果是乱序的。需手动干预执行流程。

## 方案一：Thread.join ()  

- 原理：让线程强等另一个线程结束，如 T2 等待 T1 结束，T3 等待 T2 结束。

- 底层机制：join（）基于 wait 实现，目标线程执行完后 JVM 自动调用 notifyAll (超时时间)，无需手动干预。

- 避坑技巧：生产环境建议使用带超时时间的 join (超时时间)，避免死等。

![image-20260329180536506](./image-20260329180536506.png)

| 方法      | 属于哪个类 | 作用           | 是否释放锁 | 使用场景             |
| --------- | ---------- | -------------- | ---------- | -------------------- |
| `start()` | Thread     | 启动线程       | 不涉及锁   | 开启新线程           |
| `wait()`  | Object     | 让当前线程等待 | **释放锁** | 线程间协作、等待唤醒 |



# 云图库

## 模块关系

整个云图库项目的后端拆分为以下几个核心模块:

**·用户模块:提供用户注册、登录、鉴权等功能**,利用Redis实现分布式Session登录。
· **图片模块:负责图片增删改查、上传**、多维搜索、**批量编辑**、批量抓图等操作,并结合COS对象存储完成图片管理,结合数据万象完成图片解析、压缩和缩略图生成,
· **空间模块:支持创建私有空间和团队空间,并提供空间成员管理、空间额度控制**、空间图片共享、**多人协同编辑**等功能。基于Sa-Token实现RBAC权限模型,可灵活控制空间成员权限。
· Al 模块:通过自主封装的AI接口,对接AI绘画大模型实现AI扩图功能,支持异步任务轮询。
· **协作模块:基于WebSocket实现多人实时编辑图片,利用“编辑锁”及事件驱动设计降低并发冲突风险**,并采用Disruptor技术提升吞吐量。
· 分析模块:对空间使用情况、图片分类、空间排行等进行统计,基于MyBatis Plus和分组查询(group by)实现聚合分析。
·**分表模块:******对旗舰团队空间数据进行单独的分表管理****,提升查询效率,**基于ShardingSphere动态分表实现。**
· **缓存模块:引入Redis+Caffeine构建多级缓存体系,**显著降低主页图片数据的查询耗时,同时**通过随机过期时间避免缓存雪崩。**

各模块之间的关系如下:

· 用户登录后,通过权限模块判断其是否能访问对应的空间或图片数据
· 图片上传与管理需要调用图片模块与COS对象存储服务
· 空间模块与用户模块和图片模块关联,团队空间通过space_user关联表管理成员
· AI 模块提供异步扩图能力,供图片模块或前端直接调用
· 协作模块基于WebSocket 来通知前端实时操作,实现多人在线编辑
· 统计分析模块会调用图片、空间等模块的数据接口来生成统计报表
·缓存模块贯穿在各个模块的查询操作中,提高访问效率



## 库表设计

在云图库项目中,我根据业务逻辑与访问场景分析,共设计了几张关键表:**用户表(user)、图片表(picture)、空间表(space)、空间成员表**
(space_user)等。我在设计库表时会综合查询性能、存储空间、可读性、开发成本进行考虑,下面简单说明:

1)用户表(user)

· **userAccount 建立唯一索引:**快速定位用户并避免重复注册
· **userPassword 采用MD5+盐存储:保障密码安全**
· userRole 角色字段:支持 user/admin两种枚举值
· userName 建立**普通索引**:便于模糊检索昵称
· isDelete:支持逻辑删除

2)图片表(picture)
· url/thumbnailUrl:分别保存原图和缩略**图地址**,加载大图前先显示缩略图提升体验
· **name/introduction/category**:添加**索引**,提高检索性能
· tags: JSON字符串格式存储,便于修改维护
·reviewStatus:审核状态,用枚举值减少存储空间并提高查询性能
· picSize/picWidth/picHeight/picFormat:存储图片详细信息,有助于后期统计分析
· userld/spaceld:支持按用户或空间查询
· updateTime/editTime:分开编辑时间和更新时间,有利于区分用户动作与系统自动更新

3)空间表(space)

**· spaceName/spaceLevel**:建立**索引**提升常用查询性能
· maxSize/maxCount:空间允许存储的最大容量与图片数,结合totalSize/totalCount方便监控剩余空间
· spaceType:区分个人空间/企业空间
· userld:标记该空间的创建人

4)空间成员表(space_user)
**· spaceld+userld:采用唯一索引** uk_spaceId_userId,保证同一用户在同一空间下只有一条成员记录
· spaceRole: 空间成员角色枚举 viewer/editor/admin
· **idx_spaceld/idx_userld:添加索引**,提升按空间或按成员查询的性能

## 异常处理

为了统一管理后端异常,我在项目里封装了一个自定义异常类(BusinessException)、全局异常处理器(GlobalExceptionHandler)、并且自定义了一些错误码(ErrorCode),便于前端统一处理错误。

流程如下:

当遇到业务逻辑问题时,我会抛出BusinessException,并传入相应的自定义错误码和错误信息。
GlobalExceptionHandler 会捕获这个BusinessException,并返回一个统一的响应结构给前端。
如果是未预料到的系统异常(RuntimeException),同样会被GlobalExceptionHandler拦截,返回固定的系统错误码(50000)和提示信息(系统内部异常)。

这样做可以让前端快速定位问题原因,也便于后续在不同场景下进行精细化的异常处理。

## 用户注册

在云图库项目的用户模块中,我对用户注册做了以下几步处理:

**数据校验:**对用户输入的账号、密码进行非空判断、长度判断等,如果不符合要求,则抛出自定义异常。
**账号查重:**利用MyBatis Plus 构造查询条件,在user表中检测是否已有相同的userAccount,如果存在则视为重复注册。
**密码加密**:对密码进行**MD5+盐值**(比如“yupi")的加密后再存入数据库,防止明文密码泄露。
**数据插入**:以上都通过后才执行插入操作,将用户信息保存到user表里。若数据库插入失败,则抛出异常提示。

前端发起的**注册请求**,会映射到**UserController中的/register接口**,控制层做完参数判空后就**交给 UserService的userRegister 方法来处理**。注册成功后,会**返回新用户的id**给前端确认。

## 用户登录

在云图库项目中,我采用了基于Session的登录态管理实现用户登录,实现步骤如下:

**校验参数**:检查账号、密码是否为空,长度是否合理。
**密码对比**:将前端传来的密码同样进行加密,并在数据库中查询匹配的记录。如果查无此用户或密码不匹配,就抛出异常。
**Session 存储**:若验证通过,就在后端的Session中保存用户信息,比如 request.getSession().setAttribute(USER_LOGIN_STATE, user)。
**返回脱敏数据**:由于用户表里还包括密码字段,为了安全起见,会对用户的密码进行脱敏处理后再返回登录成功信息给前端。

这样,当后续请求携带同一个Session ID时,后端即可根据Session判断当前用户的登录状态,从而进行权限判定。若Session中无登录态,则视为未登录。

## 统一权限管理

云图库项目有**普通用户、管理员2种角色**,为了简化权限控制,我做了以下工作:

角色区分:在user表中**通过 userRole字段来区分user和admin**。
**注解+AOP**:定义一个 **@AuthCheck 注解**,用于标注某个接口需要的角色;然后利用**Spring AOP写了一个拦截器(环绕通知),**在接口调用前自动校验当前登录用户是否拥有足够的权限。

使用注解:在需要管理员权限的接口上加一句

这样一来,权限管理逻辑不会分散在各种接口里,而是集中在一个切面类中维护,可读性高、扩展性也更好。若后期要增加新的权限等级或更细粒度的控制,也能在切面逻辑里统一加以实现。

**@AuthCheck(mustRole="admin")**,就能统一检查用户是否是管理员。若校验失败,就抛出无权限异常。



## 数据存储

在云图库项目中,我选择使用对象存储来存储图片文件。一方面,借助云厂商(如腾讯云COS)提供的对象存储可以快速解决海量图片的存储、传输、扩容和备份问题;另一方面,腾讯云对象存储还提供“数据万象”图像处理服务,可以在图片上传时自动解析并返回图片的尺寸、大小、格式等元数据信息。

具体实现流程:

1)项目中整合对象存储:

· 在后端配置文件中写好存储桶(bucket)名称、访问域名、密钥等信息,编写一个配置类来读取参数并初始化客户端。
·初始化客户端后,我编写了一个通用的对象存储调用类(CosManager),通过 cosClient.putObject 方法实现上传文件到存储桶。
· 基于对象存储调用类进一步封装了适用于本项目的文件上传管理类(FileManager),简化文件上传的调用。

2)自动获取图片信息:

· 在上传图片时,启用“数据万象”功能,对图片进行基础信息解析。
· 解析结果中包含宽度、高度、格式等字段,我再将这些信息写到数据库表的相应字段,便于后续查询与筛选。

这样一来,图片实际的二进制数据并不放在后端服务器或数据库中,而是托管给对象存储服务进行管理,既省去了服务器磁盘维护的繁琐,又能利用云存储高可用的特性,提升系统的稳定性和可扩展性。

## 审核功能

为了避免用户上传的图片内容违规或存在版权风险,给图片模块增加了“审核功能”。

整体策略包括:

在数据库中为**图片表(picture)添加审核状态字段(reviewStatus)和审核信息**(reviewMessage、reviewerld、reviewTime)。
用户上传或编辑图片时,图片默认状态为“待审核”或自动过审(如果是管理员上传);管理员审核后可将状态修改为“通过”或“拒绝”。
对于审核状态为“通过”的图片,普通用户在查询时才能看到;未通过或拒绝的图片仅管理员可见。
管理员可随时修改图片的审核状态,同时记录审核人、审核时间及原因,方便后期追溯。

这样一来,既能保证图片内容的安全与合规,也能将违规图片拒之门外,提高系统整体质量。

此外,其实我还了解更多企业中的审核策略,比如:

内容安全审核服务:借助专业的第三方平台的内容审核服务来实现自动审核,像腾讯云、阿里云等基本都支持图片、文本、音视频等内容的审核。

Al审核:可以将文本内容和审核规则输入给Al,让Al返回是否合规。
**分级审核策略**:区分普通用户与高信誉用户,高信誉用户可减少或免除审核流程,比如VIP用户自动过审,也可以提高部分效率。
**实名信息和内容溯源**:通过用户实名或者手机号注册,提高用户行为的责任感,减少垃圾内容的产生。
**举报机制**:通过给平台增加举报机制,还可以给举报行为一些奖励,让用户帮忙维护平台。



## url上传图片

在云图库项目中,我不仅支持用户上传本地图片文件,也支持直接填写网络图片的URL链接,后端自动下载并导入到对象存储中。大致流程如下:

前端将用户输入的图片**URL发送给后端**。
后端对该链接进行校验:**先用HEAD请求检查链接是否存在**,并获取其文件大小、类型等元信息,而不是下载文件后再校验,可以节约流量。**若检测到格式错误**(非JPEG、PNG等)或文件过大(超过2MB),则拒绝上传。
若链接校验通过,**后端使用Hutool的HttpUtil.downloadFile 将图片下载到本地临时文件**。
将临时文件**上传到对象存储服务**,并利用数据万象接口解析图片宽高、大小、格式等元信息。
将解析到的**信息与文件URL一起写入数据库**,完成导入操作。

其实步骤4和5都可以复用我已经实现的本地图片上传功能,因此我也将两种文件上传方式使用模板方法设计模式重构,减少了重复代码,并提高了代码的
可扩展性和可维护性。

## 降本增效方法

为提升云图库的性能并降低成本,我对图片查询、上传、加载、存储等环节进行了多方位的优化。

1)图片查询优化:

**· 结合Redis和Caffeine,构建了多级缓存体系**,先从本地缓存查询,若未命中再查分布式缓存。这样既能充分利用内存的高速访问,又确保了分布式环境下的数据一致性。
· 将高频访问的热点图片以及复杂查询结果直接缓存,减少对数据库的压力,成倍缩短了首页响应时间。

2)图片上传优化:

· 在上传时利用数据万象服务自动将图片格式转换为WebP进行压缩,有效降低图片文件大小,节省带宽,提升加载速度。
·支持分片上传和断点续传,用于大文件场景,减少网络波动对上传体验的影响。我也尝试过实现文件秒传,通过对文件哈希(MD5)进行校验,避免重复上传。

3)图片加载优化:

**·生成缩略图,**在首页或列表页仅加载缩略图,用户查看图片详情时再加载清晰原图,大幅减少流量。
**· 利用CDN加速,**将图片资源分配到各地CDN节点,用户就近访问,从而降低延迟;并开启浏览器缓存头,进一步提升重复访问的加载速度。
·懒加载技术,让页面只在图片真正出现在可视区域时再请求资源,降低一次性加载的压力。

4)图片存储优化:

· 使用**对象存储配合数据沉降**(生命周期管理)策略,对30天未访问的图片自动切换至低频存储,降低存储费用。
·定期清理不再使用或重复的图片文件,减少冗余占用空间。

经过上述优化,系统整体的性能显著提升,同时有效地控制了存储与带宽成本。

## 用户额度不超标

为了防止用户无限制占用存储资源,我在云图库项目里为每个空间都配置了最大图片数量(maxCount)和最大总大小(maxSize)两种限制,并且在图片上传和图片删除环节都做了相应的更新与校验:

**上传前校验**:根据spaceld 查到该空间的当前已用大小totalSize和数量totalCount,分别与maxSize、maxCount对比。若空间或用户已超出限额,则拒绝这次上传请求。
**上传成功后更新**:进入数据库事务,先写入图片表,再将totalSize累加上这张图片的大小、totalCount+1。若任意一步失败,就回滚事务,保证数据的一致性。删除图片后释放:删除图片时,也在同一事务内把对应的大小和数量从space的totalSize、totalCount字段中减去,确保计量准确。

通过 **上传前校验+事务内原子更新的方式**,即使在并发场景下,也能让空间用量处于可控状态,一旦某个空间达到了最大额度就禁止再传图,大幅降低系统被滥用的风险。

此外,参考大厂云服务的设计,我允许用户小幅超出额度(比如上传最后一张图时,即使会超额也允许上传),一定程度上减少了开发难度,并提升了用户体验。

## 批量编辑

为了提高空间图片的管理效率,我提供了批量编辑功能,包括**批量修改图片的分类、标签、名称等,**能安全快速地完成大量图片的更新操作。

实现过程主要分两步:

在Service中,**先一次性查询出要修改的图片并检查权限**,然后按需更新分类、标签、名称等字段。
使用**MyBatis Plus的 updateBatchByld 批量更新记录**,结合事务保证原子性。

通过下面几个方法,可以优化批量编辑的性能:

· 减少循环SQL:统一分批、批量更新数据库,而不是给每张图都执行一条Update。
·并发执行:如果数据量特别大,可以用CompletableFuture+线程池将不同分段的图片并发处理,并在全部处理完后再统一提交或回滚。

## 空间成员管理

我在项目里新建了一张space_user表,记录spaceld和userld的**多对多关联关系**,并附带spaceRole字段标识了用户在该空间的角色(viewer/editor/admin) .

成员管理操作比较简单,就是编写**增删改查接口+必要的权限校验**。需要注意的是,创建团队空间时,**要先在space表插入团队空间记录,再自动往space_user表插一条数据,赋予创建者管理员角色**。

## 动态分表

Apache ShardingSphere是一个提供**分库分表、读写分离、分布式事务**等功能的开源数据库中间件,支持多种模式(ShardingSphere-JDBC、ShardingSphere-Proxy)和灵活的分片配置方式。

我选择它的ShardingSphere-JDBC模式实现分库分表,主要原因是:

**功能全面**:开箱即用地实现分库分表、聚合查询、分布式事务等

**配置灵活**:能按字段取模、按时间段拆分,也支持自定义分片算法
社区活跃:遇到Bug时能找到很多文档来解决
Java生态:我用的是Spring Boot框架,ShardingSphere-JDBC的嵌入式方案对小项目更简洁轻量,更容易和现有环境集成。

## 协同编辑

我在已有的图片编辑功能上,结合**WebSocket和事件驱动** 思想,让多位团队成员能够实时查看对同一张图片的编辑变化。关键的实现点是:

**WebSocket通信:**前端与后端在建立WebSocket连接后,后端保存每个图片对应的会话集合,以及当前正在编辑的用户信息。
**编辑锁机制:**只有一个用户可以进入编辑状态,其他用户只能实时“围观”该用户操作,这样从源头上解决了并发冲突。
**事件驱动**:每当用户执行放大/旋转等编辑动作,相当于生产了一个“事件”,通过WebSocket服务器把该事件分发给其他连接用户,实时广播更新。

**断开连接:**若正在编辑用户断线或退出,会自动释放编辑锁,其他用户可申请进入编辑状态。

## Ant Design Vue

云图库项目的前端中,我大量使用了Ant Design Vue提供的组件,比如:

**Layout/Sider/Header 布局**:构建整站布局和顶部导航、侧边栏等。
**Menu菜单**:做全局导航栏,点击菜单自动跳转Vue Router 对应路由。
Tabs 页签:在创建图片页面里,用来切换“文件上传”和“URL上传”两个tab。
Form/Input/InputNumber/Select/AutoComplete/Upload/Button 等表单组件:构建各种数据输入的表单。
Table/Pagination:实现管理员查看和操作数据的管理页面,支持分页和排序。
Card/List/Image/Descriptions:在图片列表和图片详情页常用的UI组件,直观地展示图片及其描述信息。
Modal/Message/Notification/Popconfirm:根据场景显示不同的消息提示,提高交互体验。比如用户删除等危险操作时会弹出Popconfirm 确认框



## 组件

除了Ant Design官方组件,我还用到了一些第三方组件,以满足不同的业务需求。比如:

file-saver:用来下载图片文件,支持一键保存URL或Blob文件到本地,用户可一键下载云图库图片。
**dayjs:轻量日期处理库,用于格式化数据库返回的时间**,比如 dayjs(picture.createTime).format('YYYY-MM-DD HH:mm:ss')。
vue-cropper:前端裁剪图片时,会引入它来实现所见即所得的裁剪,允许用户拖拽裁剪区域。
@umijs/openapi:基于OpenAPI规范生成前端请求代码,避免手写接口方法和参数类型。



 

 



 

 

## 两个线程按 abab 顺序打印

核心思路：用**等待** **-** **通知机制**（wait()/notify()），通过共享锁控制线程执行顺序。

```java
public class ThreadPrintAbab {

  // 共享锁对象

  private static final Object LOCK = new Object();

  // 标记：控制打印 a 还是 b

  private static boolean printA = true;

 

  public static void main(String[] args) {

    // 打印 a 的线程

​    Thread threadA = new Thread(() -> {

​      for (int i = 0; i < 5; i++) {

​        synchronized (LOCK) {

​          // 不是打印 a 时等待

​          while (!printA) {

​            try {

​              LOCK.wait();

​            } catch (InterruptedException e) {

​              Thread.currentThread().interrupt();

​            }

​          }

​          System.out.print("a");

​          // 切换标记为打印 b

​          printA = false;

​          // 唤醒等待的线程

​          LOCK.notify();

​        }

​      }

​    });

 

​    // 打印 b 的线程

​    Thread threadB = new Thread(() -> {

​      for (int i = 0; i < 5; i++) {

​        synchronized (LOCK) {

​          // 是打印 a 时等待

​          while (printA) {

​            try {

​              LOCK.wait();

​            } catch (InterruptedException e) {

​              Thread.currentThread().interrupt();

​            }

​          }

​          System.out.print("b");

​          // 切换标记为打印 a

​          printA = true;

​          // 唤醒等待的线程

​          LOCK.notify();

​        }

​      }

​    });

 

​    threadA.start();

​    threadB.start();

  }

}

//输出结果：ababababab（循环 5 次）。
```

 

 



 



 

## websocket

 建立连接的过程，具体分为哪几步，websocket 的头是什么样子

（1）连接建立步骤

2. **TCP** **三次握手**：客户端与服务端先建立 TCP 连接；
3. **客户端发送 HTTP** **升级请求**：发送 GET 请求，请求将 HTTP 协议升级为 WebSocket；
4. **服务端返回 101** **响应**：校验请求头，返回101 Switching Protocols，完成协议升级；
5. **双向通信建立**：TCP 连接保持，双方通过 WebSocket 数据帧全双工通信。

（2）握手请求头（客户端）

http

GET /chat HTTP/1.1

Host: example.com:8080

Upgrade: websocket     // 必须，请求升级协议

Connection: Upgrade    // 必须，保持连接升级

Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw== // 客户端随机16字节Base64串，用于验证

Sec-WebSocket-Version: 13 // 必须，固定协议版本13

Origin: https://example.com// 跨域校验，防CSRF

（3）服务端响应头

http

HTTP/1.1 101 Switching Protocols

Upgrade: websocket

Connection: Upgrade

Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo= // 对Key+固定GUID做SHA1哈希后Base64编码，验证握手

 

## redis 在项目中是用来干啥的，key 是什么

（1）Redis 核心用途

2. **热点数据缓存**：减轻数据库压力，提升接口响应速度；
3. **分布式锁**：防止并发冲突（如秒杀、库存扣减）；
4. **计数器 /** **限流**：实现点赞、浏览量统计，接口限流；
5. **分布式 Session** **共享**：解决集群 Session 同步问题；
6. **消息队列 /** **延迟队列**：轻量级消息场景；
7. **排行榜**：用 ZSet 实现热度榜、积分榜；
8. **位图 /** **签到**：用户签到、在线状态统计；
9. **分布式 ID**：生成全局唯一自增 ID。

（2）Redis Key 设计

·     **规范**：遵循[业务名]:[模块名]:[功能名]:[唯一标识]层级格式（如user:info:1001），用冒号分隔；

·     **要求**：语义清晰、长度≤100 字符、无特殊字符、全局唯一；

·     **类型**：根据业务选择 String、Hash、List、Set、ZSet 等。

 

## 团队空间锁+编程式事务

```java
// 1. 加锁：同一个用户排队进入
String lock = String.valueOf(userId).intern();
synchronized (lock) {
    // 2. 事务：内部的多个数据库操作，要么全成，要么全败
    Long newSpaceId = transactionTemplate.execute(status -> {
        // 检查是否已存在（加锁后，这里绝对不会并发重复）
        boolean exists = this.lambdaQuery()...exists();
        ThrowUtils.throwIf(exists, ...);
        // 操作1：创建空间
        this.save(space);
        // 操作2：创建团队成员（两个操作必须同时成功）
        spaceUserService.save(spaceUser);
        return space.getId();
    });
}
```

## 后端接口文档

Knife4j 是基于Swagger工具的接口文档增强工具

接口文档在/doc.html

## 前后端跨域问题

后端配置类

```java
package com.yupi.yupicturebackend.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * 全局跨域配置
 */
@Configuration  //Spring 启动时会自动加载这个配置类，去掉注解，跨域直接失效
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        // 覆盖所有请求
        registry.addMapping("/**")
                // 允许发送 Cookie 必须的
                .allowCredentials(true)
                // 放行哪些域名（必须用 patterns，否则 * 会和 allowCredentials 冲突）
                .allowedOriginPatterns("*")  //或者前端域名
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .exposedHeaders("*");
    }
}
```



