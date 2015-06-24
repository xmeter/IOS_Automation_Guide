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
