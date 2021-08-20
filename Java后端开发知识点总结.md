# Java后端开发笔试面试知识点总结

[TOC]

## 1. Java基础

### 1.1 Java基本数据类型

| 数据类型 | 所占字节     | 默认值  | 可表示的范围                   | 包装类    |
| -------- | ------------ | ------- | ------------------------------ | --------- |
| boolean  | 未定         | False   | True/false                     | Boolean   |
| byte     | 1字节(8bit)  | 0       | -128 ~ 127 (-2^7^-2^7^-1)      | Byte      |
| char     | 2字节(16bit) | ‘\u000’ | '\u0000' ~  '\uffff '          | Character |
| short    | 2字节(16bit) | 0       | -32768~32767                   | Short     |
| int      | 4字节(32bit) | 0       | -32768 ~ 32767(-2^31^~2^31^-1) | Integer   |
| long     | 8字节(64bit) | 0       | -2^63^~2^63^-1                 | Long      |
| float    | 4字节(32bit) | 0.0f    | -2^128^~2^128^-1               | Float     |
| double   | 8字节(64bit) | 0.0d    | -2^1024^~2^1024^-1             | Double    |

#### 1.1.1 为什么包装类型不赋值就是Null，而基本类型有默认值且不是Null？

从JVM的角度上来讲，基本数据类型直接存放在Java虚拟机的局部变量表中，而包装类型属于对象类型，对象类型都存放在堆中。相比于对象类型，基本数据类型占用的空间非常小。

> 《深入理解 Java 虚拟机》 ：局部变量表主要存放了编译期可知的基本数据类型**（boolean、byte、char、short、int、float、long、double）**、**对象引用**（reference 类型，它不同于对象本身，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）。

引申，关于8种基本类型包装类和常量池

1. Byte、Short、Integer、Long这四种包装类默认创建了数值为[-128,127]的相应类型的缓存数据，
2. Character创建了数值在[0,127]范围内的缓存数据，
3. Boolean直接返回True或者False。
4. `Float`,`Double` 并没有实现常量池技术

**所有包装类的对象之间的比较，使用`equals`方法比较**

####  1.1.2 基本数据类型的自动转型

> 首先，**boolean类型与其他基本类型不能进行类型的转换（既不能进行自动类型的提升，也不能强制类型转换）， 否则，将编译出错**。

![image-20210819154720072](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819154720072.png)

1. **隐式类型转换**：也叫自动类型转换，由系统自动完成。将一个数值范围小的类型赋给一个数值范围大的数值型变量**，jvm在编译过程中俊将此数值的类型进行了自动提升。在数值类型的自动类型提升过程中，数值精度至少不应该降低（整型保持不变，float->double精度将变高）。

   ```java
   byte ->short(char)->int->long->float->double
   ```

2. **显示类型转换**：也叫强制类型转换， 是从存储范围大的类型到存储范围小的类型.当我们需要将数值范围较大的数值类型赋给数值范围较小的数值类型变量时，由于**此时可能会丢失精度**（1讲到的从int到k型的隐式转换除外），因此，需要人为进行转换。

   ```java
   double→float→long→int→short(char)→byte
   ```

另外，在进行数学运算时的数据类型自动提升与可能需要的强制类型转换。当进行数学运算的时候，数据类型会自动提升到运算符左右较大的一位。以此类推。当将最后的运算结果赋值给指定的数值类型时，可能需要进行强制类型转换。例如:

![img](https://images2015.cnblogs.com/blog/693250/201610/693250-20161027213826640-302500937.png)

a+b会自动提升为int, 因此在给c赋值的时候要强制转换成byte.

强烈建议看一下[参考](https://www.cnblogs.com/liujinhong/p/6005714.html)

### 1.2 标识符和关键字的区别是什么？

在我们编写程序的时候，需要大量地为程序、类、变量、方法等取名字，于是就有了标识符，简单来说，标识符就是一个名字。但是有一些标识符，Java 语言已经赋予了其特殊的含义，只能用于特定的地方，这种特殊的标识符就是关键字。因此，关键字是被赋予特殊含义的标识符。比如，在我们的日常生活中 ，“警察局”这个名字已经被赋予了特殊的含义，所以如果你开一家店，店的名字不能叫“警察局”，“警察局”就是我们日常生活中的关键字。

### 1.3 Java中常见的关键字

|                      |          |            |          |              |            |           |        |
| -------------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ |
| 访问控制             | private  | protected  | public   |              |            |           |        |
| 类，方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |
|                      | new      | static     | strictfp | synchronized | transient  | volatile  |        |
| 程序控制             | break    | continue   | return   | do           | while      | if        | else   |
|                      | for      | instanceof | switch   | case         | default    |           |        |
| 错误处理             | try      | catch      | throw    | throws       | finally    |           |        |
| 包相关               | import   | package    |          |              |            |           |        |
| 基本类型             | boolean  | byte       | char     | double       | float      | int       | long   |
|                      | short    | null       | true     | false        |            |           |        |
| 变量引用             | super    | this       | void     |              |            |           |        |
| 保留字               | goto     | const      |          |              |            |           |        |

#### 1.3.1 continue、break、return的区别

主要区别体现在循环里面，正常情况下，循环到一定数量级的时候，程序会自动跳出循环，但是有的时候，我们需要在循环的时候进行一些行为的控制，这就需要这几个关键字了。

- continue：指跳出当前这一次循环，继续下一次的循环。
- break：指跳出整个循环体，继续执行循环下面的语句。
- return：return用在循环体内，直接跳出方法，表示该方法执行结束

但是return还有以下两种用法：

1. return；：直接使用return结束方法执行，用于没有返回值函数的方法
2. return value：return一个特定的值，用于返回有返回值函数的方法。

### 1.4 Java泛型

#### 1.4.1 什么是泛型？

Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了**编译时类型安全检测机制**，该机制允许程序员在编译时检测到非法的类型。泛型的本质是**参数化类型**，也就是说所操作的数据类型被指定为一个参数。

#### 1.4.2 什么是类型擦除？TODO

Java的泛型都是伪泛型，因为Java在编译期间，所有的泛型信息都会被擦除，这也就是通常所说的类型擦除。

#### 1.4.3 泛型的使用方式

##### 泛型类

```java
public class Generic <T>{
    private T key;
    public Generic(T key){
        this.key = key;
    }
    
    public T getKey(){
        return this.key;
    }

    public static void main(String[] args) {
        Generic<Integer> generic = new Generic<>(123);
    }
}
```

##### 泛型接口

```java
public interface Generator<T> {
    public T method();
}
```

实现泛型接口，但是不指定类型

```java
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}
```

实现泛型接口，指定类型

```java
class GeneratorImpl implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```

##### 泛型方法

```java
public static <E> void printArray(E[] inputArray) {
    for (E element : inputArray) {
        System.out.printf("%s ", element);
    }
    System.out.println();
}
```

#### 常用的通配符

Java中常用的通配符有：T，E，K，V，？

- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个 java 类型
- K V (key value) 分别代表 java 键值中的 Key Value
- E (element) 代表 Element

### 1.5 Object类常见方法总结

```java
public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作
```

### 1.6 反射

#### 1.6.1 什么是反射？

反射：运行时分析类以及执行类中方法的能力，通过反射可以获取任意一个类的所有属性和方法，并且可以显式的调用这些方法和属性。

#### 1.6.2 反射机制的优缺点

**优点**：可以让我们的代码更加灵活，为各种框架提供开箱即用的功能提供了便利。

**缺点**：在运行时有了分析操作类的能力，这同样增加了安全问题，比如可以无视泛型参数的安全检查。另外，反射的性能也稍微差点。

#### 1.6.2 反射的应用场景

在spring、spring boot、mybatis中大量使用了反射。这些框架中大量使用了反射，而动态代理的实现也是依赖反射。

JDK动态代理示例代码

```java
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;

    public DebugInvocationHandler(Object target) {
        this.target = target;
    }


    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("after method " + method.getName());
        return result;
    }
}
```

另外，注解的实现也用到了反射。

### 1.7 异常

#### Java异常类层次结构图

![img](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-12/Java%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE.png)

#### 异常类图

![img](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/2020-12/Java%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE2.png)

`Exception` 和 `Error` 二者都是 Java 异常处理的重要子类，各自都包含大量子类。

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 受检查异常(必须处理) 和 不受检查异常(可以不处理)。
- **`Error`** ：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获 。例如，Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

### 1.8 I/O流

#### 1.8.1 序列化与反序列化

**序列化**：将数据结构或者对象转换成为二进制字节流的过程

**反序列化**：将在序列化过程中所生成的二进制字节流的过程转换成为数据结构或者对象的过程。

序列化的目的是通过网络传输对象或者说是对象存储到文件系统、数据库、内存中。

#### 1.8.2 获取键盘输入的常用两种方法

1. 通过Scanner：

   ```java
   Scanner input = new Scanner(System.in);
   String s  = input.nextLine();
   input.close();
   ```

2. 通过BufferedReader

   ```java
   BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
   String s = input.readLine();
   ```

#### 1.8.3 Java中IO流的分类

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

Java Io 流共涉及 40 多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java I0 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

按操作方式分类结构图：

![IO-操作方式分类](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/IO-%E6%93%8D%E4%BD%9C%E6%96%B9%E5%BC%8F%E5%88%86%E7%B1%BB.png)

按操作对象分类结构图：

![IO-操作对象分类](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/IO-%E6%93%8D%E4%BD%9C%E5%AF%B9%E8%B1%A1%E5%88%86%E7%B1%BB.png)



-----

## 数据结构

### 基本数据结构

#### 栈

n个字符或者数字入栈，最多可以组成多少个不同的字符串或者数组？

>组合数学中的Catalan数， C(2n,n)/(n+1) （C(2n,n)表示2n里取n） 。

#### 队列

书上给的公式为((rear-front)+MAXSIZE)%MAXSIZE。考虑以下两个情况。当rear小于front时，rear-front为负数，所以需要加上最大存储容量。但是rear大于front时，rear-front+MAXSIZE比实际长度大了MAXSIZE。需要取余。本题告诉rear大于front则只需rear-front。

循环队列判断满队列的条件，最多元素为m：`(rear+1)%m==front`

#### 树

树的性质：

![image-20210820113552848](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210820113552848.png)

二叉树的性质：

![image-20210820142036490](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210820142036490.png)



### 重要算法及思想

#### 

#### 树

红黑树 TODO

B树 TODO

B+树 TODO

KMP算法



### 牛客笔试常见题型

#### 前中后缀表达式的相互转化

**法一：**遇到数字时，加入后缀表达式；
遇到运算符时：
  若为’(’，入栈；
  若为’)’，依次把栈中运算符加入后缀表达式，直到出现’(’；
  若为除括号外其他运算符，当其优先级高于除括号外栈顶运算符时，入栈。否则从栈顶开始依次弹出比当前运算符优先级高或者相等的运算符，直到遇到优先级比它低的运算符或左括号为止。

**法二**：将中缀表达式转为树的中序遍历，然后遍历得前序遍历即为前缀遍历，后序遍历即为后缀遍历。

**法三**：推荐。更通用的算法：

```
例子：这里我给出一个中缀表达式：a+b*c-(d+e)

第一步：按照运算符的优先级对所有的运算单位加括号：式子变成了：((a+(b*c))-(d+e))

第二步：转换前缀与后缀表达式

前缀：把运算符号移动到对应的括号前面

则变成了：-( +(a *(bc)) +(de))

把括号去掉：-+a*bc+de 前缀式子出现

后缀：把运算符号移动到对应的括号后面

则变成了：((a(bc)* )+ (de)+ )-

把括号去掉：abc*+de+- 后缀式子出现
```



#### 筛选法建堆主要思想

1. 首先将N个数据元素存入一个一维数组，并把这个数组视作一棵完全二叉树。（从位置为n/2取下整的元素开始建堆）
2. 从二叉树的最后一个非叶结点开始用从上向下过滤的方法调整以该非叶结点为根节点的二叉树为最大堆。
3. 对前面的结点依次执行2的操作直到根结点执行完成为止。此时这棵二叉树就调整为了一个最大堆。









-----

## MySQL数据库

### 聚簇索引和非聚簇索引

**聚簇索引**：聚簇索引就是按照每张表的主键构造一颗B+树，同时叶子节点中存放的就是整张表的行记录数据，也将聚集索引的叶子节点称为数据页。这个特性决定了索引组织表中数据也是索引的一部分，每张表只能拥有一个聚簇索引。

*聚簇索引的优缺点：*

优点：

1. 数据访问更快，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快
2. 聚簇索引对于主键的排序查找和范围查找速度非常快

缺点：

1. 插入速度严重依赖于插入顺序，按照主键的**顺序插入**是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个**自增的ID列为主键**
2. **更新主键的代价很高**，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义主键为不可更新。
3. 二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据。

**辅助索引（非聚簇索引）**：在**聚簇索引之上创建的索引称之为辅助索引**，辅助索引访问数据总是需要二次查找。辅助索引叶子节点存储的不再是行的物理位置，而是主键值。通过辅助索引首先找到的是主键值，再通过主键值找到数据行的数据页，再通过数据页中的Page Directory找到数据行。

　　Innodb辅助索引的叶子节点并**不包含行记录的全部数据**，叶子节点除了包含键值外，还包含了相应行数据的聚簇索引键。

　　辅助索引的存在不影响数据在聚簇索引中的组织，所以一张表可以有多个辅助索引。在innodb中有时也称辅助索引为二级索引。

**聚簇索引和非聚簇索引的区别**

1. 聚簇索引的叶子节点存放的是主键值和数据行，**支持覆盖索引**；二级索引的叶子节点存放的是主键值或指向数据行的指针。
2. 由于节子节点(数据页)只能按照一颗B+树排序，故**一张表只能有一个聚簇索引**。辅助索引的存在不影响聚簇索引中数据的组织，所以一张表可以有多个辅助索引

#### 引申：InnoDB的索引实现&&MyIsam索引实现

##### 引擎：InnoDB

首先，InnoDB**也使用B+Tree作为索引结构**。

###### 1. 主键索引（聚簇索引）

**InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构**，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。![image-20210819165046664](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819165046664.png)

> 因为InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则**MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形**。

###### 2. 辅助索引（非聚簇索引）

InnoDB的所有辅助索引都引用主键作为data域。例如，下图为定义在Col3上的一个辅助索引。

![image-20210819165217879](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819165217879.png)

需要注意的是：

- 定义聚簇索引的时候，主键需要定义的小一点，如果**定义的大的话，其他索引也会很大**。如果想在表上定义更多的索引，则争取把主键定义的小一些，这样InnoDB不会压缩索引

- 与聚簇索引相比，聚簇索引的实现方式使得主键的搜索特别快，但是辅助索引需要检索两遍：首先在辅助索引上获取到主键，然后到聚簇索引上根据主键取得表的数据，这个过程也叫回表。通过描述我们可以明白，跟聚簇索引相比，非聚簇索引需要两次遍历B+树，所以效率比较慢。

  > 举个例子：
  >
  > InnoDB使用的是聚簇索引，将主键组织到一棵B+树中，而行数据就储存在叶子节点上，若使用"where id = 14"这样的条件查找主键，则按照B+树的检索算法即可查找到对应的叶节点，之后获得行数据。若对Name列进行条件搜索，则需要两个步骤：第一步在辅助索引B+树中检索Name，到达其叶子节点获取对应的主键。第二步使用主键在主索引B+树种再执行一次B+树检索操作，最终到达叶子节点即可获取整行数据。

###### 关于InnoDB其他的知识点：

- 一般建表会用一个自增主键做聚簇索引，没有的话MySQL会默认创建，但是这个主键如果更改代价较高，故建表时要考虑自增ID不能频繁update这点。

- 聚簇索引并不是一种单独的索引类型，而**是一种数据存储方式**。具体细节依赖于其实现方式。

- MySQL数据库中innodb存储引擎，B+树索引可以分为聚簇索引（也称聚集索引，clustered index）和辅助索引（有时也称非聚簇索引或二级索引，secondary index，non-clustered index）。这两种索引内部都是B+树，聚集索引的叶子节点存放着一整行的数据。

- Innobd中的主键索引是一种聚簇索引，非聚簇索引都是辅助索引，像复合索引、前缀索引、唯一索引。

- Innodb使用的是聚簇索引，MyISam使用的是非聚簇索引

##### 引擎MyIsam

MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址

###### 主键索引

MyISAM引擎使用B+Tree作为索引结构，叶节点的**data域存放的是数据记录的地址**。下图是MyISAM主键索引的原理图：

![image-20210819165955651](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819165955651.png)

###### 辅助索引

在MyISAM中，主索引和辅助索引（Secondary key）在结构上没有任何区别，**只是主索引要求key是唯一的，而辅助索引的key可以重复**。如果我们在Col2上建立一个辅助索引，则此索引的结构如下图所示：

![image-20210819170029766](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819170029766.png)

同样也是一颗B+Tree，data域保存数据记录的地址。因此，MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。

##### 两者的对比：

![image-20210819170128721](https://gitee.com/yamonc/blogImage/raw/master//img/blogImage/image-20210819170128721.png)

#### 引申：覆盖索引

覆盖索引（covering index）指一个查询语句的执行只用从索引中就能够取得，不必从数据表中读取。也可以称之为实现了索引覆盖。 当一条查询语句符合覆盖索引条件时，MySQL只需要通过索引就可以返回查询所需要的数据，这样避免了查到索引后再返回表操作，减少I/O提高效率。 如，表covering_index_sample中有一个普通索引 idx_key1_key2(key1,key2)。当我们通过SQL语句：select key2 from covering_index_sample where key1 = ‘keytest’;的时候，就可以通过覆盖索引查询，无需回表。

-----

## 必会代码

### 单例模式TODO

### 死锁代码TODO

