# Android 面试题及答案 - Java

[![License](https://img.shields.io/badge/license-Apache%202-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)

> 熟练掌握java是很关键的，大公司不仅仅要求你会使用几个api，更多的是要你熟悉源码实现原理，甚至要你知道有哪些不足，怎么改进，还有一些java有关的一些算法，设计模式等等。


* 一、基本数据类型
* 二、[String相关面试题](https://github.com/shanzi716liya/AndroidInterview/blob/master/Java/string/String%20%E7%9B%B8%E5%85%B3%E9%9D%A2%E8%AF%95%E9%A2%98.md)
* 三、运算
* 四、继承
* 五、Object 通用方法
* 六、关键字
* 七、反射
* 八、异常
* 九、泛型
* 十、注解
* 十一、特性
* Java 各版本的新特性
* Java 与 C++ 的区别
* JRE or JDK

（一） java基础面试知识点

- [java中==和equals和hashCode的区别](https://github.com/shanzi716liya/AndroidInterview/blob/master/Java/level-1/diff-equals-hashCode.md)

- int、char、long各占多少字节数

  int : 4字节；char：2字节；long：8字节；

- int与integer的区别

  - int 为基本类型；

  - integer 为对象; 
  - int 能直接使用 == 比较是否相等。Integer 是类对象，使用 == 比较是比较的对象地址，应该使用 equals 方法判断是否相等。（注意：Integer 对象内部初始化了一个默认数组，该数组从-128到127；如果通过自动装箱或 valueOf 方法初始化值在 -128 到 127的 Integer 对象，那么会自动返回默认数组中对应的对象。所以这时候通过 == 比较是否相等是返回 true。）

- 谈谈对java多态的理解

  表示"不同形态"，实际上意味着，根据实现方法的对象，相同方法名称具有不同的行为。多态主要通过：接口实现，虚方法实现，和 类继承方法重写三种方式实现。

- String、StringBuffer、StringBuilder区别

  * String 是基本类型，一旦创建则无法修改值。 
  * StringBuffer 和 StringBuilder 内部其实是一个默认长度为 16 的字符数组。调用 append 方法的时候是吧添加的字符串的字符拷贝到数组中，如果添加后的长度超过了数组的长度，那么会新建一个数组，并将原来的数组的值拷贝到新数组中。StringBuffer 和 StringBuilder 的区别是 StringBuffer 在调用 append 方法时是线程安全的，而 StringBuilder 是线程非安全的。

- 什么是内部类？内部类的作用

- 抽象类和接口区别

  抽象类可以有自己实现的方法和字段。接口只能定义了需要被实现的方法，且接口中的属性为final 类型不可修改。

- 抽象类的意义

- 抽象类与接口的应用场景

- 抽象类是否可以没有方法和属性？

  可以。抽象类中可以没有抽象方法，但有抽象方法的一定是抽象类。

- 接口的意义

- 泛型中extends和super的区别

- 父类的静态方法能否被子类重写

- 进程和线程的区别

  线程是 CPU 可调度执行的最小单位。而进程是一块包含了某些资源的内存区域。操作系统利用进程把它的工作划分为一些功能单元。简而言之，一个进程可以有多个线程，操作系统可以有多个进程。

- final，finally，finalize的区别

  * final ：关键字用于声明属性值初始化后不可修改，声明方法不可重载，声明类不可被继承；

  * finally：用于try catch 块中，使代码片段无论是否异常都能执行。

  * finalize：所有类对象都可以实现该方法。垃圾回收器在回收对象前会调用对象的该方法。但开发者不应该依赖于该方法来回收资源，因为很难确定该方法在什么时候会被调用

- 序列化的方式

  Serializable 和Parcelable

- Serializable 和Parcelable 的区别

- 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？

  因为子类可以继承父类的属性和方法，所以可以被继承。但是因为静态属性和方法属于类所有，所以不能被重写

- 静态内部类的设计意图

- 成员内部类、静态内部类、局部内部类和匿名内部类的理解，以及项目中的应用

  - 内部类：内部类对同一个包下的其他类不可见。因为成员内部类在编译器编译的时候会自动生成外部类对象的引用，所以在类内部定义，能够直接访问外部类的属性和方法，且只有创建了外部类才能够通过外部类来创建内部类。内部类不能够包含静态成员变量和静态成员方法
  - 静态内部类：类似成员内部类，但静态内部类在编译的时候编译器不会自动创建外部类对象的引用。这意味着静态内部类不能够访问外部类的非静态成员变量和方法，且它的创建不依赖于外部类。
  - 局部内部类：在方法域中定义的类。不能够使用public 或 private ,protected权限符声明；除了在定义的作用域，对外部不可见。局部内部类对比内部类，不仅可以访问外部类的属性和方法，还能够访问定义域的局部变量（局部变量需要声明为 final，因为局部内部类在访问局部变量是，事实上是在创建局部内部类对象的时候，拷贝了一份局部变量，只有声明为 final 类型的局部变量，在初始化后值不会改变而保证局部内部类的拷贝与外界一致）
  - 匿名内部类：对于局部内部类，如果在局部方法中只需要创建一个类对象，那么连类的名字也不需要命名；

- 谈谈对kotlin的理解

- 闭包和局部内部类的区别

- string 转换成 integer的方式及原理

（二） java深入源码级的面试题（有难度）

- 哪些情况下的对象会被垃圾回收机制处理掉？
- 讲一下常见编码方式？
- utf-8编码中的中文占几个字节；int型几个字节？
- 静态代理和动态代理的区别，什么场景使用？
- Java的异常体系
- 谈谈你对解析与分派的认识。
- 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？
- Java中实现多态的机制是什么？
- 如何将一个Java对象序列化到文件里？
- 说说你对Java反射的理解
- 说说你对Java注解的理解
- 说说你对依赖注入的理解
- 说一下泛型原理，并举例说明
- Java中String的了解
- String为什么要设计成不可变的？
- Object类的equal和hashCode方法重写，为什么？

（三） 数据结构

- 常用数据结构简介
- 并发集合了解哪些？
- 列举java的集合以及集合之间的继承关系
- 集合类以及集合框架
- 容器类介绍以及之间的区别（容器类估计很多人没听这个词，Java容器主要可以划分为4个部分：List列表、Set集合、Map映射、工具类（Iterator迭代器、Enumeration枚举类、Arrays和Collections），具体的可以看看这篇博文 Java容器类）
- List,Set,Map的区别
- List和Map的实现方式以及存储方式
- HashMap的实现原理
- HashMap数据结构？
- HashMap源码理解
- HashMap如何put数据（从HashMap源码角度讲解）？
- HashMap怎么手写实现？
- ConcurrentHashMap的实现原理
- ArrayMap和HashMap的对比
- HashTable实现原理
- TreeMap具体实现
- HashMap和HashTable的区别
- HashMap与HashSet的区别
- HashSet与HashMap怎么判断集合元素重复？
- 集合Set实现Hash怎么防止碰撞
- ArrayList和LinkedList的区别，以及应用场景
- 数组和链表的区别
- 二叉树的深度优先遍历和广度优先遍历的具体实现
- 堆的结构
- 堆和树的区别
- 堆和栈在内存中的区别是什么(解答提示：可以从数据结构方面以及实际实现方面两个方面去回答)？
- 什么是深拷贝和浅拷贝
- 手写链表逆序代码
- 讲一下对树，B+树的理解
- 讲一下对图的理解
- 判断单链表成环与否？
- 链表翻转（即：翻转一个单项链表）
- 合并多个单有序链表（假设都是递增的）

（四） 线程、多线程和线程池

- 开启线程的三种方式？
- 线程和进程的区别？
- 为什么要有线程，而不是仅仅用进程？
- run()和start()方法区别
- 如何控制某个方法允许并发访问线程的个数？
- 在Java中wait和seelp方法的不同；
- 谈谈wait/notify关键字的理解
- 什么导致线程阻塞？
- 线程如何关闭？
- 讲一下java中的同步的方法
- 数据一致性如何保证？
- 如何保证线程安全？
- 如何实现线程同步？
- 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
- 线程间操作List
- Java中对象的生命周期
- Synchronized用法
- synchronize的原理
- 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
- static synchronized 方法的多线程访问和作用
- 同一个类里面两个synchronized方法，两个线程同时访问的问题
- volatile的原理
- 谈谈volatile关键字的用法
- 谈谈volatile关键字的作用
- 谈谈NIO的理解
- synchronized 和volatile 关键字的区别
- synchronized与Lock的区别
- ReentrantLock 、synchronized和volatile比较
- ReentrantLock的内部实现
- lock原理
- 死锁的四个必要条件？
- 怎么避免死锁？
- 对象锁和类锁是否会互相影响？
- 什么是线程池，如何使用?
- Java的并发、多线程、线程模型
- 谈谈对多线程的理解
- 多线程有什么要注意的问题？
- 谈谈你对并发编程的理解并举例说明
- 谈谈你对多线程同步机制的理解？
- 如何保证多线程读写文件的安全？
- 多线程断点续传原理
- 断点续传的实现

（五）并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）：

平时Android开发中对并发编程可以做得比较少，Thread这个类经常会用到，但是我们想提升自己的话，一定不能停留在表面，,我们也应该去了解一下java的关于线程相关的源码级别的东西。
学习的参考资料如下：

- Java 内存模型

- java线程安全总结
  深入理解java内存模型系列文章

线程状态：

- 一张图让你看懂JAVA线程间的状态转换

锁：

- 锁机制：synchronized、Lock、Condition
  Java 中的锁

并发编程：

- Java并发编程：Thread类的使用
- Java多线程编程总结
- Java并发编程的总结与思考
- Java并发编程实战-----synchronized
- 深入分析ConcurrentHashMap