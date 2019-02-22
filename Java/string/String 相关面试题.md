# String 相关面试题

## 简介
String 被声明为 final，因此它不可被继承。

在 Java 8 中，String 内部使用 char 数组存储数据。


```JAVA
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```
在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 coder 来标识使用了哪种编码。


```JAVA
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```
value 数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

## 不可变的好处

1. 可以缓存 hash 值
因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
2. String Pool 的需要
如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。
3. 安全性
String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。
4. 线程安全
String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

## String, StringBuffer and StringBuilder

##### 1. 可变性
* String 不可变
* StringBuffer 和 StringBuilder 可变
##### 2. 线程安全
* String 不可变，因此是线程安全的
* StringBuilder 不是线程安全的
* StringBuffer 是线程安全的，内部使用 synchronized 进行同步

## String Pool
**字符串常量池（String Pool）**保存着所有字符串字面量（literal strings），这些字面量在**编译时期**就确定。不仅如此，还可以使用 **String 的 intern()** 方法在运行过程中将字符串添加到 String Pool 中。

当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

下面示例中，s1 和 s2 采用 new String() 的方式新建了两个不同字符串，而 s3 和 s4 是通过 s1.intern() 方法取得一个字符串引用。intern() 首先把 s1 引用的字符串放到 String Pool 中，然后返回这个字符串引用。因此 s3 和 s4 引用的是同一个字符串。


```JAVA
String s1 = new String("abc");
String s2 = new String("abc");
System.out.println(s1 == s2);           // false
String s3 = s1.intern();
String s4 = s1.intern();
System.out.println(s3 == s4);           // true
```

如果是采用 "aaa" 这种字面量的形式创建字符串，会自动地将字符串放入 String Pool 中。


```JAVA
String s5 = "aaa";
String s6 = "aaa";
System.out.println(s5 == s6);  // true
```

在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。


## String相关面试题

1. 下面程序的运行结果是什么？


```JAVA
        String s1 = "abc";
        StringBuffer s2 = new StringBuffer(s1);
        System.out.println(s1.equals(s2));//1

        StringBuffer s3 = new StringBuffer("abc");
        System.out.println(s3.equals("abc"));//2

        System.out.println(s3.toString().equals("abc"));//3
```

-------

> false false true
>
答：注释 1 打印为 false，主要考察 String 的 equals 方法，String 源码中 equals 方法有对参数进行 instance of String 判断语句，StringBuffer 的祖先为 CharSequence，所以不相等； 注释 2 打印为 false，因为 StringBuffer 没有重写 Object 的 equals 方法，所以 Object 的 equals 方法实现是 == 判断，故为 false； 注释 3 打印为 true，因为 Object 的 toString 方法返回为 String 类型，String 重写了 equals 方法实现为值比较。


2. 怎样将 GB2312 编码的字符串转换为 ISO-8859-1 编码的字符串？

> 答：

> ```JAVA
>         String s1 = "你好";
>         String s2 = new String(s1.getBytes("GB2312"), "ISO-8859-1");
> ```


3. String、StringBuffer、StringBuilder 的区别是什么？

> String 是字符串常量，StringBuffer 和 StringBuilder 都是字符串变量，后两者的字符内容可变，而前者创建后内容不可变；StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，线程安全会带来额外的系统开销，所以 StringBuilder 的效率比 StringBuffer 高；String 的每次修改操作都是在内存中重新 new 一个对象出来，而 StringBuffer、StringBuilder 则不用，且提供了一定的缓存功能，默认 16 个字节数组的大小，超过默认的数组长度时扩容为原来字节数组的长度 * 2 + 2，所以使用 StringBuffer 和 StringBuilder 时可以适当考虑下初始化大小，以便通过减少扩容次数来提高代码的高效性。


4. String 为什么是不可变的？

> String 不可变是因为在 JDK 中 String 类被声明为一个 final 类，且类内部的 value 字节数组也是 final 的，只有当字符串是不可变时字符串池才有可能实现，字符串池的实现可以在运行时节约很多 heap 空间，因为不同的字符串变量都指向池中的同一个字符串；如果字符串是可变的则会引起很严重的安全问题，譬如数据库的用户名密码都是以字符串的形式传入来获得数据库的连接，或者在 socket 编程中主机名和端口都是以字符串的形式传入，因为字符串是不可变的，所以它的值是不可改变的，否则黑客们可以钻到空子改变字符串指向的对象的值造成安全漏洞；因为字符串是不可变的，所以是多线程安全的，同一个字符串实例可以被多个线程共享，这样便不用因为线程安全问题而使用同步，字符串自己便是线程安全的；因为字符串是不可变的所以在它创建的时候 hashcode 就被缓存了，不变性也保证了 hash 码的唯一性，不需要重新计算，这就使得字符串很适合作为 Map 的键，字符串的处理速度要快过其它的键对象，这就是 HashMap 中的键往往都使用字符串的原因。


5. 说说 String str = "hello world"; 和 String str = new String("hello world"); 的区别？

> 在 java 的 class 文件中有专门的部分用来存储编译期间生成的字面常量和符号引用，这部分叫做 class 文件常量池，在运行期间对应着方法区的运行时常量池，所以 String str = "hello world"; 在编译期间生成了 字面常量和符号引用，运行期间字面常量 "hello world" 被存储在运行时常量池（只保存了一份），而通过 new 关键字来生成对象是在堆区进行的，堆区进行对象生成的过程是不会去检测该对象是否已经存在的，所以通过 new 来创建的一定是不同的对象，即使字符串的内容是相同的。


6. 语句 String str = new String("abc"); 一共创建了多少个对象？

> 这个问题其实有歧义，但是很多公司还特么爱在笔试题里面考察，非要是遇到了就答两个吧（一个是 “xyz”，一个是指向 “xyz” 的引用对象 str）；之所以说有歧义是因为该语句在运行期间只创建了一个对象（堆上的 "abc" 对象），而在类加载过程中在运行时常量池中先创建了一个 "abc" 对象，运行期和类加载又是有区别的，所以这个题目的问法是有些不严谨的。因此这个问题如果换成 String str = new String("abc"); 涉及到几个 String 对象，则答案就是 2 个了。关于这道题可以参考文章《请别再拿“String s = new String("xyz"); 创建了多少个 String 实例”来面试了吧》，链接：http://rednaxelafx.iteye.com/blog/774673/