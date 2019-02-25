# Android 面试题及答案 - Java

[![License](https://img.shields.io/badge/license-Apache%202-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)

> 熟练掌握java是很关键的，大公司不仅仅要求你会使用几个api，更多的是要你熟悉源码实现原理，甚至要你知道有哪些不足，怎么改进，还有一些java有关的一些算法，设计模式等等。

<!-- GFM-TOC -->
* [一、Java基础面试知识点](#一Java基础面试知识点)
* [二、Java深入源码级的面试题（有难度）](#Java深入源码级的面试题（有难度）)
* [三、线程、多线程和线程池](#三线程、多线程和线程池)
* [四、并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）](#并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）)
<!-- GFM-TOC -->

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

# 一、 Java基础面试知识点

### Java中==和equals和hashCode的区别

  - 基本数据类型的==比较的值相等.
  - 类的==比较的内存的地址，即是否是同一个对象，在不覆盖equals的情况下，同比较内存地址，原实现也为 == ，如String等重写了equals方法
  - hashCode也是Object类的一个方法。返回一个离散的int型整数。在集合类操作中使用，为了提高查询速度。（HashMap，HashSet等比较是否为同一个）
  - 如果两个对象equals，Java运行时环境会认为他们的hashcode一定相等。
  - 如果两个对象不equals，他们的hashcode有可能相等。
  - 如果两个对象hashcode相等，他们不一定equals。
  - 如果两个对象hashcode不相等，他们一定不equals



### int、char、long各占多少字节数

| TYPE                | BYTE   | BIT    |
| ------------------- | ------ | ------ |
| int           float | 4 byte | 32 bit |
| short       char    | 2 byte | 16 bit |
| double     long     | 8 byte | 64 bit |



### int与Integer的区别

  - int 基本类型

  - Integer 对象，int的封装类



### 谈谈对Java多态的理解

  - 继承、重写、向上转型。实现多态形式：继承和接口

  - 使用父类类型的引用指向子类的对象

  - 该引用只能调用父类中定义的方法和变量

  - 如果子类中重写了父类中的一个方法，那么在调用这个方法的时候，将会调用子类中的这个方法；（动态连接、动态调用）



### String、StringBuffer、StringBuilder区别

  - String：字符串常量 不适用于经常要改变值得情况，每次改变相当于生成一个新的对象

  - StringBuilder：字符串变量（线程不安全） 确保单线程下可用，效率略高于StringBuffer

  - StringBuffer：字符串变量 （线程安全）



### 什么是内部类？内部类的作用

  内部类可直接访问外部类的属性
  Java中内部类主要分为**成员内部类**、**局部内部类**(嵌套在方法和作用域内)、**匿名内部类**（没构造方法）、**静态内部类**（static修饰的类，不能使用任何外围类的非static成员变量和方法， 不依赖外围类）



### 抽象类和接口区别

  - 均不能被实例化 不能直接实例化 多态实例化其子类

  - 接口抽象级别高于抽象类

  - 接口只能声明方法，抽象类既可以声明方法也可以有自己实现的方法和字段

  - 抽象类内可以没有抽象方法,抽象方法不能是静态以及私有的

  - 抽象类单继承，接口多实现

  - 接口内可含不可变常量，之前没用过。但将常量变量放在interface中违背了其作为接口的作用而存在的宗旨，也混淆了interface与类的不同价值



### 抽象类的意义

  - 我们在开始使用的时候就是用的接口，后来实现的子类里有些子类有共同属性，或者相同的方法实现，所以提取出来一个抽象类，作为类和接口的中介。
  - 只有抽象类没有抽象对象
  - 定义模板



### 抽象类与接口的应用场景



### 抽象类是否可以没有方法和属性？

  可以。抽象类中可以没有抽象方法，但有抽象方法的一定是抽象类。



### 接口的意义



### 泛型中extends和super的区别

  - super：<? super T>表示包括T在内的任何T的父类 下界通配符
  - extends：<? extends T>表示包括T在内的任何T的子类 上界通配符



### 父类的静态方法能否被子类重写

  - 可被继承 不能被重写
  - 静态方法从程序开始运行后就已经分配了内存，子类中如果定义了相同名称的静态方法，并不会重写，而应该是在内存中又分配了一块给子类的静态方法。



### 进程和线程的区别

  - 进程是cpu资源分配的最小单位，线程是cpu调度的最小单位。
  - 进程之间不能共享资源，而线程共享所在进程的地址空间和其它资源。
  - 一个进程内可拥有多个线程，进程可开启进程，也可开启线程。
  - 一个线程只能属于一个进程，线程可直接使用同进程的资源，线程依赖于进程而存在。



### final，finally，finalize的区别

  * final：修饰类、成员变量和成员方法，类不可被继承，成员变量不可变，成员方法不可重写

  * finally：与try...catch...共同使用，确保无论是否出现异常都能被调用到

  * finalize：类的方法，垃圾回收之前会调用此方法，子类可以重写finalize()方法实现对资源的回收



### 序列化的方式

  - Serializable和Parcelable
  - static、trasient修饰的变量不可被序列化
  - 若一个类可序列化，其子类，内部类都要可序列化



### Serializable和Parcelable 的区别

  - Serializable Java 序列化接口 在硬盘上读写 读写过程中有大量临时变量的生成
  - Parcelable Android 序列化接口 效率高 使用麻烦 在内存中读写（AS有相关插件 一键生成所需方法）



### 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？

  - 因为子类可以继承父类的属性和方法，所以可以被继承。但是因为静态属性和方法属于类所有，所以不能被重写而是被隐藏

  - 如果子类里面定义了静态方法和属性，那么这时候父类的静态方法或属性称之为"隐藏"。如果你想要调用父类的静态方法和属性，直接通过父类名.方法或变量名完成。



### 静态内部类的设计意图

  - 降低包的深度，方便类的使用，不依赖与外在的类，不用使用外在类的非静态属性和方法，只是为了方便管理类结构而定义。在创建静态内部类的时候，不需要外部类对象的引用。

  - 普通内部类拥有外部类的指针，可以随意使用外部类的变量及方法。生成实例时需先生成外部类的实例。



### 成员内部类、局部内部类、匿名内部类和静态内部类的理解，以及项目中的应用

  - Java中内部类主要分为**成员内部类**、**局部内部类**(嵌套在方法和作用域内)、**匿名内部类**（没构造方法）、**静态内部类**（static修饰的类，不能使用任何外围类的非static成员变量和方法， 不依赖外围类）

  - 内部类：内部类对同一个包下的其他类不可见。因为成员内部类在编译器编译的时候会自动生成外部类对象的引用，所以在类内部定义，能够直接访问外部类的属性和方法，且只有创建了外部类才能够通过外部类来创建内部类。内部类不能够包含静态成员变量和静态成员方法

  - 局部内部类：在方法域中定义的类。不能够使用public 或 private ,protected权限符声明；除了在定义的作用域，对外部不可见。局部内部类对比内部类，不仅可以访问外部类的属性和方法，还能够访问定义域的局部变量（局部变量需要声明为 final，因为局部内部类在访问局部变量是，事实上是在创建局部内部类对象的时候，拷贝了一份局部变量，只有声明为 final 类型的局部变量，在初始化后值不会改变而保证局部内部类的拷贝与外界一致）

  - 匿名内部类：对于局部内部类，如果在局部方法中只需要创建一个类对象，那么连类的名字也不需要命名；

  - 静态内部类：类似成员内部类，但静态内部类在编译的时候编译器不会自动创建外部类对象的引用。这意味着静态内部类不能够访问外部类的非静态成员变量和方法，且它的创建不依赖于外部类。

  - 应用

    - 使用内部类最吸引人的原因是：每个内部类都能独立地继承一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响。

    - 因为Java不支持多继承，支持实现多个接口。但有时候会存在一些使用接口很难解决的问题，这个时候我们可以利用内部类提供的、可以继承多个具体的或者抽象的类的能力来解决这些程序设计问题。可以这样说，接口只是解决了部分问题，而内部类使得多重继承的解决方案变得更加完整。



### 谈谈对Kotlin的理解

  Kotlin是对Java的一层包装，语法接近于函数式编程，基于JVM设计,100%兼容Java,切换成本小。两种代码可并存，互相调用，互相转换。

  - Null 安全

  - Lambda 方便进行函数式编程

  - 流式API

  - ...



### 闭包和局部内部类的区别

  - 闭包能够将一个方法作为一个变量去存储，这个方法有能力去访问所在类的自由变量。

  - 闭包的价值在于可以作为函数对象或者匿名函数，持有上下文数据，作为第一级对象进行传递和保存。闭包广泛用于回调函数、函数式编程中。

  - 在Java中，闭包是 通过“接口与内部类实现的”



### String 转换成 Integer的方式及原理

  - Integer.valueOf(str)


# 二、 java深入源码级的面试题（有难度）

### 哪些情况下的对象会被垃圾回收机制处理掉？

  - Java GC机制主要完成3件事：确定哪些内存需要回收，确定什么时候需要执行GC，如何执行GC。

  - 分代分配，分代回收。对象将根据存活的时间被分为：年轻代（Young Generation）、年老代（Old Generation）、永久代（Permanent Generation，也就是方法区）

  - **年轻代Minor GC或叫Young GC:** 采用停止-复制（Stop-and-copy）”清理法。分为Eden区和两个Survivor区。内存首先在Eden区内连续分配，当Eden满时执行一次GC，仍存活的移入同一个survivor区，重复上述过程直至当前survivor区满，GC后仍存活的直接移入另一个survivor区（总有一个survivor为空）

  - **年老代Major GC也叫Full GC:** 对象如果在年轻代存活了足够长的时间而没有被清理掉（即在几次 Young GC后存活了下来），则会被复制到年老代，年老代的空间一般比年轻代大，能存放更多的对象，在年老代上发生的GC次数也比年轻代少。

  - 如果对象比较大（比如长字符串或大数组），Young空间不足，则大对象会直接分配到老年代上（大对象可能触发提前GC，应少用，更应避免使用短命的大对象）。

  - 年老代中维护一个512 byte的块——”card table“，所有老年代对象引用新生代对象的记录都记录在这里。Young GC时，只要查这里即可，不用再去查全部老年代，因此性能大大提高。
    标记-清除，标记-整理算法

    ```
    **方法区（永久代）：**永久代的回收有两种：常量池中的常量，无用的类信息，常量的回收很简单，没有引用了就可以被回收。对于无用的类进行回收，必须保证3点：
    
    1. 类的所有实例都已经被回收
    2. 加载类的ClassLoader已经被回收
    3. 类对象的Class对象没有被引用（即没有通过反射引用该类的地方）
    
    引用计数法(循环引用时失效)  标记-清除算法 标记-整理算法 copying算法 generation算法
    ```



### 讲一下常见编码方式？

  - ASCII 码 128位

  - GBK 《汉字内码扩展规范》

  - UTF-16 Unicode码 2字节定长编码

  - UTF-8 1~6字节变长编码



### UTF-8编码中的中文占几个字节；int型几个字节？

  - 常用中文字符占用3个字节（大约2万多字），但超大字符集中的更大多数汉字要占4个字节

  - 英文一个字节



### 静态代理和动态代理的区别，什么场景使用？

  - 静态代理：由程序员创建或工具生成代理类的源码，再编译代理类。所谓静态也就是在程序运行前就已经存在代理类的字节码文件，代理类和委托类的关系在运行前就确定了。装饰器模式。

    - 优点：业务类只需要关注业务逻辑本身，保证了业务类的重用性。这是代理的共有优点。
    - 缺点：
      - 代理对象的一个接口只服务于一种类型的对象，如果要代理的方法很多，势必要为每一种方法都进行代理，静态代理在程序规模稍大时就无法胜任了。
      - 如果接口增加一个方法，除了所有实现类需要实现这个方法外，所有代理类也需要实现此方法。增加了代码维护的复杂度。

  - 动态代理（通过反射实现）：

    在静态代理模式下，Proxy所做的事情，无非是调用在不同的request时，调用触发realSubject对应的方法；更抽象点看，Proxy所作的事情；在Java中 方法（Method）也是作为一个对象来看待了，动态代理工作的基本模式就是将自己的方法功能的实现交给 InvocationHandler角色，外界对Proxy角色中的每一个方法的调用，Proxy角色都会交给InvocationHandler来处理，而InvocationHandler则调用具体对象角色的方法。

    

    ![img](https:////upload-images.jianshu.io/upload_images/10844298-391c3b9d11ab2ed1..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/750/format/webp)

    image

    具体步骤是：

    - 实现InvocationHandler接口创建自己的调用处理器
    - 给Proxy类提供ClassLoader和代理接口类型数组创建动态代理类
    - 以调用处理器类型为参数，利用反射机制得到动态代理类的构造函数
    - 以调用处理器对象为参数，利用动态代理类的构造函数创建动态代理类对象

      ```
      public class MyInvocationHandler implements InvocationHandler {
          private String TAG = "MyInvocationHandler";
          private Object obj;
      
          public Object bind(Object obj) {
              this.obj = obj;
              return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), this);
          }
      
          @Override
          public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
              Log.d(TAG, "start" + method.getName());
              Object result = method.invoke(obj, args);
              Log.d(TAG, "end" + method.getName());
              return result;
          }
      }
      
      Human human = new HumanImp();
      ((TextView)findViewById(R.id.tv_content)).setText(((Human)new MyInvocationHandler().bind(human)).eat());
      ```
      

    动态代理优点 ：

    - 动态代理类的字节码在程序运行时由Java反射机制动态生成，无需程序员手工编写它的源代码。

    - 动态代理类不仅简化了编程工作，而且提高了软件系统的可扩展性，因为Java 反射机制可以生成任意类型的动态代理类。

    缺点：

    JDK的动态代理机制只能代理实现了接口的类，而不能实现接口的类就不能实现JDK的动态代理，cglib是针对类来实现代理的，他的原理是对指定的目标类生成一个子类，并覆盖其中方法实现增强，但因为采用的是继承，所以不能对final修饰的类进行代理。   

    作用：

    - 方法增强，让你可以在不修改源码的情况下，增强一些方法，比如添加日志等。
    - 以用作远程调用，好多rpc框架就是用代理方式实现的。

  [参考](http://blog.csdn.net/fengyuzhengfan/article/details/49586277)



### Java的异常体系

  ![img](https:////upload-images.jianshu.io/upload_images/10844298-1908125aa409879e..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/838/format/webp)

  image

  - Error是程序无法处理的错误，比如OutOfMemoryError、ThreadDeath等。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止。
  - Exception是程序本身可以处理的异常，这种异常分两大类运行时异常（不检查异常）和非运行时异常（检查异常，不处理，程序就不能编译通过）。程序中应当尽可能去处理这些异常。
  - try、catch、finally:
    - try、catch、finally三个语句块均不能单独使用，三者可以组成 try...catch...finally、try...catch、
      try...finally三种结构，catch语句可以有一个或多个，finally语句最多一个。
    - try、catch、finally三个代码块中变量的作用域为代码块内部，分别独立而不能相互访问。
      如果要在三个块中都可以访问，则需要将变量定义到这些块的外面。
    - 多个catch块时候，只会匹配其中一个异常类并执行catch块代码，而不会再执行别的catch块，
      并且匹配catch语句的顺序是由上到下。

  - throw: 方法体内部
  - throws: 方法体外部



### 谈谈你对解析与分派的认识。

  - 调用目标在编译器进行编译时就必须确定下来，这类方法的调用称为解析
  - 解析调用一定是个静态过程，在编译期间就完全确定，在类加载的解析阶段就会把涉及的符号引用转化为可确定的直接引用，不会延迟到运行期再去完成。而分派调用则可能是静态的也可能是动态的，根据分派依据的宗量数（方法的调用者和方法的参数统称为方法的宗量）又可分为单分派和多分派。两类分派方式两两组合便构成了**静态单分派、静态多分派、动态单分派、动态多分派**四种分派情况。
    - 静态分派
      所有依赖静态类型来定位方法执行版本的分派动作，都称为静态分派，静态分派的最典型应用就是多态性中的方法重载（方法相同，参数不同）。静态分派发生在编译阶段，因此确定静态分配的动作实际上不是由虚拟机来执行的。
    - 动态分派
      动态分派与多态性的另一个重要体现——方法覆写（完全相同方法）有着很紧密的关系。向上转型后调用子类覆写的方法便是一个很好地说明动态分派的例子。这种情况很常见，因此这里不再用示例程序进行分析。很显然，在判断执行父类中的方法还是子类中覆盖的方法时，如果用静态类型来判断，那么无论怎么进行向上转型，都只会调用父类中的方法，但实际情况是，根据对父类实例化的子类的不同，调用的是不同子类中覆写的方法。很明显，这里是要根据变量的实际类型来分派方法的执行版本的。而实际类型的确定需要在程序运行时才能确定下来，这种在运行期根据实际类型确定方法执行版本的分派过程称为动态分派。

  [参考文章](https://www.jianshu.com/p/1976b01c07d2)


### 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？

  继承于object 的equals方法。



### Java中实现多态的机制是什么？

  - 继承、重写、向上转型

  - 实现多态形式：继承和接口

    - 使用父类类型的引用指向子类的对象

    - 该引用只能调用父类中定义的方法和变量

    - 如果子类中重写了父类中的一个方法，那么在调用这个方法的时候，将会调用子类中的这个方法；（动态连接、动态调用）



### 如何将一个Java对象序列化到文件里？

  - 对象实现Serializable，不希望被序列化的属性加transient属性
  - ObjectOutputStream & ObjectInputStream

  ```
        File aFile = new File("e:\\c.txt");
          Stu a = new Stu(1, "aa", "1");
          FileOutputStream fileOutputStream = null;
          try {
              fileOutputStream = new FileOutputStream(aFile);
              ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
              objectOutputStream.writeObject(a);
              objectOutputStream.flush();
              objectOutputStream.close();
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              if (fileOutputStream != null) {
                  try {
                      fileOutputStream.close();
                  } catch (IOException e) {
                      // TODO Auto-generated catch block
                      e.printStackTrace();
                  }
              }
          }
          FileInputStream fileInputStream = new FileInputStream(aFile);
          ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
          Stu s = (Stu) objectInputStream.readObject();
          System.out.println(s);
  ```



### 说说你对Java反射的理解

  - 允许当程序运行时改变程序结构或变量类型
  - 在 Java 和 Android 开发中，一般情况下下面几种场景会用到反射机制：
    - 需要访问隐藏属性或者调用方法改变程序原来的逻辑，这个在开发中是很常见的，由于一些原因，系统并没有开放一些接口出来，这个时候利用反射是一个有效的解决办法。
    - 自定义注解，注解就是在运行时利用反射机制来获取的。
    - 在开发中动态加载类，比如在 Android 中的动态加载解决65k问题等等，模块化和插件化都离不开反射。

  [参考文章](https://www.jianshu.com/p/6277c1f9f48d)



### 说说你对Java注解的理解

  注解本身不能对代码运行产生影响，但是注解可以作为一个标记，用反射之类的手段获取到这个标记后，就能对标记的内容进行处理。例如在方法参数上加注解，用反射获取到注解后对该注解标记的形式参数注入实际参数。

  - 自定义注解：

    ```
    @Retention(RetentionPolicy.RUNTIME)
    @Target({ElementType.FIELD})
    @Documented
    @Inherited
    public @interface Bind {
        int value() default 1;
        boolean canBeNull() default false;
    }
    ```

  - 元注解（给自定义注解使用的注解）

  - **@Rentention Rentention**
    @Rentention Rentention用来标记自定义注解的有效范围，他的取值有以下三种：
    - RetentionPolicy.SOURCE: 只在源代码中保留 一般都是用来增加代码的理解性或者帮助代码检查之类的，比如我们的Override;
    - RetentionPolicy.CLASS: 默认的选择，能把注解保留到编译后的字节码class文件中，仅仅到字节码文件中，运行时是无法得到的;
    - RetentionPolicy.RUNTIME:注解不仅能保留到class字节码文件中，还能在运行通过反射获取到，这也是我们最常用的。
  - **@Target**
    - @Target指定Annotation用于修饰哪些程序元素。
    - @Target也包含一个名为”value“的成员变量，该value成员变量类型为ElementType[ ]，ElementType为枚举类型，值有如下几个：
      - ElementType.TYPE：能修饰类、接口或枚举类型
      - ElementType.FIELD：能修饰成员变量
      - ElementType.METHOD：能修饰方法
      - ElementType.PARAMETER：能修饰参数  
      - ElementType.CONSTRUCTOR：能修饰构造器  
      -  ElementType.LOCAL_VARIABLE：能修饰局部变量
      - ElementType.ANNOTATION_TYPE：能修饰注解
      - ElementType.PACKAGE：能修饰包

  - **@Documented**
    使用了@Documented的可以在javadoc中找到

  - **@Interited**
    注解里的内容可以被子类继承，比如父类中某个成员使用了上述@Bind(value)，Bind中的value能给子类使用到。

  - Android库中使用注解：
    - 编译时动态处理，动态**生成代码**，如Butter Knife、Dagger 2
    - 运行时动态处理，获得注解信息，如Retrofit



### 说说你对依赖注入的理解

  - 像这种非自己主动初始化依赖，而通过外部来传入依赖的方式，我们就称为依赖注入。
  - 依赖注入的实现有多种途径，而在 Java 中，使用注解是最常用的。比如通过ButterKnife、Dagger依赖注入库实现，都是使用注解来实现依赖注入，但它利用 APT(Annotation Process Tool) 在编译时生成辅助类，这些类继承特定父类或实现特定接口，程序在运行时加载这些辅助类，调用相应接口完成依赖生成和注入。

  [反射、注解与依赖注入总结](https://www.jianshu.com/p/24820bf3df5c)



### 说一下泛型原理，并举例说明

  - 实现原理：类型擦除 先检查，再编译
  - 出现原因：解决类型转换问题
  - Java 伪泛型，编译期间会擦除泛型，只留原始类型，生成的Java字节码中是不包含泛型中的类型信息，所以可以通过反射添加原始泛型不允许的参数
  - 若无限定用Object替换，若有限定用第一个边界的类型变量来替换。

  [参考文章](https://link.jianshu.com?t=http%3A%2F%2Fblog.csdn.net%2Fwisgood%2Farticle%2Fdetails%2F11762427)



### Java中String的了解

  - final类，不可继承。

  - String对象一旦被创建就是固定不变的了，对String对象的任何改变都不影响到原对象，相关的任何change操作都会生成新的对象。

  - 优化：字符串常量池，已存在直接返回，故常量池中一定不存在两个相同的字符串。



### String为什么要设计成不可变的？

  - 设计考虑，效率优化问题，以及安全性这三大方面，比如线程问题：不可变量总是线程安全的。



### Object类的equal和hashCode方法重写，为什么？

  两个Object equals时，其hashcode必相等



# 三、线程、多线程和线程池

### 开启线程的三种方式？

  - 继承Thread类创建线程类

    ```
    public class MyThread extends Thread {
        @Override
        public void run() {
            super.run();
        }
    }
    
    new MyThread().start();
    ```

  - 通过Runnable接口创建线程类

    ```
    new Thread(new Runnable() {
        @Override
        public void run() { }
    }).start();
    ```

  - 通过Callable和Future创建线程

    ```
    new Thread(new FutureTask<String>(new Callable<String>() {
        @Override
        public String call() throws Exception { }
    })).start();
    ```



### 线程和进程的区别？

  参上



### 为什么要有线程，而不是仅仅用进程？

  - 系统多个进程，等同多个应用，进程间异步；如果一个应用一个进程，应用会是单线程应用

  - 线程可认为是进程的细化版本

  - 进程切换的开销要大，而线程切换的开销较小。



### run()和start()方法区别

  - run()是直接调用线程对象的run()方法，同步

  - start()是开启一个新的线程，异步



### 如何控制某个方法允许并发访问线程的个数？

  - 想控制允许访问线程的个数就要使用到Semaphore。
  - Semaphore有两个方法semaphore.acquire() 和semaphore.release()。
    - semaphore.acquire() ：请求一个信号量，这时候的信号量个数-1（一旦没有可使用的信号量，也即信号量个数变为负数时，再次请求的时候就会阻塞，直到其他线程释放了信号量）。
    - semaphore.release() 释放一个信号量，此时信号量个数+1。



### 在Java中wait和sleep方法的不同；

  - wait只能在同步（synchronize）环境中被调用，而sleep不需要。

  - 进入wait状态的线程能够被notify和notifyAll线程唤醒，但是进入sleeping状态的线程不能被notify方法唤醒。

  - wait通常有条件地执行，线程会一直处于wait状态，直到某个条件变为真。但是sleep仅仅让你的线程进入睡眠状态。

  - wait方法在进入wait状态的时候会释放对象的锁，但是sleep方法不会。

  - wait方法是针对一个被同步代码块加锁的对象，而sleep是针对一个线程。



### 谈谈wait/notify关键字的理解

  - wait()方法可以使处于临界区内的线程进入等待状态，同时释放被同步对象的锁

  - notify()方法可以唤醒一个因调用wait操作而处于阻塞状态中的线程，使其进入就绪状态。被重新唤醒的线程会视图重新获得临界区的控制权也就是锁，并继续执行wait方法之后的代码。如果发出notify操作时没有处于阻塞状态中的线程，那么该命令会被忽略



### 什么导致线程阻塞？

  - 线程睡眠：Thread.sleep (long millis)方法，使线程转到阻塞状态。millis参数设定睡眠的时间，以毫秒为单位。当睡眠结束后，就转为就绪（Runnable）状态。sleep()平台移植性好。

  - 线程等待：Object类中的wait()方法，导致当前的线程等待，直到其他线程调用此对象的 notify() 唤醒方法。这个两个唤醒方法也是Object类中的方法，行为等价于调用 wait() 一样。wait() 和 notify() 方法：两个方法配套使用，wait() 使得线程进入阻塞状态，它有两种形式，一种允许 指定以毫秒为单位的一段时间作为参数，另一种没有参数，前者当对应的 notify() 被调用或者超出指定时间时线程重新进入可执行状态，后者则必须对应的 notify() 被调用.

  - 线程礼让，Thread.yield() 方法，暂停当前正在执行的线程对象，把执行机会让给相同或者更高优先级的线程。yield() 使得线程放弃当前分得的 CPU 时间，但是不使线程阻塞，即线程仍处于可执行状态，随时可能再次分得 CPU 时间。调用 yield() 的效果等价于调度程序认为该线程已执行了足够的时间从而转到另一个线程.

  - 线程自闭，join()方法，等待其他线程终止。在当前线程中调用另一个线程的join()方法，则当前线程转入阻塞状态，直到另一个进程运行结束，当前线程再由阻塞转为就绪状态。

  - suspend() 和 resume() 方法：两个方法配套使用，suspend()使得线程进入阻塞状态，并且不会自动恢复，必须其对应的resume() 被调用，才能使得线程重新进入可执行状态。典型地，suspend() 和 resume() 被用在等待另一个线程产生的结果的情形：测试发现结果还没有产生后，让线程阻塞，另一个线程产生了结果后，调用 resume() 使其恢复。Thread中suspend()和resume()两个方法在JDK1.5中已经废除，不再介绍。因为有死锁倾向。



### 线程如何关闭？

  - 线程正常执行完毕，正常结束
  - 中断标志位方式（interrupt、自定义标志位）终止线程
    - interrupt()中断
    - while循环检查isInterrupted()，若为true退出循环，线程结束



### 数据一致性如何保证？

  加锁、原子性、事务



### 如何保证线程安全？

  - 不使用多线程

  - 不可变量

  - 加锁同步机制



### 如何实现线程同步？

- 讲一下java中的同步的方法

  - 使用synchronized关键字

    Java中的每个对象都有一个monitor对象，当使用synchronized关键字来同步一段代码时，java提供monitorenter和monitorexit两个字节码指令来支持同步操作，

    - monitorenter

      每一个对象都关联一个monitor对象，线程执行monitorenter时获取该对象所关联的monitor的拥有权。如果另一个线程已经占有了该对象所关联的monitor，当前线程阻塞进入等待队列直到该对象被解锁，然后尝试获取拥有权。如果当前线程已经拥有该对象所关联的monitor，当前线程增加monitor的表示线程进入次数的计数器。如果该对象所关联的monitor没有被任何线程拥有，当前线程变成该monitor的拥有者，设置monitor的计数器（entry count）为1.

    - monitorexit
      当前线程必须为该对象的monitor的拥有者，将表示该线程进入monitor次数的计数器减1。如果计数器的值变为0，当前线程释放monitor。如果对象关联的monitor变为自由状态，其他等待的线程才能获取monitor。

    - 作用于成员方法和static方法的synchronized分别锁住this对象和类的class对象。

      

  - 使用java.util.concurrent.locks包中的方法来实现线程同步

    - 这个包中的对象使用硬件的CAS（Compare-And-Swap）原子语义来实现同步。大致的行为是，如果该变量的值为期望的值时将其改为指定的值，否则（表示被并行的其他线程修改了）什么也不做。而这些操作是在direct memory中进行的而不是heap中，查看源代码会发现使用了sun.misc.Unsafe类来操作direct memory，这个类是没有公开的，但是网上有很多介绍使用这个类来实现高性能库的方法，可以搜一下啦。

    - 这个包中主要有三个接口Condition，Lock，ReadWriteLock。 Lock锁的语义和synchronized一致，但是增加了非阻塞式加锁tryLock()，获取可以被中断的锁，以及timeout式地获取锁。
      使用Lock的代码样例为：

      ```
      Lock l = new ReentrantLock();
      l.lock();
      try {
          // Access the resource protected by this lock
      } finally {
          l.unlock();
      }
      ```

    - ReadWriteLock管理一对Lock,分别进行读和写操作的加锁。便于实现读写同步。

    - Condition可以通过lock.newCondition()方法来获取，它拥有的方法有await(原子地释放拥有的锁然后挂起当前线程),signal,signalAll, 实现的语义如同Object.wait, Object.notify以及Object.notifyAll方法，但是通过Condition可以在一个对象上实现多套wait集(wait-sets)。在Condition的javadoc中演示了用其实现生产者-消费者模式的示例。



### 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？

  能，读写锁



### 线程间操作List

  - 使用线程安全的集合类，如Vector，它的所有方法都是同步的，几乎不需要自己加同步代码。

  - 对于非线程安全的集合类，可以用synchronized自己做同步处理

  - 对于非线程安全的集合类，可以使用同步包装方法对集合进行包装，包装后的对象（下例的list）是线程安全的。

    List list = Collections.synchronizedList(new ArrayList());



### Java中对象的生命周期

  - 创建阶段(Created)
  - 应用阶段(In Use)
  - 不可见阶段(Invisible)
  - 不可达阶段(Unreachable)
  - 收集阶段(Collected)
  - 终结阶段(Finalized)
  - 对象空间重分配阶段(De-allocated)



### synchronized用法
### synchronized的原理

  - Java中的每个对象都有一个monitor对象，当使用synchronized关键字来同步一段代码时，java提供monitorenter和monitorexit两个字节码指令来支持同步操作，

    - monitorenter

      每一个对象都关联一个monitor对象，线程执行monitorenter时获取该对象所关联的monitor的拥有权。如果另一个线程已经占有了该对象所关联的monitor，当前线程阻塞进入等待队列直到该对象被解锁，然后尝试获取拥有权。如果当前线程已经拥有该对象所关联的monitor，当前线程增加monitor的表示线程进入次数的计数器。如果该对象所关联的monitor没有被任何线程拥有，当前线程变成该monitor的拥有者，设置monitor的计数器（entry count）为1.

    - monitorexit
      当前线程必须为该对象的monitor的拥有者，将表示该线程进入monitor次数的计数器减1。如果计数器的值变为0，当前线程释放monitor。如果对象关联的monitor变为自由状态，其他等待的线程才能获取monitor。
      作用于成员方法和static方法的synchronized分别锁住this对象和类的class对象。

    - 作用于成员方法和static方法的synchronized分别锁住this对象和类的class对象。



### 谈谈对synchronized关键字，类锁，方法锁，重入锁的理解

  - synchronized关键字

    - 某个线程在进行unlock M操作前进行的所有写入操作对进行lock M操作的线程都是可见的。
    - 只要用synchronized保护会被多个线程读写的共享字段，就可以避免这些共享字段受到重排序和可见性的影响

  - 锁对象只有两种

    - 类锁，每个类只有一个class对象
    - 对象锁，这里的对象指类的实例，有时我们也称实例为对象，区别于class对象

  - 类锁

    - synchronized(ClassName.class){}：作用范围是代码块{}，锁对象是所属类的class对象
    - synchronized静态方法：作用范围是该静态方法，锁对象是所属类的class对象。

  - 对象锁（方法锁也是一种对象锁）

    - synchronized(obj){}：作用范围是代码块{}，锁对象是obj实例
    - synchronized方法：作用范围是该方法，锁对象是调用这个方法的obj实例。

  - 重入锁

    - 通俗说：自己可以再次获得自己内部锁。比如有1条线程获得了某对象的锁，此时这个对象锁还没有释放，当其再次想获得这个对象的锁的时候还是可以获取的，如果锁不可重入，就会造成死锁。

    - 关键字synchronized拥有锁重入的功能，也就是在使用synchronized是当一个线程得到一个对象锁后，再次请求此对象锁时是可以再次得到该对象的锁的。这也证明在一个synchronized方法/块的内部掉用本类的其他synchronized方法/块是永远可以得到锁的。



### static synchronized 方法的多线程访问和作用

  属于类锁，作用范围是该静态方法，锁对象是所属类的类对象。



### 同一个类里面两个synchronized方法，两个线程同时访问的问题

  - 两个synchronized方法的锁对象相同，都是该实例。

  - 两个线程同时访问该实例的 func1()和func2()，将同步执行，因为竞争的是同一把锁。

    - 线程a执行，线程b阻塞等待
    - 线程a执行完释放锁，线程b竞争到锁继续执行



### volatile的原理



### 谈谈volatile关键字的用法



### 谈谈volatile关键字的作用

  - 可见性

    - Java中的happen-before规则：某个线程对volatile字段进行的写操作的结果对其他线程立即可见。换言之，对volatile字段的写入并不会被缓存起来。
    - 向volatile字段写入的值如果对线程B可见，那么之前写入的所有值就都是可见的。

  - 有序性

    - 防止指令重排序



### 谈谈NIO的理解

  Java NIO是一种新式的IO标准，与之间的普通IO的工作方式不同。标准的IO基于字节流和字符流进行操作的，而NIO是基于通道(Channel)和缓冲区(Buffer)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入通道也类似。



### synchronized和volatile 关键字的区别
### synchronized保证了什么特性，volatile保证了什么特性

  - synchronized ：保证了可见性、原子性、有序性

  - volatile：保证了可见性、防止指令重排序



### synchronized与Lock的区别

  - synchronized

    - 基于JVM底层实现的数据同步，又称为对象监视器（object）
    - 优点：实现简单，语义清晰，便于JVM堆栈跟踪，加锁解锁过程由JVM自动控制，提供了多种优化方案，使用更广泛
    - 缺点：悲观的排他锁，不能进行高级功能

  - Lock

    - Lock是纯Java手写的，与底层的JVM无关。在java.util.concurrent.locks包中有很多Lock的实现类，常用的有ReenTrantLock、ReadWriteLock(实现类有ReenTrantReadWriteLock)，其实现都依赖java.util.concurrent.AbstractQueuedSynchronizer类（简称AQS）
    - 优点：可定时的、可轮询的与可中断的锁获取操作，提供了读写锁、公平锁和非公平锁
    - 缺点：需手动释放锁unlock，不适合JVM进行堆栈跟踪

  - 都是可重入锁



### synchronized和Lock真正锁的是什么

  - synchronized：锁的是类的class对象，或类的实例
  - Lock：锁的是



### ReentrantLock 、synchronized 和volatile比较



### ReentrantLock的内部实现



### Lock原理



### 死锁的四个必要条件？

  - 互斥条件：一个资源每次只能被一个进程使用。
  - 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
  - 不剥夺条件：进程已获得的资源，在末使用完之前，不能强行剥夺。
  - 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。

  这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之
  一不满足，就不会发生死锁。



### 怎么避免死锁？

  理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和
  解除死锁。所以，在系统设计、进程调度等方面注意如何不让这四个必要条件成立，如何确
  定资源的合理分配算法，避免进程永久占据系统资源。此外，也要防止进程在处于等待状态
  的情况下占用资源。因此，对资源的分配要给予合理的规划。



### 对象锁和类锁是否会互相影响？

  类锁和对象锁不是同一个锁，一个是类的class对象的锁，一个是类的实例的锁。也就是线程a访问类静态synchronized方法时，允许另一个线程b访问类的实例的synchronized方法。反过来也是成立的，因为他们的锁是不同的。



### 什么是线程池，如何使用?



### Java的并发、多线程、线程模型



### 谈谈对多线程的理解



### 可见性，原子性，有序性的理解



### 多线程有什么要注意的问题？



### 谈谈你对并发编程的理解并举例说明



### 谈谈你对多线程同步机制的理解？



### 如何保证多线程读写文件的安全？



### 多线程断点续传原理



### 断点续传的实现

# 四、并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）：

平时Android开发中对并发编程可以做得比较少，Thread这个类经常会用到，但是我们想提升自己的话，一定不能停留在表面，,我们也应该去了解一下java的关于线程相关的源码级别的东西。
学习的参考资料如下：


### Java 内存模型

  - 重排序

    所谓重排序，英文记作Recorder，是指编译器和Java虚拟机通过改变程序的处理顺序来优化程序。

  - 可见性

    - 在Java内存模型中，线程A写入的值并不一定会立即对线程B可见。
      普通操作是通过缓存（CPU的缓存，寄存器，Java虚拟机临时保存的变量）来执行的，比如线程A读内存到缓存，修改缓存并没有回写内存，线程B读内存到缓存，此时读取到的值不是最新值。也就是普通读读取到的值并不一定是最新的值，通过普遍写入的值也不一定会立即对其他线程可见。

  - synchronized

    - 某个线程在进行unlock M操作前进行的所有写入操作对进行lock M操作的线程都是可见的。
    - 只要用synchronized保护会被多个线程读写的共享字段，就可以避免这些共享字段受到重排序和可见性的影响

  - volatile

    - Java中的happen-before规则：某个线程对volatile字段进行的写操作的结果对其他线程立即可见。换言之，对volatile字段的写入并不会被缓存起来。
    - 向volatile字段写入的值如果对线程B可见，那么之前写入的所有值就都是可见的。

  - final

    当final字段的初始化结束后，无论在任何时候，它的值对其他线程都是可见的



### Java线程安全总结
  深入理解java内存模型系列文章



### 一张图让你看懂JAVA线程间的状态转换

  ![](https://pic4.zhimg.com/80/v2-287f87ad5328f2aa5cd7fbd48dadcd8f_hd.jpg)



### 锁：



### 锁机制：synchronized、Lock、Condition
  Java 中的锁



### 并发编程：
- Java并发编程：Thread类的使用
- Java多线程编程总结
- Java并发编程的总结与思考
- Java并发编程实战-----synchronized
- 深入分析ConcurrentHashMap