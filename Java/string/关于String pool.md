# 关于String pool

why pooling String object? 
     String对象是不可变的,一旦创建，就无法改变（除非引用指向新的string）。另外在程序中，String占用内存非常大。因此，将String对象池化收益是很高的。

程序通过**String.intern**方法来控制手动**将string对象缓存**起来。

## 和JAVA6相比，JAVA7 8中的改进

在JAVA6及之前的，String 对象池放在**永久代**中，在调用string.intern()方法时，会将String对象拷贝一份到永久代中。**这样容易导致永久代溢出**（永久代内存容量是有限的，且在运行中是不可变的）。 
     在JAVA7及之后，将String pool移出了永久代中，而将String pool直接放在的**堆**中，这意味着你不再被限制在固定的内存中啦.所有的字符对象将和其他普通对象一样位于堆中.你可以通过调整堆大小来进行调整应用程序。 
     
### 那池中的String对象何时会被回收？ 
          
所有在字符串池的字符对象如果没有任何引用指向他们就会适时的被垃圾回收.当前讨论的所有版本都是这么做的. 如果你要对一个字符进行intern操作 并且没有任何引用指向它-那么它将会在字符串池中被垃圾回收掉. 

### 关于java7,8中的intern()方法的改变： 
当常量池中没有该字符串时，JDK7的intern（）方法的实现不再是在常量池中创建与此String内容相同的字符串，而改为在常量池中记录Java Heap中首次出现的该字符串的引用，并返回该引用。（即intern的效果是常量池有直接返回常量池中的引用，否则返回的heap中第一次创建时的引用）

## 实现
### JVM 字符串池在JAVA 6， 7 , 8中的实现 
     字符串池是使用一个拥有**固定容量的HashMap**。默认的池大小是**1009**.是一个常量在JAVA6早期版本中，随后在java6_30至java6_41中开始为可配置的.而在java 7中一开始就是可以配置的(至少在java7_02中是可以配置的).你需要指定参数 -XX:StringTableSize=N, N是字符串池map的大小. 确宝他是一个为更好的性能预先准备的数字. 
     在java7中,换句话说。你被限制在一个更大的堆内存中.意味着你可以预先设置好String池的大小(这个值取决于你的应用程序需求).通常说来，一旦程序开始内存消耗，内存都是成百M的增长.在这种情况下.给一个拥有100万的String对象的字符串池分8-16M的内存看起来是比较适合的(不要使用1,000,000 作为-XX:StringTaleSize 的值 - 它不是质数;使用1,000,003代替)
     
     
## 什么时候，String会被放入到string pool中


### (1)直接使用双引号声明出来的String对象会直接存储在常量池中


```
String a = "计算机软件"; 
```

> 分析：因为计算机软件五个字直接使用了双引号声明，故JVM会在运行时常量池中首先查找有没有该字符串，有则直接返回该字符串在常量池中的引用；没有则直接在常量池中创建该字符串，然后返回引用。此时，该句代码已经执行完毕，不会在java Heap（堆）中创建内容相同的字符串。该字符串只在常量池中创建了一个String对象。


```
String a = "计算机" + "软件";
```

> 分析：由于JVM存在编译期优化，对于两个直接双引号声明的String的+操作，JVM在编译期会直接优化为“计算机软件”一个字符串。同String a = “计算机软件”，效果是一样的。

### (2)关于new String格式的


```
String a = new String("计算机软件");
```

> 分析：该行代码生成了**两个String对象**（Stack（栈）中的对象引用不在讨论范围内）： 
>      第一步，因为计算机软件五个字直接使用了双引号声明，故JVM会在运行时常量池中首先查找有没有该字符串，有则进入第二步；没有则直接在常量池中创建该字符串，然后进入第二步。 
>      第二步：在常量池中创建了一个String对象之后，由于使用了new，JVM会在Heap（堆）中创建一个内容相同的String对象，然后返回堆中String对象的引用。该行代码分别在常量池和堆中生成了两个内容相同的String对象。


```
String b = "计算机";  
String a = b + "软件";
```

> 分析：由于b是一个String变量，编译期无法确定b的值，故不会优化为一个字符串。即使我们知道b的值，但JVM认为它是个变量，变量的值只能在运行期才能确定，故不会优化。 
>      运行期字符串的+连接符相当于new，故该行代码在Heap中创建了一个内容为“计算机软件”的String对象，并返回该对象的引用。至此，该代码执行完毕，因为没有直接双引号声明计算机软件这5个字的字符串，故常量池中不会生成计算机软件这5个字的字符串。但是会有“计算机”和“软件”这两个String对象，因为他们都用双引号声明了。



```
String final b = "计算机";  
String a = b + "软件";
```

> 分析：该代码与代码四的唯一区别是将b声明为final类型，即为常量。故在编译期JVM能确定b的值，所以对+可以优化为“计算机软件”5个字的字符串。该代码的运行同代码一。


## 例子:
这么备注的结果是用java8运行出的来的

```JAVA
        String str1 = new StringBuilder("ja").append("va").toString();  
        System.out.println(str1 == str1.intern());  //false
        //java是关键字，在string pool中肯定是会有的

        String str2 = new StringBuilder("计算机").append("aa技术").toString();  
        System.out.println(str2 == str2.intern());  //true
        //计算机aa技术 在池中没有，但是在heap中存在，则intern时，会直接返回该heap中的引用

        String s1=new String("test");  //false
        System.out.println(s1==s1.intern()); 
        //test作为字面量，放入了string pool中，而new时，s1指向的是heap中新生成的string对象，地址不同，为false

        String s2=new StringBuilder("abc").toString();//false
        System.out.println(s2==s2.intern()); 
        //同上

        String s3=new StringBuilder("dd").append("ee").toString();
        System.out.println(s3==s3.intern()); //true


        String a = "Alice"; //String Pool
        String b = "Bob";  //String Pool

        String a1 = "Alice"; //String Pool
        String b1 = new String("Bob"); // heap but not in string pool

        System.out.println("a == a1 ? " + (a == a1));//a == a1 ? true
        System.out.println("b == b1 ? " + (b == b1));//b == b1 ? false

```
## 总结
1.直接new的会有两个heap对象（字面量放在常量池中，另外，创建了一个新的值为字面量的string对象，并将改引用返回给了new的左值） 
2.intern不会产生重复的String对象。 
3.字面量是会直接放在String pool中的，其他的你可以通过intern自己放入。

## new String("abc")

使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。

"abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
而使用 new 的方式会在堆中创建一个字符串对象。
创建一个测试类，其 main 方法中使用这种方式来创建字符串对象。

```JAVA
public class NewStringTest {
    public static void main(String[] args) {
        String s = new String("abc");
    }
}
```

使用 javap -verbose 进行反编译，得到以下内容：


```JAVA
// ...
Constant pool:
// ...
   #2 = Class              #18            // java/lang/String
   #3 = String             #19            // abc
// ...
  #18 = Utf8               java/lang/String
  #19 = Utf8               abc
// ...

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=2, args_size=1
         0: new           #2                  // class java/lang/String
         3: dup
         4: ldc           #3                  // String abc
         6: invokespecial #4                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
         9: astore_1
// ...

```

在 Constant Pool 中，#19 存储这字符串字面量 "abc"，#3 是 String Pool 的字符串对象，它指向 #19 这个字符串字面量。在 main 方法中，0: 行使用 new #2 在堆中创建一个字符串对象，并且使用 ldc #3 将 String Pool 中的字符串对象作为 String 构造函数的参数。

以下是 String 构造函数的源码，可以看到，在将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制 value 数组内容，而是都会指向同一个 value 数组。


```java
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```