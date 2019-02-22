# Android 面试题及答案 - Android

[![License](https://img.shields.io/badge/license-Apache%202-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)

> Android面试题包括Android基础，还有一些源码级别的、原理这些等。所以想去大公司面试，一定要多看看源码和实现方式，常用框架可以试试自己能不能手写实现一下，锻炼一下自己。


（一）Android基础知识点

* 四大组件是什么

  - Activity
    其中Activity是我们最常使用的组件，也是我们最熟悉的组件，一般用于呈现页面内容，处理用户交互等等
  - Service
    Service是一个可以运行于后台的组件，我们一般用于处理一些不需要用户知道，但是又必须比较长时间存在的操作，比如下载
  - ContentProvider
    主要用于进程间通信，比如暴露某个APP的信息内存给予另外一个APP获取使用，比如获取联系人等等
  - BroadcastReceiver
    广播接受者主要用于接受广播信息，像我们日常使用中可能用于两种情况：
    - APP内：界面间通信，例如退出app，可以发送自杀广播
    - APP间：可以收到第三方发出的广播，进而进行对应的响应操作。

  

* 四大组件的生命周期和简单用法

  - Activity

    - 生命周期

      oncreate()->onstart()->onresume()->onpause()->onstop()->ondestory()

    - 简单用法

      startActivity

  - Service

    - 生命周期
      - startService()的生命周期:
        oncreate()->onstartComment()->onstart()->onDestory()
      - bindService()的生命周期：
        oncreate()->onbind()->onUnbind()->onDestroy()

    - 简单用法 
      - startService()
        通过简单的startService()进行service启动，此后启动该Service的组件无法把控Service的生命周期，理论上此后该Service可以在后台无期限运行（实际根据情况该Service可能会在任意一个时刻被杀死，这里牵连到了另外一个知识点：**Service防杀**）
        我们可以在onStartCommand()里面做我们要做的操作，注意Service跟Activity一样不可以做耗时操作，虽然运行anr时间比Activity多了近一倍。
      - bindService()
        通过绑定的方式启动Service
        绑定后，该Service与启动绑定操作的组件形成绑定，当组件销毁时，该Service也随着销毁。
        其中组件与Service形成一个典型的BC体系，Service相当于服务器，组件可以通过**IBinder**像Service       发送请求并获取回应。

  - BroadcastReceiver（分为2种）:

    - 简单用法 

      - 静态注册（常驻广播）
        在AndroidManifest.xml中进行注册,App启动的时候自动注册到系统中，不受任何组件生命周期影响，（即便应用程序已经关闭），但是 *耗电*，*占内存* 
      - 动态注册（非常驻广播）
        在代码中进行注册,通过**IntentFilter**意图过滤器筛选需要监听的广播,
        记得**注销**（推荐在onResume()注册，在onPause()注销），使用灵活，生命周期随组件变化

    - 全局广播

      - 普通广播（最常用的那种)
      - 系统广播
        系统广播无须开发者进行发送，我们只需做好广播接收器进行接收即可，常用的几个系统广播为：
        - android.net.conn.CONNECTIVITY_CHANGE 监听网络变化
        - Intent.ACTION_PACKAGE_ADDED 成功安装apk
          ......
      - 有序广播
        有序广播是指广播按照一定的优先级被广播接受者依次接收，代码实例
        **发送广播**
        **定义2个广播接受者**
        **动态注册2个广播接受者，设置不同的优先级**
        **高优先级的广播接受者接受信息，篡改信息，塞回篡改后的信息**
        **低优先级广播接收源信息，接收上一个广播信息，分别打印出来**

    - 本地广播

      上面所讲的广播都是全局广播，全局广播指的是，我在甲App发送了一条广播，其中已App也可以收到这个广播信息，存在问题为：

      - 其他app可以发送相对应的 intent-filter到我方App,导致我方错误处理
      - 其他app可以设置对应的 intent-filter接收我方App发出的广播，导致安全性问题

      为解决这个问题，我们可以使用本地广播：

      - 方式一： 
        - 设置exported为false(默认为true),使该广播只在app内传递
        - 广播传递和接收时，添加响应的permission
        - 通过**intent.setPackage(com.xxx.xxx)**
      - 方式二 LocalBroadcastManager：

  - ContentProvider

    - contentProvider 内容提供者

    - contentresolver 内容解析器

      

* Activity之间的通信方式

  - Intent

    在startActivity()或者startActivityForResult()时，通过Intent携带需要的信息，但要注意，intent对携带写信的大小有限制

  - 广播Broadcast或者LocalBroadcast

    在A中发出广播，在B中接收广播并解析其中数据

  - 用数据存储的方式

    理论上凡是数据存储的方式，我们均能在A存储信息，并在B读取，达到通信的目的，具体方式如SharedPreference/SQLite/File/Android剪切板等

  - 使用静态变量

    在A中将静态变量赋值，在B中读取并置空

  - Service(IPC模型)，也就是上文的bingService()

  - 通过EventBus/rxBus 进行通信

    

* Activity各种情况下的生命周期

  - 正常情况下是：onCreate()->onStart()->onResume()->onPause()->onStop()->onDestory()

  - 被dialog遮罩情况下： 
    - 如果是自身的dialog那么不会走自己的生命周期
    - 如果是其他组件传递的dialog，那么会走onpause()

  - 可以看到我们前面有一个onRestart()的生命周期，这个生命周期我们不常用，具体是什么时机会被调用呢： 
    - home键，然后切换回来
    - 跳转到另外一个activity，然后back键
    - 从本应用跳转到另外一个应用，然后回来

  

  - 基本的生命周期
    - --`onCreate()` (Activity创建时调用 )
    -  --`onStart()`(可见未获取焦点，无法与之交互 )
    -  --`onResume()`(可见已获取焦点，可与之交互 )
    -  --`onPause()`(可见，失去焦点 )
    -  --`onStop()`(不可见 )
    -  --`onRestart()`(Activity重启)
    -  --`onDestroy()`(Activity销毁)
    - --`onSaceInstanceState`可能会被回收的时候调用,与上面的先后顺序各个Android版本不同
    -  --`onResotreInstanceState`没有被回收的话就不会调用
    -  --`onConfigurationChanged`

  

  - 各种情况下的生命周期
    - 正常通过startActivity或者桌面快捷方式直接启动直到可见可正常交互
       `onCreate()`--`onStart()`--`onResume()`
    - 正常销毁,(在onResume之后),执行`finish()`方法或者点击回退按钮
       `onPause()`--`onStop()`--`onDestroy()`
    - 在`onCreate()`里调用`finish()`
       `onCreate`--`onDestroy()`
    - 在`onStart()`里调用`finish()`
       `onCreate`--`onStart`--`onStop`--`onDestroy`
    - 正常启动后按下Home键(API4.4`onSaveInstanceState`在`onPause`之后)
       `onPause`--`onSaveInstanceState`--`onStop`
    - 按下Home键没回收之前再点开
       `onRestart`--`onStart`--`onResume`
    - 出现Dialog
       不调用任何生命周期
    - 出现Dialog主题的Activity
       `onPause`--`onSaveInstanceState`
    -  `去掉dialog`
       `onResume`
    - 在`manifest`文件设置`android:configChanges`属性`orientation|keyboard|screenSize`
       切换横/竖屏
       `onConfigurationChanged`
    - ..在`manifest`文件设置`android:configChanges`属性`orientation|keyboard`
       在4.4以上横屏
       `onPause`--`onSaveInstanceState`--`onStop`--`onCreate()`--`onStart()`--`onResume()`
       4.4以下(不一定,跟是否改变了屏幕大小有关)横屏
       `onConfigurationChanged`
       竖屏4.4以下
       `onConfigurationChanged`
       竖屏4.4以上
       `onPause`--`onSaveInstanceState`--`onStop`--`onCreate()`--`onStart()`--`onResume()`
    - ..在`manifest`文件设置`android:configChanges`属性`orientation`
       切横屏
       `onPause`--`onSaveInstanceState`--`onStop`--`onCreate()`--`onStart()`--`onResume()`
       切竖屏4.4以上一次2.3以下2次
       `onPause`--`onSaveInstanceState`--`onStop`--`onCreate()`--`onStart()`--`onResume()`
       `onPause`--`onSaveInstanceState`--`onStop`--`onCreate()`--`onStart()`--`onResume()`
    - 设置launchMode的情况下,如果只是把Acitvity调回到前台
       会执行`onNewIntent()`

  

* 横竖屏切换的时候，Activity 各种情况下的生命周期

  - 不做任何配置，生命周期重新走一遍：完整流程为：onCreate()->onStart()->onResume()->onPause()->onSaveInstanceState()->onStop()->Ondestory()->onCreate()->onStart()->onRestoreInstanceState()->onResume()。
  - 做配置：**android:configChanges="orientation|screenSize"**
    横竖屏不走其他生命周期
  - 其中onSaveInstanceState()可以保存用户数据，对应的onRestoreInstanceState()可以读取之前保存的用户数据

  

* Activity与Fragment之间生命周期比较

  - activity有7个生命周期，fragment有十一个生命周期

  - fragment以及activity生命周期

    其中他们的关系是：

    - 创建的时候：*Activity* 带动 *Fragment* 走生命周期，表格关系为：

    | 所属     | Activity | Fragment                                         |
    | -------- | -------- | ------------------------------------------------ |
    | 创建操作 | onCreate | onAttach/onCreate/onCreateView/onActivityCreated |
    | 创建操作 | onStart  | onStart()                                        |

    - 销毁的时候：*Fragment* 带动 *Activity* 走生命周期，表格关系为：

    | 所属     | Fragment                               | Activity    |
    | -------- | -------------------------------------- | ----------- |
    | 创建操作 | onPause()                              | onPause()   |
    | 创建操作 | onStop()                               | onStop()    |
    | 创建操作 | onDestroyView()/onDestroy()/onDetach() | onDestroy() |

  

* Activity上有Dialog的时候按Home键时的生命周期

  Dialog并不会引起Activity的生命周期变化，所以在弹框的情况下，点击home键，其Activity的生命周期与正常时一样

  

* 两个Activity 之间跳转时必然会执行的是哪几个方法？

  - 一般情况下比如说有两个activity,分别叫A,B。
  - 当在A 里面激活B 组件的时候, A会调用onPause()方法,然后B调用onCreate() ,onStart(), onResume()。
  - 这个时候B覆盖了A的窗体, A会调用onStop()方法。
  - 如果B是个透明的窗口,或者是对话框的样式, 就不会调用A的onStop()方法。
  - 如果B已经存在于Activity栈中，B就不会调用onCreate()方法。

  

* 前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命值周期回调方法。

  - 前台切换到后台：onPause()->onStop()
  - 如果正常切换回来，那么会走onRestart()->onStart()->onResume()
  - 如果很久没切回来，系统内存紧急被回收了，那么回来会重新走一次生命周期
  - 弹出dialog
    - 如果dialog是自身Activity弹出来的，则不会走生命周期
    - 如果不是自身Activity弹出来的，则走onPause(),退出Dialog后走onRestart()->onResume()

  

* Activity的四种启动模式对比

  - standard

    这个是Activity的默认启动方式，我们不需要额外的配置
    在该配置下，启动一个Activity就会在该应用的Activity栈中压入一个Activity,返回的时候就直接把该Activity弹出栈。

  - singleTop

    这个是栈顶复用模式
    在该配置下，如果在Activity栈，栈顶是该Activity，那么会走onNewIntent()->onResume()
    如果不是，那么就走正常的生命周期

  - singleInstance

    这个是栈内复用模式
    在该配置下，如果在该Activity栈，栈内存在该Activity(没有要求是栈顶)，那么会走onNewIntent()->onResume()，并且把位于该Activity上方的Activity全部出栈，使该Activity位于栈顶

  - singleTask

    这个配置下，Activity独享一个Activity栈。

  

* Activity状态保存于恢复

  - onSaveInstanceState(Bundle state) 保存状态
  - onRestoreInstanceState(Bundle state) 恢复状态

  

* fragment各种情况下的生命周期

  - Fragment在[Activity](https://www.baidu.com/s?wd=Activity&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)中replace
    - 新替换的Fragment：onAttach（） > onCreate（） > onCreateView （）> onViewCreated（） > onActivityCreated （）> onStart （）> onResume（）
    - 被替换的Fragment：onPause（） > onStop（） > onDestroyView （）> onDestroy （）> onDetach（）

  - Fragment在Activity中replace，并addToBackStack
    - 新替换的Fragment（没有在BackStack中）：onAttach（） > onCreate（） > onCreateView（） > onViewCreated （）> onActivityCreated（） > onStart （）> onResume（）
    - 新替换的Fragment（已经在BackStack中）：onCreateView（） > onViewCreated （）> onActivityCreated（） > onStart（） > onResume（）
    - 被替换的Fragment：onPause（） > onStop（） > onDestroyView（）

  - Fragment进入了运行状态：

    Fragment在上述的各种情况下进入了onResume（）后，则进入了运行状态，以下4个生命周期方法将跟随所属的Activity一起被调用：onPause（） > onStop （）> onStart（） > onResume（）

  - 关于Fragment的onActivityResult方法：

    使用Fragment的startActivity（）方法时，FragmentActivity的onActivityResult（）方法会回调相应的Fragment的onActivityResult（）方法，所以在重写FragmentActivity的onActivityResult（）方法时，注意调super.onActivityResult（）。

  

* Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用？

  - startActivityForResult是FragmentActivity的方法。

  - 其实和Activity的startActivityForResult是一样的，只不过需要注意在Fragment里面调用的话，直接使用：

    ```
    val targetIntent = Intent(this@MyFragment.context, DialogActivity::class.java)
    startActivityForResult(targetIntent, 1)
    ```

    而不需要使用getActivity()/activity：

    ```
    val targetIntent = Intent(this@MyFragment.context, DialogActivity::class.java)
    activity.startActivityForResult(targetIntent, 1)
    ```

  

* 如何实现Fragment的滑动？

  - 把Fragment放到ViewPager里面去
  - 把Fragment放到RecyclerView/ListView/GridView

  

* fragment之间传递数据的方式？

  - Intent传值

  - 广播传值

  - 静态调用

  - 本地化存储传值

  - 暴露接口/方法调用

  - eventBus等之类的

    

* Activity 怎么和Service 绑定？

  - 通过BindService绑定，具体步骤

    - 新建Activity, Service。Activity里面绑定Service。

    - Service新建绑定类继承Binder，具体代码如下：
      **Service** 

        ```
        public class MyService extends Service {
            private IBinder iBinder=new MyBinder();
            
            @Nullable
            @Override
            public IBinder onBind(Intent intent) {
                return iBinder;
            }
        
            class MyBinder extends Binder {
                public MyService get() {
                    return MyService.this;
                }
            }
        }
        ```

      **Activity**

      ```
        public class ServiceActivity extends AppCompatActivity {
        
            @Override
            protected void onCreate(@Nullable Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
        
                MyConnection myConnection = new MyConnection();
                Intent intent = new Intent(this, MyService.class);
                bindService(intent, myConnection, Service.BIND_AUTO_CREATE);
            }
        
        
            private class MyConnection implements ServiceConnection {
                @Override
                public void onServiceConnected(ComponentName name, IBinder service) {
                    MyService myService = ((MyService.MyBinder) service).get();
                    Log.e("lht", "onServiceConnected: " + myService.getPackageName());
                }
        
                @Override
                public void onServiceDisconnected(ComponentName name) {
        
                }
        
                @Override
                public void onBindingDied(ComponentName name) {
        
                }
            }
        }
      ```

  

* 怎么在Activity 中启动自己对应的Service？

  Activity通过bindService(Intent service, ServiceConnection  conn, int flags)跟Service进行绑定。同上。

  

* service和activity怎么进行数据交互？

  - Binder对象

  - 广播

    

* [Service的开启方式](https://www.jianshu.com/p/4c798c91a613)

  

* [请描述一下Service 的生命周期](https://www.jianshu.com/p/4c798c91a613)

  

* [谈谈你对ContentProvider的理解](https://www.jianshu.com/p/ea8bc4aaf057)

  

* 说说ContentProvider、ContentResolver、ContentObserver 之间的关系

  - ContentProvider 内容提供者,用于对外提供数据

  - ContentResolver.notifyChange(uri)发出消息

  - ContentResolver 内容解析者, 用于获取内容提供者提供的数据

  - ContentObserver 内容监听器, 可以监听数据的改变状态

  - ContentResolver.registerContentObserver()监听消息

    

* 请描述一下广播BroadcastReceiver的理解

  - 是四大组件之一,主要用于接收app发送的广播
  - 2.内部通信实现机制:通过android系统的Binder机制.

  

* 广播的分类

  - 无序广播
    - 优点: 完全异步,逻辑上可被任何接受者收到广播,效率高
    - 缺点: 接受者不能讲处理结果交给下一个接受者,且无法终止广播.

  - 有序广播
    - 按被接收者的优先级循序传播
    - A>B>C,每个都有权终止广播,下一个就得不到
    - 每一个都可进行修改操作,下一个就得到上一个修改后的结果.

  

* 广播使用的方式和场景

  - 开机启动, sd卡挂载, 低电量, 外拨电话, 锁屏等
  - 比如根据产品经理要求, 设计播放音乐时, 锁屏是否决定暂停音乐.

  

* 在manifest 和代码中如何注册和使用BroadcastReceiver?

  在清单文件中注册广播接收者称为静态注册，在代码中注册称为动态注册。静态注册的广播接收者只要 app 在系
  统中运行则一直可以接收到广播消息，动态注册的广播接收者当注册的 Activity 或者 Service 销毁了那么就接收不到
  广播了。

  - 静态注册：在清单文件中进行如下配置 

    ```
    `<receiver android:name=``".BroadcastReceiver1"` `>``　　<intent-filter>``　　　　<action android:name=``"android.intent.action.CALL"` `></action>``　　</intent-filter>``</receiver>`
    ```

  - 动态注册：在代码中进行如下注册 

    ```
    `receiver = ``new` `BroadcastReceiver();``IntentFilter intentFilter = ``new` `IntentFilter();``intentFilter.addAction(CALL_ACTION);``context.registerReceiver(receiver, intentFilter);`
    ```

  

* 本地广播和全局广播有什么差别？

* BroadcastReceiver，LocalBroadcastReceiver 区别

  - 全局广播 BroadcastReceiver

    是针对应用间、应用与系统间、应用内部进行通信的一种方

  - 本地广播LocalBroadcastReceiver

    仅在自己的应用内发送接收广播，也就是只有自己的应用能收到，数据更加安全广播只在这个程序里，而且效率更高。

  

* AlertDialog,popupWindow,Activity区别

  - AlertDialog是非阻塞式对话框，当弹出来的时候，后台还是可以继续做事情的
  - popupWindow则是阻塞式的对话框

  

* Application 和 Activity 的 Context 对象的区别

  - Application是全局的Context,可以用于需要长期持久的contenxt操作
  - 而Activity的Context只是属于该Activity,如果被非法持有的话，很容易导致该Activity无法被回收，造成内存泄露

  

* Android属性动画特性

  - 不仅限于对View进行操作（移动、缩放、旋转和淡入淡出），具备时间差值器
  - 改变了View的属性，移动是真移动，而补间动画仅仅是改变了绘制相对位置，View的坐标位置属性并未改变，所以导致点击事件还是在原位置

  

* 如何导入外部数据库?

  - 把外部建好的数据库文件放在raw中，
  - 读入该数据库文件
  - 写入到data/data/batabases文件夹中
  - 正常读取

  

* LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。

  - FrameLayout(框架布局)

    此布局是五种布局中最简单的布局，Android中并没有对child view的摆布进行控制，这个布局中所有的控件都会默认出现在视图的左上角，我们可以使用`android:layout_margin`，`android:layout_gravity`等属性去控制子控件相对布局的位置。

  - LinearLayout(线性布局)

     一行（或一列）只控制一个控件的线性布局，所以当有很多控件需要在一个界面中列出时，可以用LinearLayout布局。 此布局有一个需要格外注意的属性:`android:orientation=“horizontal|vertical`。

     ```
     * 当`android:orientation="horizontal`时，*说明你希望将水平方向的布局交给**LinearLayout** *，其子元素的`android:layout_gravity="right|left"` 等控制水平方向的gravity值都是被忽略的，*此时**LinearLayout**中的子元素都是默认的按照水平从左向右来排*，我们可以用`android:layout_gravity="top|bottom"`等gravity值来控制垂直展示。
     * 反之，可以知道 当`android:orientation="vertical`时，**LinearLayout**对其子元素展示上的的处理方式。
     　　
     ```

  - AbsoluteLayout(绝对布局)

  ​       可以放置多个控件，并且可以自己定义控件的x,y位置

  - RelativeLayout(相对布局)

    这个布局也是相对自由的布局，Android 对该布局的child view的 水平layout& 垂直layout做了解析，由此我们可以FrameLayout的基础上使用标签或者Java代码对垂直方向 以及 水平方向 布局中的views进行任意的控制.

    相关属性：

    ```
    　　android:layout_centerInParent="true|false"
    　　android:layout_centerHorizontal="true|false"
    　　android:layout_alignParentRight="true|false"
    　　
    ```

  - TableLayout(表格布局)

    将子元素的位置分配到行或列中，一个TableLayout由许多的TableRow组成

    

* 谈谈对接口与回调的理解

  - You call me i call you

  - 通信的桥梁

  - 解耦

  

* 回调的原理

  传递通信信使listener，双方持有，通过listener接口实现相互跳转回调。

  

* 写一个回调demo

  

* 介绍下SurfView

  - View适用于主动更新的情况，而SurfaceView则适用于被动更新的情况，比如频繁刷新界面。

  - View在主线程中对页面进行刷新，而SurfaceView则开启一个子线程来对页面进行刷新。

  - View在绘图时没有实现双缓冲机制，SurfaceView在底层机制中就实现了双缓冲机制。（双缓冲技术是游戏开发中的一个重要的技术。当一个动画争先显示时，程序又在改变它，前面还没有显示完，程序又请求重新绘制，这样屏幕就会不停地闪烁。而双缓冲技术是把要处理的图片在内存中处理好之后，再将其显示在屏幕上。双缓冲主要是为了解决 反复局部刷屏带来的闪烁。把要画的东西先画到一个内存区域里，然后整体的一次性画出来。）

  

  [SurfaceView 详解](https://www.jianshu.com/p/b037249e6d31)

  

* RecycleView的使用

  

* 序列化的作用，以及Android两种序列化的区别

  - Serializable接口
    是一个空接口，为对象提供标准的序列化和反序列化操作。
    其中有一个long类型的值为**serialVersionUID**，我们开发人员很少用到这个，但是官方推荐是最好可以声明这个属性，主要用于校验序列化和反序列化。

  > serialVersionUID的详细工作过程是这样的：序列化的时候系统会把当前类的serialVersionUID写入序列化的二进制文件中，当反序列化的时候系统会检测文件中的serialVersionUID是否和当前类的serialVersionUID一致，如果一致就说明序列化的类的版本和当前类的版本是相同的，这个时候可以成功反序列化；否则说明当前类和反序列化的类相比发生了某些变化，比如成员变量的数量、类型发生了变化，这个时候是无法正常反序列化的。

  - Parcelable接口
    是androidSdk特意为序列化和反序列化开发出来的一个接口，相当于Serializable具有更好的效率。
    但是Parcelable接口使用起来比较麻烦。具有如此的属性：

    | 方法                                | 功能                                                         | 标记位                        |
    | ----------------------------------- | ------------------------------------------------------------ | ----------------------------- |
    | createFromParcel(Parcel in)         | 从序列化后的对象中创建原始对象                               |                               |
    | newArray(int size)                  | 创建指定长度的原始对象数组                                   |                               |
    | User(Parcel in)                     | 从序列化后的对象中创建原始对象                               |                               |
    | writeToParcel(Parcel out,int flags) | 将当前对象写入序列化结构中                                   | PARCALABLE_WRITE_RETURN_VALUE |
    | describeContents                    | 返回当前对象的内容描述，几乎所有情况都返回0，仅在当前对象中存在文件描述符时返回1 | CONTENTS_FILE_DESCRIPTOR      |

  - 两者区别

    | 区别     | Serializable                          | Parcelable                              |
    | -------- | ------------------------------------- | --------------------------------------- |
    | 所属API  | JAVA API                              | Android SDK API                         |
    | 原理     | 序列化和反序列化过程需要大量的I/O操作 | 序列化和反序列化过程不需要大量的I/O操作 |
    | 开销     | 开销大                                | 开销小                                  |
    | 效率     | 低                                    | 很高                                    |
    | 使用场景 | 序列化到本地或者通过网络传输          | 内存序列化                              |

  

* 差值器

  根据初始值-终止值及时间函数，计算当前瞬间值。有线性匀速差值器，加速差值器，减速差值器等

  

* 估值器

  输入为差值器当前瞬间值，自定义map转换，输出目标值，类似二次差值器

  

* Android中数据存储方式

  - 文件存储
  - SharedPreferences
  - SQLite数据库存储
  - ContentProvider（进程间通信，比如媒体库media（内部实现为数据库））
  - 网络存储

（二）Android源码相关分析

* Android动画框架实现原理

  - 补间动画
    1. 窗口中的所有 View 是共用一个 Canvas 对象
    2. ViewGroup中，dispatchDraw->drawChild->child.draw(canvas)保证从顶至下，每一个View都绘制到
    3. 需要绘制的 View 的绘制位置是在 Canvas 通过 translate 函数调用来进行切换的，即正常无动画情形下，每个View的绘制原点(x, y)为相对父ViewGroup中的布局坐标
    4. ViewGroup dispatchDraw时，检测子View当前是否有动画，若有，取当前动画瞬间值，通过 translate 函数调用来进行切换改变子View的绘制原点，随时间偏移，即动画效果。

  - Gif动画

    它的实现是通过 Movie 这个类来对 Gif 文件进行读取和解码的，同时在 onDraw 函数中不断的绘制每一帧图片完成的，这个示例代码在 onDraw 中调用 invalidate 来反复让 View 失效来让系统不断调用 SampleView 的 onDraw 函数；至于选出哪一帧图片进行绘制则是传入系统当前时间给 Movie 类，然后让它根据时间顺序来选出帧图片。反复让 View 失效的方式比较耗资源，绘制效果允许的话可以采取延时让 View 失效的方式来减小 CPU 消耗。

    目前使用这个方式播放一些 Gif 格式的动画时会出现花屏的现象，这是因为 Android 中使用的 libgif 库是比较老的版本，新的 tag 不支持，所以导致花屏，解决办法有制作 Gif 图片时别使用太新的 tag 或完善 android 中对应的 libgif 库。

    

* Android各个版本API的区别

  - android 5.0引入material design风格

  - android 6.0引入权限动态管理

  - android 7.0引入多窗口支持，传统网络状态监听静态广播失效等

  - android 8.0引入画中画等

  

* Requestlayout，onlayout，onDraw，DrawChild区别与联系

  - requestLayout

    子View调用requestLayout方法，会标记当前View及父容器，同时逐层向上提交，直到ViewRootImpl处理该事件，ViewRootImpl会调用三大流程，从measure开始，对于每一个含有标记位的View及其子View都会进行测量onMeasure、布局onlayout、绘制onDraw。requestLayout如果没有改变l,t,r,b，那就不会触发onDraw；但是如果这次刷新是在动画里，mDirty非空，就会导致onDraw

  - onlayout

    如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局

  - onDraw

    绘制视图本身

  - DrawChild

    重新回调每个子视图的draw()方法

    

* invalidate和postInvalidate的区别及使用

  - invalidate: 用于在主线程刷新View，执行自身draw方法
  - postInvalidate：用于在子线程线程刷新View，执行自身draw方法（底层是mainHandler post  -> invalidate ）

  

* Activity-Window-View三者的差别

  - 通用比喻：Acitivty像一个工匠（控制单元），Window像窗户（承载模式）,View像窗花（显示视图），LayoutInflater像剪刀，xml配置像窗花图纸

  - 三者关系 
    1. 在Activity中调用attach，创建了一个Window
    2. 创建的window是其子类PhoneWindow，在attach中创建PhoneWindow
    3. 在Activity中调用setContentView(R.layout.xxx)实际上是调用的getWindow().setContentView()
    4. 调用PhoneWindow中的setContentView方法
    5. 创建ParentView：作为ViewGroup的子类，实际是创建的DecorView(作为FramLayout的子类）
    6. 将制定的R.layout.xxxx进行填充，通过布局填充器进行填充（其中的parent就是DecorView）
    7. 调用到ViewGroup
    8. 调用ViewGroup的removeAllView()，先将所有View移除，
    9. 添加新的View: addView()

  

* 谈谈对Volley的理解

  特性

  - 自动调度网络请求，支持多并发网络请求
  - 透明的磁盘内存响应结果缓存
  - 支持请求优先级调整
  - 支持取消请求，并提供相应API
  - 高度可拓展
  - 网络请求结果异步回调

  缺陷

  - 不适合做大的网络下载请求

    

* 如何优化自定义View

  - 降低刷新频率，减少不必要的调用invalidate()方法，尽量调用多参invalidate()，能做到局部刷新而不是整体刷新
  - 减少不必要的layout()调用，因为这个方法需要遍历整个View树来获取你真实的layout位置。如果真的必须经常性地调用，那么你可以考虑写一个特殊的ViewGroup

  - 使用好硬件加速（TargetApi>=11）

  - 初始化时创建对象；不要在onDraw方法内创建绘制对象

  - 做好状态的储存与恢复（onsaveinstance onrestoreinstance）

  

* 低版本SDK如何实现高版本api？

  - 使用support包的方法

  - 拷贝高版本api

  - 在代码中加入版本控制逻辑，添加@targetapi使编译器通过

    

* 描述一次网络请求的流程

  域名解析、TCP的三次握手、建立TCP连接后发起HTTP请求、服务器响应HTTP请求、浏览器解析html代码，同时请求html代码中的资源（如js、css、图片等）、最后浏览器对页面进行渲染并呈现给用户

  HTTP请求格式：

  - 请求行: 请求方法（GET/POST/DELETE/PUT/HEAD）、URI路径、HTTP版本号
  - 请求头: 缓存相关信息（Cache-Control，If-Modified-Since）
     客户端身份信息（User-Agent）等键值对信息
  - 消息体: 客户端发给服务端的请求数据，这部分数据并不是每个请求必须的(post put)

  HTTP响应格式：

  - 状态行：有HTTP协议版本号，状态码和状态说明三部分构成。
  - 响应头：用于说明数据的一些信息，比如数据类型、内容长度等键值对。
  - 消息体：服务端返回给客户端的HTML文本内容。或者其他格式的数据，比如：视频流、图片或者音频数据。

  

* HttpUrlConnection 和 okhttp关系

  从Android4.4开始HttpURLConnection的底层实现采用的是okHttp

  

* Bitmap对象的理解

  - Bitmap对象的内存分为两部分:
    - Bitmap对象
    - Bitmap像素数据
      - 在Android 2.3.3（API 10）之前，Bitmap的像素数据的内存时分配在Native堆上的，而Bitmap对象的内存则分配在Dalvik堆上的；
      - 由于Native堆上的内存时不受DVM管理的，如果想要回收Bitmap的所占用内存的话，那么需要调用Bitmap.recyle()方法。
      - 而API 10之后呢，谷歌将像素数据的内存分配也移到DVM堆上，由DVM管理，因此在dvm回收前;
         只需要保证Bitmap对象不被任何GC Roots强引用就可以回收这部分内存

  - 占用内存大，多对象易引发OOM

  - Bitmap占用内存计算 = 图片最终显示出来的宽 * 图片最终显示出来的高 * 图片品质（Bitmap.Config的值）

  - 减少内存消耗：根据View的大小利用BitmapFactory.Options计算合适的inSimpleSize(>1)来对Bitmap进行相对应的裁剪

  - Bitmap的优化主要是加快图片的加载速度和降低图片占用内存的大小

  

* looper架构

  封装消息循环和消息队列的一个类

  - Looper类用来为一个线程开启一个消息循环。 默认情况下android中新诞生的线程是没有开启消息循环的。(主线程除外,主线程系统会自动为其创建Looper对象,开启消息循环。) Looper对象通过MessageQueue来存放消息和事件。一个线程只能有一个Looper,对应一个MessageQueue。

  - 通常是通过Handler对象来与Looper进行交互的。Handler可看做是Looper的一个接口,用来向指定的Looper发送消息及定义处理方法。

  

* ActivityThread，AMS，WMS的工作原理

  - AMS和WMS都属于Android中的系统服务，被所有的App公用的

  - AMS(ActivityManagerServices)统一调度所有应用程序的Activity，负责系统中所有Activity的生命周期。
  - WMS(WindowManagerService)控制所有Window的显示与隐藏以及要显示的位置

  

  [Android系统服务 —— WMS与AMS](https://www.jianshu.com/p/47eca41428d6)

  

* 自定义View如何考虑机型适配

  - 合理使用warp_content，match_parent。
  - 尽可能的是使用RelativeLayout。
  - 针对不同的机型，使用不同的布局文件放在对应的目录下，android会自动匹配。
  - 尽量使用点9图片。
  - 使用与密度无关的像素单位dp，sp。
  - 引入android的百分比布局。
  - 切图的时候切大分辨率的图，应用到布局当中。在小分辨率的手机上也会有很好的显示效果。

  

* 自定义View的事件

  触摸&&绘制

  

* AstncTask+HttpClient 与 AsyncHttpClient有什么区别？

  

* LaunchMode应用场景

  sinngleTop: 防抖
  singleTask: 入口Activity
  singleInstance: 单实例，多应用公用，不常见

  

* AsyncTask 如何使用?

  AsyncTask是Android提供的一个助手类，它对Thread和Handler进行了封装，方便我们使用，asyncTask.execute() 只能在UI主线程中调用，多任务时不并发，线程池

  AsyncTask有四个重要的回调方法，分别是：onPreExecute（主线程）、doInBackground（工作线程）, onProgressUpdate（主线程，在doInBackground中手动调publishProgress） 和 onPostExecute（主线程 执行完毕）。

  局限性：

  - 在Android 4.1版本之前，AsyncTask类必须在主线程中加载，这意味着对AsyncTask类的第一次访问必须发生在主线程中；在Android 4.1以及以上版本则不存在这一限制，因为ActivityThread（代表了主线程）的main方法中会自动加载AsyncTask 
  - AsyncTask对象必须在主线程中创建 
  - AsyncTask对象的execute方法必须在主线程中调用 
  - 一个AsyncTask对象只能调用一次execute方法

  

* SpareArray原理

  - 当使用HashMap(K, V),如果K为整数类型时,使用SparseArray的效率更高

    ```
        mKeys = new int[initialCapacity];
        mValues = new Object[initialCapacity];
    ```

  - key value 分别为一个数组，key是有序插入的
  - 查找key时使用二分查找法，降低了时间复杂度（O（log2n）），根据找到的key的下标取value

  

  

* 请介绍下ContentProvider 是如何实现数据共享的？

  统一了数据访问方式

  

* AndroidService与Activity之间通信的几种方式

  - bindService 通过Binder得到Service对象

  - 广播

  - ......

    

* IntentService原理及作用是什么？

  IntentService保留了Service原有的特性，并且将耗时的操作都放在的子线程中，通过Handler的回调方式实现了数据的返回。

  - IntentService继承Service，专门处理异步请求。
  - 客户端通过调用startService(Intent)发起请求，自然数据是通过Intent进行传递的。
  - 一旦Service启动后，对于Intent所传递的数据操作都通过工作线程（worker thread）进行处理。
  - 在完成数据的处理之后，Handler会回调其处理结果。在任务结束后会将自身服务关闭。

  通过Handler、Message、Looper在Service中实现的异步线程消息处理的机制。但是由于是通过衍生Service的方式实现的，因此具有Service的生命周期特性。

  

* 说说Activity、Intent、Service 是什么关系

  Activity Service 四大组件，通过Intent传递消息

  

* ApplicationContext和ActivityContext的区别

  - 生命周期长短，某些静态引用等Root持有Activity的Context会引起内存泄漏
  - 和UI相关的方法基本都不建议或者不可使用Application

  - Context数量 = Activity数量 + Service数量 + 1

  ![img](https:////upload-images.jianshu.io/upload_images/10844298-b70f98cd3d717c2c..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  标注：

  - 数字1：启动Activity在这些类中是可以的，但是需要创建一个新的task。一般情况不推荐。

  - 数字2：在这些类中去layout inflate是合法的，但是会使用系统默认的主题样式，如果你自定义了某些样式可能不会被使用。

  - 数字3：在receiver为null时允许，在4.2或以上的版本中，用于获取黏性广播的当前值。（可以无视）

  

* SP是进程同步的吗?有什么方法做到同步？

  不是，使用 ContentProvider

  

* 谈谈多线程在Android中的使用

  - 避免ANR(UI线程5s 广播10s 服务20s)
  - 防止耗时操作拥堵线程  完成持续性长的耗时操作

  - Thread && Runnable:
  - 直接继承Thread和实现Runnable接口实现多线程区别
    - 众所周知在Java中类仅支持单继承，当定义一个新的类的时候，它只能扩展一个外部类。假如创建自定义线程类的时候是通过扩展 Thread类的方法来实现的，那么这个自定义类就不能再去扩展其他的类。因此，如果自定义类必须扩展其他的类，那么就可以使用实现Runnable接口的方法来定义该类为线程类，这样就可以避免Java单继承所带来的局限性。但继承Thread和实现Runnable重要区别并不是在于此，更重要的是实现Runnable接口的方式创建的线程可以处理同一资源，从而实现资源的共享。
    - 实现Runnable接口相对于扩展Thread类来说，具有无可比拟的优势。此方式不仅有助于程序的健壮性，使代码能够被多个线程共享，而且代码和数据资源相对独立，从而特别适合多个具有相同代码的线程去处理同一资源的情况。使得线程、代码和数据资源三者有效分离，很好地体现了面向对象程序设计的思想。因此，几乎所有的多线程程序都是通过实现Runnable接口的方式来完成的。

  - AsyncTask

  - IntentService

  

* 进程和 Application 的生命周期

  - 进程重要级：前台（foreground）>可视（visible）>服务(service)>背景（background）>空（cache）
  - Application生命周期：

    ```
    public class App extends Application {
    
        @Override
        public void onCreate() {
            // 程序创建的时候执行
            super.onCreate();
        }
        @Override
        public void onTerminate() {
            // 程序终止的时候执行
            super.onTerminate();
        }
        @Override
        public void onLowMemory() {
            // 低内存的时候执行
            super.onLowMemory();
        }
        @Override
        public void onTrimMemory(int level) {
            // 程序在内存清理的时候执行
            super.onTrimMemory(level);
        }
        @Override
        public void onConfigurationChanged(Configuration newConfig) {
            super.onConfigurationChanged(newConfig);
        }    
    }
    ```

  

* 封装View的时候怎么知道view的大小

  - 调用view宽高时期view.getMeasuredWidth/Height（确保view已经测量完毕）。

  - Activity/View#onWindowFocusChanged 它会被调用多次，当 Activity的窗口得到焦点和失去焦点均会被调用。
  - View.post(runnable) 通过post将一个runnable投递到消息队列的尾部，当Looper调用此 runnable的时候，View也初始化好了。
  - ViewTreeObserver.addOnGlobalLayoutListener

  

* RecycleView原理

* AndroidManifest的作用与理解

（三）常见的一些原理性问题

* Handler机制和底层实现

  - 在消息接收的线程初始化Handler实例，若接收消息的线程非主线程，需要开启Looper，主线程默认开启Looper，一个线程只有一个Looper与一个MessageQueue，可以拥有多个Handler。 一个Message经由Handler的发送，MessageQueue的入队，Looper的抽取，又再一次地回到Handler的怀抱
  - 底层实现
    - 一个线程有且只有一个Looper、Looper持有唯一MessageQueue消息队列
    - Handler构造函数需要Looper，若不指定Looper，生成Handler实例时：Looper.myLooper() 通过ThreadLocal<Looper>来实现线程内Looper唯一性。
    - Handler消息的post（message、runnable）会封装一个Message(target字段持有该handler对象)，Message入消息队列MessageQueue
    - Looper循环遍历MessageQueue，取出msg，调用msg.handler.dispatchMessage(msg)
      - 若msg.runnable不为Null，调用msg.runnable.run()。
      - 否则，若Handler构造方法中的代理Handler.Callback不为Null，方式执行mCallback.handleMessage(msg)。
      - 否则，执行handler.handleMessage(msg)

  

* Handler、Thread和HandlerThread的差别

  - Handler: 线程间通信

  - Thread: 线程

  - HandlerThread: 内部拥有并管理一个looper的thread，减少了开发者的工作量

    

* handler发消息给子线程，looper怎么启动？

  - 手动调用 Looper.prepare():

    ```
    private static void prepare(boolean quitAllowed) {
            if (sThreadLocal.get() != null) {
                throw new RuntimeException("Only one Looper may be created per thread");
            }
            sThreadLocal.set(new Looper(quitAllowed));
    }
    ```

  - Looper.loop();

    ```
    public static void loop() {
            final Looper me = myLooper();
            if (me == null) {
                throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
            }
            final MessageQueue queue = me.mQueue;
    
            // Make sure the identity of this thread is that of the local process,
            // and keep track of what that identity token actually is.
            Binder.clearCallingIdentity();
            final long ident = Binder.clearCallingIdentity();
    
            for (;;) {
                Message msg = queue.next(); // might block
                if (msg == null) {
                    // No message indicates that the message queue is quitting.
                    return;
                }
    
                // This must be in a local variable, in case a UI event sets the logger
                Printer logging = me.mLogging;
                if (logging != null) {
                    logging.println(">>>>> Dispatching to " + msg.target + " " +
                            msg.callback + ": " + msg.what);
                }
    
                msg.target.dispatchMessage(msg);
    
                if (logging != null) {
                    logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
                }
    
                // Make sure that during the course of dispatching the
                // identity of the thread wasn't corrupted.
                final long newIdent = Binder.clearCallingIdentity();
                if (ident != newIdent) {
                    Log.wtf(TAG, "Thread identity changed from 0x"
                            + Long.toHexString(ident) + " to 0x"
                            + Long.toHexString(newIdent) + " while dispatching to "
                            + msg.target.getClass().getName() + " "
                            + msg.callback + " what=" + msg.what);
                }
    
                msg.recycleUnchecked();
            }
        }
    ```

    


* 关于Handler，在任何地方new Handler 都是什么线程下?

  Handler构造函数需要Looper，若不指定Looper，生成Handler实例时：Looper.myLooper() 通过ThreadLocal<Looper>来实现线程内Looper唯一性，即当前代码运行时所在线程的Looper。

  

* ThreadLocal原理，实现及如何保证Local属性？

  - ThreadLocal并不是一个Thread，而是Thread的局部变量，也许把它命名ThreadLocalVariable更容易让人理解一些。

  - 当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。
    可以用于维护单例中的非线程安全对象，从线程的角度看，每个线程都保持一个对其线程局部变量副本的隐式引用，只要线程是活动的并且 ThreadLocal 实例是可访问的；在线程消失之后，其线程局部实例的所有副本都会被垃圾回收（除非存在对这些副本的其他引用）。

  - 实现：在ThreadLocal类中有一个table，用于存储每一个线程的变量副本，Map中元素的键为线程对象，而值对应线程的变量副本

  

* 请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系

  在开发Android 应用时必须遵守单线程模型的原则：

  1. 不要阻塞UI线程

  2. 确保只在UI线程中访问Android UI工具包

     

* 请描述一下View事件传递分发机制

  dispatchTouchEvent -> onInterceptTouchEvent -> onTouchEvent

  

* Touch事件传递流程

  - 从Activity根触发TouchEvent事件

  - ViewGroup.dispatchTouchEvent遍历子View（childView），定位第一个消耗事件的View即mMotionTarget，步骤

    - TouchEvent事件的屏幕坐标(x,y)必须命中childView矩形范围Rect并且childView必须是Visibility
    - childView.dispatchTouchEvent返回ture，如果没有覆写，dispatchTouchEvent会调用onTouchEvent
    - childView若为ViewGroup，继续递归

  - 生成TouchEvent路径链，viewGoup -> viewGoup.mMotionTarget-> viewGoup.mMotionTarget.mMotionTarget-> ... ->mMotionTarget，此后Event事件仅会沿此路径传递

  - dispatchTouchEvent调用onInterceptTouchEvent，若返回false不拦截事件，代理调用mTagerView. dispatchTouchEvent，递归到最后mMotionTarget，若为View，调用super.dispatchTouchEvent如下。

    ```
        @Override
        public boolean dispatchTouchEvent(MotionEvent event) {
            if (mOnTouchListener != null && (mViewFlags & ENABLED_MASK) == ENABLED &&
                    mOnTouchListener.onTouch(this, event)) {
                return true;
            }
            return onTouchEvent(event);
        }
    ```

    - onTouchListener优先级大于onTouchEvent
    - dispatchTouchEvent调用onTouchEvent，类似代理分发

  - 中间任一环节的viewGroup.onInterceptTouchEvent返回true，将置当前viewGoup.mMotionTarget为Null，切断Event路径链，导致事件拦截

  

* 事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？

  事件处理优先级：onTouchListener（若返回true则不向下调用）> onTouchEvent > onClickListener

  

* View和ViewGroup分别有哪些事件分发相关的回调方法

  - dispatchTouchEvent

  - onInterceptTouchEvent

  - onTouchEvent

    

* View刷新机制

  - Android每隔16.6ms会刷新一次屏幕，即60帧。

* View绘制流程

  - measure -> layout -> draw
  - onMeasure执行：

  - forceLayout为true：这表示强制重新布局，可以通过View.requestLayout()来实现；

  - needsLayout为true，这需要specChanged为true（表示本次传入的MeasureSpec与上次传入的不同），并且以下三个条件之一成立：

    - sAlwaysRemeasureExactly为true: 该变量默认为false；

    - isSpecExactly为false: 若父View对子View提出了精确的宽高约束，则该变量为true，否则为false
    - matchesSpecSize为false: 表示父View的宽高尺寸要求与上次测量的结果不同

  

* 自定义控件原理

* 自定义View如何提供获取View属性的接口？

* Android代码中实现WAP方式联网

* AsyncTask机制

* AsyncTask原理及不足

  

* 如何取消AsyncTask？

  cancel方法 --> isCancelled()为true --> 在doInBackground中手动检查决定是否继续运行

  我们可以随时调用 cancel（boolean）去取消这个加载任务，调用这个方法会间接调用 iscancelled 并且返回true 。
  调用cancel()后，在doInBackground（）return后 我们将会调用onCancelled(Object) 不在调用onPostExecute(Object)
  为了保证任务更快取消掉，你应该在doInBackground（）周期性的检查iscancelled 去进行判断。

  

* 为什么不能在子线程更新UI？

  - Exception:Only the original thread that created a view hierarchy can touch its views

    ```
    void checkThread() {
        if (mThread != Thread.currentThread()) {
           throw new CalledFromWrongThreadException("Only the original thread that created a view          hierarchy can touch its views.");
        }
    }
    ```

  - 在Activity创建完成后（Activity的onResume之前ViewRootImpl实例没有建立），mThread被赋值为主线程（ViewRootImpl），所以直接在onCreate中创建子线程是可以更新UI的

  - 在子线程中添加 Window，并且创建 ViewRootImpl，可以在子线程中更新view

  - 设计思考：
    Android 的单线程模型，因为如果支持多线程修改 View 的话，由此产生的线程同步和线程安全问题将是非常繁琐的，所以 Android 直接就定死了，View 的操作必须在创建它的 UI 线程，从而简化了系统设计。
    有没有可以在其他非原始线程更新 ui 的情况呢？有，SurfaceView 就可以在其他线程更新。

  

* ANR产生的原因是什么？

* ANR定位和修正

* oom是什么？

* 什么情况导致oom？

* 有什么解决方法可以避免OOM？

* Oom 是否可以try catch？为什么？

* 内存泄漏是什么？

* 什么情况导致内存泄漏？

* 如何防止线程的内存泄漏？

* 内存泄露场的解决方法

* 内存泄漏和内存溢出区别？

* LruCache默认缓存大小

* ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)

* 如何通过广播拦截和abort一条短信？

* 广播是否可以请求网络？

* 广播引起anr的时间限制是多少？

* 计算一个view的嵌套层级

* Activity栈

* Android线程有没有上限？

* 线程池有没有上限？

* ListView重用的是什么？

* Android为什么引入Parcelable？

* 有没有尝试简化Parcelable的使用？

（四）开发中常见的一些问题

* ListView 中图片错位的问题是如何产生的?
* 混合开发有了解吗？
* 知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，H5，小程序，WPA等。做Android的了解一些前端js等还是很有好处的)；
* 屏幕适配的处理技巧都有哪些?
* 服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？
* 动态布局的理解
* 怎么去除重复代码？
* 画出 Android 的大体架构图
* Recycleview和ListView的区别
* ListView图片加载错乱的原理和解决方案
* 动态权限适配方案，权限组的概念
* Android系统为什么会设计ContentProvider？
* 下拉状态栏是不是影响activity的生命周期
* 如果在onStop的时候做了网络请求，onResume的时候怎么恢复？
* Bitmap 使用时候注意什么？
* Bitmap的recycler()
* Android中开启摄像头的主要步骤
* ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？
* 点击事件被拦截，但是想传到下面的View，如何操作？
* 微信主页面的实现方式
* 微信上消息小红点的原理
* CAS介绍（这是阿里巴巴的面试题，我不是很了解，可以参考博客: CAS简介）