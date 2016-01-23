# Android-Collection
安卓开发知识点集锦

博客：	[安卓之家](http://jp1017.gitcafe.io/)

# mipmap目录和drawable目录区别
---

mipmap在使用上，把它当drawable用就好了。但是用mipmap系统会在缩放上提供一定的性能优化，因此图标最好放到mipmap目录下，看下官方总结：

<!--more-->

```bash
It’s best practice to place your app icons in mipmap- folders (not the drawable- folders) 
    because they are used at resolutions different from the device’s current density.
```

# 屏幕尺寸与分辨率
---

1.1 手机常见分辨率:
4:3

| 1   |	 2 |
| ---- | ----:|
| VGA： 640×480 (Video Graphics Array)  |	QVGA： 320×240 (Quarter VGA) |
| HVGA： 480×320 (Half-size VGA)	|  SVGA： 800×600 (Super VGA) |

5:3

WVGA 800×480 (Wide VGA)

16:9

|1|	2|
| ---- | ----:|
|FWVGA： 854×480 (Full Wide VGA)	|HD： 1920×1080 High Definition
|QHD： 960×540 |720p： 1280×720 标清|
|1080p 1920×1080 高清||

1.2 分辨率对应DPI

|1|	2|
| ---- | ----:|
|“HVGA： mdpi”	|“WVGA： hdpi “
|“FWVGA： hdpi “	|“QHD： hdpi “
|“720P： xhdpi”|	“1080P： xxhdpi “

# 获取SHA1和MD5值
---

如何获取安卓签名证书的SHA1和MD5值呢，这里用命令行操作，adt可以在设置里查看。
直接来了哈：

```sh
cd ~/.android
keytool -list -v -keystore debug.keystore
```

搞定！提示输入密码的时候输入android或者直接回车。

# setEnabled
---

View的setEnabled方法，使能控件，如果设置为false，该控件永远不会活动，不管设置为什么属性，都无效；
设置为true，表明激活该控件，控件处于活动状态，处于活动状态，就能响应事件了，比如触摸、点击事件等；
setEnabled就相当于一个总开关，只有总开关打开了，才能使用其他事件。
另外还有，setClickable，使能点击，设置为true时，表明控件可以点击，如果为false，就不能点击；注意，setOnClickListener方法会默认把控件的setClickable设置为true。

## setFocusable 

使能控件获得焦点，设置为true时，并不是说立刻获得焦点，要想立刻获得焦点，得用requestFocus；
使能获得焦点，就是说具备获得焦点的机会、能力，当有焦点在控件之间移动时，控件就有这个机会、能力得到焦点。

# onAttachedToWindow()
---

关于onAttachedToWindow()，要知道两点：
1. onAttachedToWindow()运行在onResume之后；
2. DecorView的LayoutParams是在ActivityThread的handleResumeActivity中设置的，并且该函数会调用Activity的onResume生命周期，所以在onResume之后可以设置窗体尺寸，也就是在onAttachedToWindow()方法里设置。
详细说明，请看这里：onAttachedToWindow()在整个Activity生命周期的位置及使用
补充两个图，activity和fragment完整生命周期方法：

 
|activity	|fragment |
|- |- |
|![activity](http://7xlah4.com1.z0.glb.clouddn.com/20151016activity%E5%AE%8C%E6%95%B4%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%96%B9%E6%B3%95.jpg)	|![fragment](http://7xlah4.com1.z0.glb.clouddn.com/20151016activity%E5%92%8Cfragment%E5%AE%8C%E6%95%B4%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%96%B9%E6%B3%95.jpg)

# padding
---

View的padding属性是内边距，类似的是margin，外边距，一般用于view和view，如下图：
![padding](http://7xlah4.com1.z0.glb.clouddn.com/20151016padding和margin.png)

# 使用dp和sp
---

使用dp和sp适应不同屏幕，dp一般用于view，sp常用于文本
设置两个view之间的空间时，应该使用dp而不是px：

```xml
<Button 
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:text="@string/clickme"
	android:layout_marginTop="20dp"
	/>
```

当指定文本尺寸时，始终应该使用sp：

```xml
<TextView
	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	android:textSize="20sp"
	/>
```

# Android Support包
---

Google官方提供Android Support Library package来保证高版本SDK的向下兼容。通过使用Support包，可以让拥有最新SDK特性的应用运行在相应API(比如v7就是API Level 7即Android 2.1)及更高版本的设备之上。
+ v4 Support Library
此包用在API lever 4(即Android 1.6)及更高版本之上。它包含了较多的内容，使用广泛，例如：Fragment，NotificationCompat，LoadBroadcastManager，ViewPager，PageTabStrip，Loader，FileProvider 等。
+ v7 Support Libraries
此包是针对API level 7(即Android 2.1)及以上版本而设计的，但是v7是要依赖v4这个包的，v7支持了Action Bar以及一些Theme的兼容。

>Note: v7 appcompat library
v7 appcompat library 是包含在 v7 Support Libraries里面的一个包，正是此包增加了Action Bar 用户界面的设计模式，并加入了对material design 的支持，是我们使用最多的一个兼容包。

+ v13 Support Library
此包是针对API level 13(即Android 3.2)及更高版本设计的，一般我们都不常用，平板开发中能用到，这里就不介绍了。
+ v17 Preference Support Library for TV
此包主要是为了TV设备而设计。
常见问题分析：

```sh
No resource found that matches the given name '@style/Theme.AppCompat.Light'
```
这种问题一般就是兼容包没处理好导致，那么导入v7库就好了，或者把AndroidManifest.xml文件里面，minSdkVersion改成14，甚至修改主题为@style/Theme.Light，哈哈，推荐第一种哦，导入v7包。

# assets和res
---

+ assets:
不会在R.java文件下生成相应的标记，assets文件夹可以自己创建文件夹，必须使用AssetsManager类进行访问，存放到这里的资源在运行打包的时候都会打入程序安装包中，访问方法：

```java
inputStreamReader = new InputStreamReader(Context.getAssets().open("test.json"), "UTF-8");
```

+ res:
会在R.java文件下生成标记，这里的资源会在运行打包操作的时候判断哪些被使用到了，没有被使用到的文件资源是不会打包到安装包中的。
res/raw和assets文件夹来存放不需要系统编译成二进制的文件，例如字体文件等
res/xml:可以在Activity中使用getResource().getXML()读取这里的资源文件
res/raw:该目录下的文件可以直接复制到设备上，不能有子文件夹，编译软件时，这里的数据不需要编译，直接加入到程序安装包中，使用方法是getResource().OpenRawResources(ID),其中参数ID的形式是R.raw.XXX.

# Android属性allowBackup
---

最好设置成false，黑客易获取帐号密码登录你的应用，详情这里：Android 属性 allowBackup 安全风险浅析
尽管新版本1.0.32移除了backup 和 restore，还有旧版要提防哦。

# include merge ViewStub在布局中的使用
---

+ include

单个组件的代码重用可用style，多个组件的共同重用可用include
include引用的layout的根标签：可以为viewGroup标签，也可为单个widget，或者是merge
经常会有同学在RelativeLayout中使用include标签
但是却发现include进来的控件无法用layout_alignParentBottom=”true”之类的标签来调整。这个真的非常恼火。其实解决方法非常简单，只要你在include的时候同时重载下layout_width和layout_height这两个标签就可以了。如果不重载，任何针对include的layout调整都是无效的！

+ merge

可以优化布局层级，一般用于FrameLayout。
merge布局和FrameLayout类似,相同的效果。不同的是 merge布局只能被标签包含，或者Activity.setContentView所使用。
当LayoutInflater遇到能被其他layout用包含进去，并不再另外生成ViewGroup容器，本元素也特别有用这个标签时，它会跳过它，并将内的元素添加到的父元素里。Activity能直接使用的原因是Activity的父元素是FrameLayout
merge能被其他layout用包含进去，并不再另外生成ViewGroup容器.就是说,会减少一层layout到达优化layout的目的

**限制:**
只能作为XML布局的根标签使用。
当Inflate以开头的布局文件时，必须指定一个父ViewGroup，并且必须设定attachToRoot为true（参看inflate(int, android.view.ViewGroup, Boolean)方法）。

+ ViewStub 

此标签可以使UI在特殊情况下，直观效果类似于设置View的不可见性，但是其更大的意义在于被这个标签所包裹的Views在默认状态下不会占用任何内存空间。ViewStub通过include从外部导入Views元素。
用法：通过android:layout来指定所包含的内容。默认情况下，ViewStub所包含的标签都属于visibility=GONE。ViewStub通过方法inflate()来召唤系统加载其内部的Views。

```xml
<ViewStub android:id="@+id/stub"
android:inflatedId="@+id/subTree"
android:layout="@layout/mySubTree"
android:layout_width="120dip"
android:layout_height="40dip" />
```

# handler的使用注意点
---

handle
详细请看技术小黑屋：[Android中Handler引起的内存泄露](http://droidyue.com/blog/2014/12/28/in-android-handler-classes-should-be-static-or-leaks-might-occur/?droid_refer=ninki_posts)
这里总结几点：
1 当一个Android应用启动的时候，会自动创建一个供应用主线程使用的Looper实例。Looper的主要工作就是一个一个处理消息队列中的消息对象。在Android中，所有Android框架的事件（比如Activity的生命周期方法调用和按钮点击等）都是放入到消息中，然后加入到Looper要处理的消息队列中，由Looper负责一条一条地进行处理。主线程中的Looper生命周期和当前应用一样长。

2 当一个Handler在主线程进行了初始化之后，我们发送一个target为这个Handler的消息到Looper处理的消息队列时，实际上已经发送的消息已经包含了一个Handler实例的引用，只有这样Looper在处理到这条消息时才可以调用Handler#handleMessage(Message)完成消息的正确处理。

3 在Java中，非静态的内部类和匿名内部类都会隐式地持有其外部类的引用。静态的内部类不会持有外部类的引用。关于这一内容可以查看：[细话Java：”失效”的private修饰符](http://droidyue.com/blog/2014/10/02/the-private-modifier-in-java/)

要解决这种问题，思路就是不使用非静态内部类，继承Handler时，要么是放在单独的类文件中，要么就是使用静态内部类。因为**静态的内部类不会持有外部类的引用，所以不会导致外部类实例的内存泄露**。当你需要在静态内部类中调用外部的Activity时，我们可以使用弱引用来处理。另外关于同样也需要将Runnable设置为静态的成员属性。注意：一个静态的匿名内部类实例不会持有外部类的引用。 修改后不会导致内存泄露的代码如下

```java
public class SampleActivity extends Activity {

  /**
   * Instances of static inner classes do not hold an implicit
   * reference to their outer class.
   */
  private static class MyHandler extends Handler {
    private final WeakReference<SampleActivity> mActivity;

    public MyHandler(SampleActivity activity) {
      mActivity = new WeakReference<SampleActivity>(activity);
    }

    @Override
    public void handleMessage(Message msg) {
      SampleActivity activity = mActivity.get();
      if (activity != null) {
        // ...
      }
    }
  }

  private final MyHandler mHandler = new MyHandler(this);

  /**
   * Instances of anonymous classes do not hold an implicit
   * reference to their outer class when they are "static".
   */
  private static final Runnable sRunnable = new Runnable() {
      @Override
      public void run() { /* ... */ }
  };

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Post a message and delay its execution for 10 minutes.
    mHandler.postDelayed(sRunnable, 1000 * 60 * 10);

    // Go back to the previous Activity.
    finish();
  }
}
```

其实在Android中很多的内存泄露都是由于**在Activity中使用了非静态内部类导致**的，就像本文提到的一样，所以当我们使用时要非静态内部类时要格外注意，如果其实例的持有对象的生命周期大于其外部类对象，那么就有可能导致内存泄露。个人倾向于使用静态类和弱引用的方法解决这种问题。

# 使用SparseArray替代HashMap
---

这个属于安卓性能优化的范畴，SparseArray是android为这样的Hashmap而专门写的类，目的是提高效率，其核心是折半查找函数（binarySearch）。

```java
private static int binarySearch(int[] a, int start, int len, int key) {
    int high = start + len, low = start - 1, guess;

    while (high - low > 1) {
        guess = (high + low) / 2;

        if (a[guess] < key)
            low = guess;
        else
            high = guess;
    }

    if (high == start + len)
        return ~(start + len);
    else if (a[high] == key)
        return high;
    else
        return ~high;
}
```

在Android中，当我们需要定义

```java
HashMap<Integer, E> hashMap = new HashMap<Integer, E>();
```

时，我们可以使用如下的方式来取得更好的性能。

```java
SparseArray<E> sparseArray = new SparseArray<E>();
```

相应的还有SparseBooleanArray和SparseIntArray，分别用来取代键为Integer，值为Boolean以及键和值都为Integer的HashMap。

# 安卓资源及访问
---

1 在Java代码中访问资源文件

R.res_type.xxx 来访问自定义资源文件
android.R.res_type.xxx 来访问系统提供的资源文件
还可以通过类Resources 来访问资源文件，Resources提供了一个getSystem()静态方法返回本类对象，即可调用getXxx() 类型，返回各个不同类型的值，比如：
String appName = Resources.getSystem().getString(R.string.app_name);

2 在XML文件中访问资源文件

引用自定义资源 @res_type/xxx 比如：android.text=”@string/hello_world”
引用系统资源 @android:res_type/xxx 比如：android.background=”@android:color/green”
一般情况下都会按照上面的方式访问资源，但是有一些特殊的：引用主题属性 ?android:attr/xxxx
系统自带的资源文件是存放在SDK安装目录android-sdk/platforms/android-xx/data/res_type/xxx下
，不同的类型，在不同的文件夹下，比如drawable-xhdpi下有很多资源。

# Character类的digit方法
---

static int digit(char ch, int radix)：根据基数返回当前字符的值的十进制。如果不满足Character.MIN_RADIX <= radix <= Character.MAX_RADIX，或者，ch不是radix基数中的有效值，返回”-1”；如果ch是“大写”的A到Z之间，则返回ch - ‘A’ + 10 的值；如果是“小写”a到z之间，返回ch - ‘a’ + 10 的值。

```java
System.out.println("Character.MIN_RADIX: " + Character.MIN_RADIX );
System.out.println("Character.MAX_RADIX: " + Character.MAX_RADIX );
System.out.println("Character.digit('2',2): " + Character.digit('2',2) );
System.out.println("Character.digit('7',10): " + Character.digit('7',10) );
System.out.println("Character.digit('F',16): " + Character.digit('F',16) );
结果为：
Character.MIN_RADIX: 2
Character.MAX_RADIX: 36
Character.digit('2',2): -1
Character.digit('7',10): 7
Character.digit('F',16): 15
```

一个应用请看这里：[java下16进制字符串和字节数组的相互转化](http://jp1017.gitcafe.io/2015/11/04/java%E4%B8%8B16%E8%BF%9B%E5%88%B6%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%92%8C%E5%AD%97%E8%8A%82%E6%95%B0%E7%BB%84%E7%9A%84%E7%9B%B8%E4%BA%92%E8%BD%AC%E5%8C%96/)
其逆运算是forDigit:
static char forDigit(int digit, int radix) ：根据特定基数判断当前数值表示的字符。4的逆运算，非法数值时返回“’\u0000’”。

```java
System.out.println("Character.MIN_RADIX: " + Character.MIN_RADIX );
System.out.println("Character.MAX_RADIX: " + Character.MAX_RADIX );
System.out.println("Character.forDigit(2,2): " + Character.forDigit(2,2) );
System.out.println("Character.forDigit(7,10): " + Character.forDigit(7,10) );
System.out.println("Character.forDigit(15,16): " + Character.forDigit(15,16) );
结果为：
Character.MIN_RADIX: 2
Character.MAX_RADIX: 36
Character.forDigit(2,2):
Character.forDigit(7,10): 7
Character.forDigit(15,16): f
```

# 65536方法数异常
---

请参考博文：安卓错误：[DexIndexOverflowException:method ID not in [0,0xffff]:65536](http://jp1017.gitcafe.io/2015/11/10/%E5%AE%89%E5%8D%93%E9%94%99%E8%AF%AF%EF%BC%9ADexIndexOverflowException-method-ID-not-in-0-0xffff-65536/)

# service使用注意点
---

# java定时器举例
---

```java
//倒计时30s操作
new CountDownTimer(30000, 100) {
    @Override
    public void onTick(long millisUntilFinished) {
        btnTraceDate.setText("seconds remaining: "
                + millisUntilFinished / 1000 + " S " + millisUntilFinished % 1000 / 100);
    }

    @Override
    public void onFinish() {
        btnTraceDate.setText("Done");
    }
}.start();
```

# 内存优化
---

通常情况下我们所说的内存是指手机的RAM，它包括以下几部分：
1 寄存器：寄存器处于CPU内部，在程序中无法控制；

2 栈：存放基本数据类型和对象的引用；

3 堆：存放对象和数组，由虚拟机GC来管理；

4 静态存储区域(static field)：在固定的位置存放应用程序运行时一直存在的数据，Java在内存中专门划分了一个静态存储区域来管理一些特殊的数据变量，如静态的数据变量；View一般不要设置为static

5 常量池(constant pool)：虚拟机必须为每个被装在的类维护一个常量池，常量池就是这个类所用的常量的一个有序集合，包括直接常量（基本类型、string）和对其他类型、字段和方法的符号引用。

# getResources().getColor() is deprecated
---

用23的api，会有此警告，处女座的朋友看着不爽，那么找一个替换下咯，推荐用这个：

```java
ContextCompat.getColor(context, R.color.color_name)
```

ContextCompat是v4包里的，请放心使用，另外还有`getDrawable()等方法`。

# SharedPreference.Editor的apply和commit方法异同

1. apply没有返回值，而commit返回boolean，true表明修改提交成功。

2. apply是将修改数据原子提交到内存, 后面再调用apply函数，数据将会直接覆盖前面的内存数据，然后异步提交到硬件磁盘,这样从一定程度上提高了很多效率；而commit是同步的提交到硬件磁盘，因此，在多个并发的提交commit的时候，他们会等待正在处理的commit保存到磁盘后再操作下一个数据，从而降低了效率。

3. apply方法不会提示任何失败的提示。 
由于在一个进程中，sharedPreference是单实例，一般不会出现并发冲突，如果对提交的结果不关心的话，建议使用apply，当然需要确保提交成功且有后续操作的话，还是需要用commit的。

# Snackbar
---

```java
    Snackbar.make(getWindow().getDecorView(), "Hi, Snack!", Snackbar.LENGTH_SHORT).show();
```

# 安卓事件
---

先来个文章：
[Android触摸事件分发机制](http://hunankeda110.iteye.com/blog/1944311)

分为`事件传递`和`事件处理`

+ 事件传递：MotionEvent事件的传递是采用隧道方式传递。隧道方式，即从根元素依次往下传递直到最内层子元素或在中间某一元素中由于某一条件停止传递。

+ 事件处理：MotionEvent事件的处理采用冒泡方式。冒泡方式，从最内层子元素依次往外传递直到根元素或在中间某一元素中由于某一条件停止传递。

Android中onclick，onLongClick是都是由ACTION_DOWN，ACTION_UP组成。如果在同一个View中onTouchEvent、onclick、onLongClick都进行了重写。onTouchEvent最先捕获ACTION_DOWN、ACTION_UP等单元事件。接下来才可能发生onClick、onLongClick事件。一个onclick事件是由ACTION_DOWN和ACTION_UP组成的。一个onLongClick事件至少有一个ACTION_DOWN。

onLongClick是在单独的线程执行，发生在ACTION_UP之前。Onclick发生在ACTION_UP之后，也就是说，如果在onLongClick返回false，onclick就会发生，而onlongClick返回true，则代表此事件已经被消费。Onclick不再发生。





分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！
