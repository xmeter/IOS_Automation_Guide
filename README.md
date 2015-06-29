IOS自动化指南
============
前言
---
随着进入移动互联网时代，移动操作系统在人们的生活当中占有的比重越来越高，而在移动操作系统当中，最主流的就是Android和IOS，而随着移动项目的逐渐开展，越来越多的公司对自动化的需求也逐渐强烈，那么现在我们就通过苹果公司自己的这套自动化工具（**UIAutomation**）来进行一个解说。
环境搭建
-------
在苹果公司来讲，它的产品都是封闭的，所以要使用它的工具，就必须要有它的产品，所以一个MAC是必不可少的，不论是MAC MINI, MACBOOK，还是IMAC，你至少也得有一样，当然有点人会说，我可以在PC上装黑苹果啊，这个看起来确实是一个解决方案，但是，黑苹果在驱动的兼容性上是极其坑的，如果你要在黑苹果上搞开发，那么与环境搞抗争将会成为你的日常。

硬件上的问题就说到这里了，那么说说软件，我们在进行IOS的自动化的过程中需要哪些软件呢？由于苹果本身标榜的就是人机友好的交互，所以，我们需要安装的东西都是高度集成的，不需要安太多：

1. Xocde 可以从MAC App store直接下载
 
2. IOS 模拟器

3. Xcode command line工具 最好是最新的

4. 如果需要把你的APP编译到你的设备上去，你还需要一个开发者账号，这里我们不讨论

因为UIAutomation是用JS来写的，没有一个高度集成的IDE来做，所以我们既可以在instruments里面来写，也可以用记事本来写，我个人推荐sublime text来写。但是如果你是初学的话，最好是在instruments里面来写，这样也方便你运行与调试。

开始第一个UIA脚本
----------------
环境搭建好以后，我们就可以开始来书写我们的第一个脚本了，那么要写脚本，我们就要首先指定我们要测试的app，然而这个时候就要分为两种情况了：

1. 在模拟器上

2. 在真机上

### 在模拟器上 ###
如果我们要在模拟器上写脚本，我们需要项目的源码，然后把他编译成为.app文件，这样我们才能够通过模拟器来运行被测程序。

源码项目一般是以.xcodeproj结尾的，这时候我们可以双击这个项目，打开它。然后我们就可以看到Xcode的界面了。
![xcode主界面](https://geekpics.net/images/2015/06/17/YdWAq0O3.jpg)

在Xcode的界面里，我们需要设置以下位置（也就是scheme）的东西，这样我们就指定了被测程序以及被测程序运行的模拟器。
![设置scheme](https://geekpics.net/images/2015/06/17/cYrTDi.jpg)

启动Instruments（Product>Profile）或者command + I。
![启动instruments](https://geekpics.net/images/2015/06/17/V9TmjW1.jpg)

在弹出来的这个窗口内选择Automation这个template。
![弹出窗](https://geekpics.net/images/2015/06/17/MwlN6au7.jpg)

Instruments打开以后，我们就可以在以下区域写脚本了。
![instruments主窗口](https://geekpics.net/images/2015/06/17/CHvG.jpg)

###在真机上###
在真机上又分为两种情况：

1. 有源码
2. 无源码

**有源码**的情况基本跟在模拟器上跑一样，我们需要先把真机用USB连接起来，然后在真机的setting>developer option>enable uiautomation 勾选上。然后我们设置scheme为UIADemo>IOS Device。
![设置scheme](https://geekpics.net/images/2015/06/18/3Dhbrt7.jpg)
然后我们要保证我们的APP是用开发者账户编译的，因为我没有开发者账号，所以就不细讲这点了。
后面的步骤就可以参考在模拟器上运行的步骤。

**无源码**的情况则有一定的区别了，这个时候咱们只有一个.ipa文件，那么首先，我们可以借助各种助手类工具（比如ITools）把这个APP先安装到我们的设备上，然后打开Xcode>open developer tool>Instruments
![打开instruments](https://geekpics.net/images/2015/06/18/MJhex4.jpg)
然后就可以看到如下弹出窗口，在这个窗口里设置设备以及要运行的app，接着选中Automation这个template，点击确定。
![选择template](https://geekpics.net/images/2015/06/18/s7J4O0.jpg)
这样我们就进入到了instruments的主界面，然后就可以开始书写第一个程序了。
![instruments主窗口](https://geekpics.net/images/2015/06/17/CHvG.jpg)
### 第一个UIA脚本的书写 ###
完成了以上内容的设置以后，我们就可以正式开始书写脚本了，下面我们就来实现以下：

    var target = UIATarget.localTarget();
    tablet.logElementTree();
以上的代码实现了打印出当前正在运行设备的所有元素的效果：
![元素结构](https://geekpics.net/images/2015/06/24/jZGAYCuH.jpg)
那么我们是怎么得到这个结果的呢？

首先，我们在instruments的script界面里面写下前面的代码，然后点击左上角的"record"键运行我们的脚本。
![运行脚本](https://geekpics.net/images/2015/06/24/4D6ofgFez.jpg)
然后，在运行结束以后，我们要切换到trace log界面去，就能看到一开始我们看到的元素结构了。
![tracelog 界面](https://geekpics.net/images/2015/06/24/viVN3J17kR.jpg)
录制脚本
-------
实际上instruments还提供了录制功能，当我们点击下面的红色的录制按钮以后，我们对APP的操作就会被记录下来，然后当我们点击终止键以后，就会在script窗口生成相应的代码。

![录制功能](https://geekpics.net/images/2015/06/24/7mazu5M.jpg)
录制看起来是个很美好的功能，然而就跟大多数具有录制功能的工具一样，录制生成的脚本，很多都是根据坐标来操作的，那么当你换了设备或者是分辨率以后，整个脚本就基本没啥用处了，所以从我的角度来讲，是不推荐大家用录制功能的。
元素定位
-------
对UI自动化来讲，最主要的问题就是定位元素，而对于UIA来说，跟所有的UI自动化工具一样，它只能跟可见的元素进行交互，你如果需要了解哪些元素是可以交互的，logElementTree()就是你的好帮手了。

每个可访问的UI控件都是由一个Javascript对象来代表的。UI控件一般含有几个属性： name, value, elements, parent.整个窗口包含了很多空间，构成了一个层级结构（hierarchy）。于是在UIA当中，所有的元素都是分层的，要访问某个元素，我们就必须从最顶层开始寻找。

- UIATarget 是最上层元素，代表着被测系统的最高等级的用户界面元素，可以是真机，也可以是模拟器上的IOS系统。

- frontMostApp 是UIATaget的下面一层，代表着当前显示在最前的App, 我们可以粗略的认为这就是我们的App。
 
- mainWindow 是frontMostAPP的下面一层，代表着当前正在运行的App的主要界面，可以理解为我们所看到的最前面的窗口。

如下图就是一个大概的结构示例：
![UIhierarchy](https://geekpics.net/images/2015/06/25/6h5H3AZ.jpg)
因为在UIA当中，我们的元素是一个javascript对象，所以我们可以通过以下几种方式来进行定位：

- name 理论上每个元素都有一个name属性，所以我们可以通过name来定位元素。

- index 对于同一类的元素来说，他们的name属性可能是相同的，我们就可以通过index来定位，在UIA当中，index是从0开始计算的，并且是从上往下数的。

比如这个button元素：
![button](https://geekpics.net/images/2015/06/25/e19bLHokj.jpg)
我们可以用以下代码来找到它：

    var target = UIATarget.localTarget();
    //使用name定位
    var button1 = target.frontMostApp().mainWindow().buttons()["Enter"];
    //使用index定位
    var button1 = target.frontMostApp().mainWindow().buttons()[0];
    
在进行定位的时候，所有的元素都需要**从最顶层来一层一层的找**。

### 如何判断我们该用哪个tag来表示要寻找的元素？ ###
通过前面那个图我们可以看出，任何一个元素在element tree里面都是以UIA开头，然后加上这个元素的Tag，比如Button，StaticText等等。那么当我们在代码里面运用的时候，我们就需要剔除UIA这个开头，然后把tag的首字母小写，最后在tag末尾加上一个复数的代表s，因为在UI hierarchy当中，我们打印出来的各个元素理论上都不止一个，我们得到的元素更像是存在一个list里面。一般来讲，这样我们就可以正确在代码里面写出我们需要的元素的tag了，当然凡事也有例外，想要了解更多的元素tag的知识，请点击[这里](https://developer.apple.com/library/ios/documentation/ToolsLanguages/Reference/UIAElementClassReference/index.html#//apple_ref/doc/uid/TP40009903)。

UIA的API解析
-----------
对于UIA来讲，每个element都有一些公共的方法来对他们进行操作，下面我们就来一一解析一下。
### 单击(tap) ###
在UIA当中，单击是通过tap()这个方法来实现的，在我们定位到元素以后，便可以调用这个方法。

    var button1 = target.frontMostApp().mainWindow().buttons();
    button1.tap(); 

### 双击(doubleTap) ###
UIA当中，双击则是通过doubleTap()来实现的：

    var button1 = target.frontMostApp().mainWindow().buttons();
    button1.doubleTap();
### 滚动(scrollToVisible) ###
当一个元素不可见的时候，UIA是不能够操作它的，所以我们就可以用scrollToVisible()来在它的父容器当中滚动，一直到它可见为止，一般来讲这种元素都是在tableview或者webview里面。

    var toptext = target.frontMostApp().mainWindow().staticTexts()["Second View"];
    toptext.scrollToVisible();
这个所谓的可见是指当前在你的荧幕上可见，但是如果我们在模拟器上运行，我们的显示器的分辨率可能会比我们的模拟器的分辨率小，这就导致了我们在模拟器上看到有滚动条，并且不一定能够看到我们所要的元素，然而实际上该元素是可见的，如果这个时候我们使用这个方法，就会报异常。

### 两个手指点击(twoFingerTap) ###
两个手指点击在IOS系统当中一般是展开一个菜单，类似windows的鼠标右键一样的功能，我们就使用twoFingerTap()这个方法来实现。

    var toptext = target.frontMostApp().mainWindow().staticTexts()["Second View"];
    toptext.twoFingerTap();

###捏放(pinch)###
捏放在IOS系统里面一般是来进行放大和缩小的，pinchopen是放大，pinchclose是缩小，分别使用pinchOpenFromToForDuration()和pinchCloseFromToForDuration()来实现。

    UIATarget.localTarget().pinchOpenFromToForDuration({x:20, y:200},{x:300, y:200},2);
    UIATarget.localTarget().pinchCloseFromToForDuration({x:20, y:200}, {x:300, y:200},2);   
后面的参数分别为起始坐标，结束坐标，以及持续时间。

###拖放###
拖放在UIA里面是有两种实现方式，分别为dragFromToForDuration()和flickFromTo()。

    UIATarget.localTarget().dragFromToForDuration({x:160, y:200},{x:160,y:400},1);
    UIATarget.localTarget().flickFromTo({x:160, y:200},{x:160, y:400});
dragFromToForDuration的最后一个参数为持续时间，而flickFromTo则没有这个持续时间，他跟真实的操作差不多，不需要一个持续时间，当你拖放完了，这个操作也就算结束了。

###拿起来不放(touchAndHold)###
按住并持续一段时间在UIA里面是用touchAndHold()来实现的。


    UIATarget.localTarget().touchAndHold(2);
这个参数代表的是持续的时间，单位是秒。
###确认元素是否有效(checkIsValid)###
当我们进行了一系列操作以后，可能会导致元素的状态发生改变，这时候就需要我们先检查一下这个元素是否还可用，我们就可以用checkIsValid()这个方法来实现，同时这个方法返回的是个布尔类型。

###检查元素是否获得了键盘输入(hasKeyboardFocus)###
当我们要检查一个元素里面是否有键盘输入的事件，我们就可以通过hasKeyboardFocus()来实现。这个方法返回的是一个数字，1代表存在，0代表不存在，null表示这个状态不可用，也就是说这个元素是没有这个状态的。

###检查元素是否有效(isValid)###
当我们要检查我们最后一次尝试访问的元素是否存在的时候，我们通过isValid()来实现。当要确定这个元素确实存在的时候，我们用checkIsValid()来替代这个方法。这个方法返回的是一个布尔类型。

###检查元素是否可用(isEnable)###
当元素加载时，会有一个过程，只有当元素enable以后，才可以进行交互，所以我们可以用isEnable()来判断。这个方法返回的也是数字，1代表可用，0代表不可用，null代表这个状态不可用。

###检查元素是否可见(isVisible)###
因为UIA只能与可见的元素交互，所以我们可以用isVisible()来确定元素是否可见。这个方法返回值也是数字，1代表可见，0代表不可见，null代表状态不可用。

###等待元素失效(waitForInvalid)###
有的时候我们需要等到某个元素失效才能进行下一步操作，我们就可以使用waitForInvalid()来实现。这个方法返回的是个布尔值。

###获取元素的label属性###
获取元素的label属性，我们使用label()这个方法。

###获取元素的name属性###
获取元素的name属性，我们使用label()这个方法。

###获取元素的value属性###
获取元素的name属性，我们使用value()这个方法。

###查看一个元素的name是不是某个字段###
要查看一个元素的name属性匹配了特定字符串，可以使用withName()这个方法，里面带参数name。

