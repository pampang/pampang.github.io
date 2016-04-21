# Windows下安装React Native的安卓环境 #
---

###目录
1. 安装Java SDK
1. 安装Android SDK
1. 安装C++环境
2. 安装python环境
2. （可选）安装Android Studio（安卓编辑环境）
3. 安装react-native-cli
4. 初始化我的第一个React Native项目
5. 运行我的React Native项目
6. 安装安卓虚拟机
6. 连接我的真机设备
7. 在我的虚拟机或者真机上运行项目
8. P.S.

## 1、安装Java SDK

这个可以自行谷歌教程，安装之后记得配置**环境变量**即可。
附官方下载地址：http://www.oracle.com/technetwork/java/javase/downloads/index.html

####注意：
1. 需要配置环境变量，PATH
1. 需要配置JAVA_HOME环境变量，这个需要自行创建，RN在编译时需要这个变量。

## 2、安装Android SDK

通过http://androiddevtools.cn/ 下载Android SDK（往下拉，找到Android SDK，然后下载）。
这是国内的环境，不需要翻墙，下载更加迅速。
下载之后是一个RAR，直接解压到自己想要的目录下即可。

然后进入到该文件夹下面的SDK Manager，确保以下项目已经安装并更新到最新：

> Tools/Android SDK Tools (24.3.3)
> 
> Tools/Android SDK Platform-tools (22)
> 
> Tools/Android SDK Build-tools (23.0.1)
> 
> Android 6.0 (API 23)/SDK Platform (1)
> 
> Extras/Android Support Library(23.0.1)

推荐使用代理来安装。在http://androiddevtools.cn/ 最上方边有很详细的代理配置方法。

####注意：
1. 需要配置环境变量，PATH
2. 还要将其文件夹下面的tools路径配置到环境变量中。**具体建议百度，大把教程。**
1. 需要配置ANDROID_HOME环境变量，这个需要自行创建，RN在编译时需要这个变量。

##3、安装C++环境

直接安装Visual Studio 2013/2015即可将整个C++环境都搞定。
推荐下载地址：http://tieba.baidu.com/p/4102221157

##4、安装Python环境

推荐python v2.7。直接下载并安装即可。
http://lihuipeng.blog.51cto.com/3064864/850562

##5、（可选）安装Android Studio

这个是编写android很好的集成IDE，类似Visual Studio等。但是我不建议使用，因为太复杂了，我也不会用。。。
直接使用SDK Manager等便可很好的完成任务，无须用到AS。

在http://androiddevtools.cn/ 下即可安装到Android Studio了。下载下来是一个RAR，直接解压到想要放的文件夹内，即可使用了。

##6、安装react-native-cli

没理由没用过NodeJS吧？
直接一句命令：

```javascript
    npm install -g react-native-cli
```
然后`react native`这个命令就可以全局使用啦。

##7、初始化我的第一个React Native项目

同样也是一句命令：

```javascript
	react-native init AwesomeProject
```
然后一个名为AwesomeProject的RN项目就会在你命令行的当前文件夹下面创建。
在创建过程中需要下载东西，例如React-Native等依赖，因此会很慢。
并未确定，但我估计很大概率需要翻墙环境。

##8、运行我的React Native项目

命令行切到AwesomeProject/目录下，直接运行：

```
	react-native start
	// 有些项目配置了npm script, 依照npm script也可以运行。
```
RN的环境就运行起来了。
但是你并没有一个机器来运行。所以我们需要安装一个虚拟机，或者连接我们的真机，来运行我们的RN项目。

## 9、安装安卓虚拟机

尝试过安卓Android SDK中自带的adb模拟器，非常烂，不建议使用。
通过android avd来启动：http://jingyan.baidu.com/article/066074d64d743cc3c21cb02b.html

同时也尝试过几款虚拟机，包括夜神模拟器,genymotion模拟器，最终亲测genymotion模拟器可以完美运行。

### Genymotion模拟器
---

首先需要安装genymotion。
我们需要注册一个账号之后，才能够在官网下载该软件。这个账号也是我们后来部署单一虚拟机的入口。因此必须注册一个。

注册之后，genymotion有两个版本，包括集成了Virtual Box的版本和standalone版本。

**在这里，建议安装standalone版本，同时在外部单独下载一个virtual box配合使用。**

OK，下载安装之后，我们就可以部署我们的设备了。点击'Add'按钮即可添加自己的虚拟机设备。

> PS：有些同学会在运行的时候觉得十分卡顿，这里有一个教程：
> http://www.cnblogs.com/JohnTsai/p/4132252.html
> **勾选3D加速**之后，虚拟机的速度会有一个很大的提升，建议设置。

目前遇到一个问题，就是genymotion跟我们的VPN会有网络冲突。尤其是在我使用AnyConnect开启我的VPN网络时，由于AnyConnect的内部实现原因，会导致整个genymotion不可用，虚拟机设备无法开启。

查了一大堆资料，没什么结果。最终将AnyConnect切换为OpenConnect之后，问题解决了。
**推荐直接使用OpenConnect开启VPN。**


##10、连接我的真机设备

OK，到了使用我们手上真机的时刻了。
首先进入到设备的开发者选项中，勾选“USB调试”选项。
确认安装好手机相应的驱动之后，在命令行中输入：
```
	adb devices
```
这时候，会有一个已连接的设备列表。

```
    List of devices attached
	// 虚拟机设备
    192.168.56.101:5555 device
	// 真实设备
    14ed2fcc device
```

如果没有发现自己连接上去的设备，我们尝试着将adb服务关闭之后再次重启。

```
	// 关闭adb
	adb kill-server
	// 开启adb
	adb start-server
	// 查看已连接的设备
	adb devices
```

这时候手机应该会弹出一个窗，说是否运行连接这台手机调试。**必须选是**！

OK，这时候我们的设备已经连接成功了。

##11、在我的虚拟机或者真机上运行项目

将命令行切到项目的根目录下，现在一个终端下运行：
```
	// 打开RN服务器
	react-native start
```
运行
```
	// 将项目注入到设备中
	react-native run-androi
```
RN就会帮我们编译项目，并将打包出来的apk安装到我们的设备中。
一般来说，genymotion创建出来的虚拟机设备是没有任何问题的。
但是我手上的魅族魅蓝metal设备则出现了无法安装的问题。

经过大量查阅，发现
http://csbun.github.io/blog/2015/12/starting-react-native-with-android/
这一文章中提出到一个方法：
```
	//  android/build.gradle 第 8 行的版本号改成 1.2.3 即可
	
	buildscript {
	    repositories {
	        jcenter()
	    }
	    dependencies {
	        classpath 'com.android.tools.build:gradle:1.2.3'
	
	        // NOTE: Do not place your application dependencies here; they belong
	        // in the individual module build.gradle files
	    }
	}
```

曾经尝试，至少能够安装apk，但是metal下依然白屏，失败。

对于真机设备，当安装完成之后，一开始进入到app里面，是会出现红色的报错界面的。
进入就对啦，说明app在你的手机里面起效了~

摇晃手机，进入到dev菜单。点击最下面的dev settings。
将手机与电脑处于同一个WIFI下，在命令行输入ipconfig找到自己当前WIFI下的IPv4地址，配上端口8081,输入到设备的"Debug server host & port for device"中。

之后再摇晃手机，点击Reload JS，就可以从RN服务器中获取最新的代码啦！

希望这个方法能够对你起效^_^

更多的信息，推荐你阅读：http://csbun.github.io/blog/2015/12/starting-react-native-with-android/

好的，到现在我们已经将自己的项目在android环境下跑起来啦！尽情的开发吧！

## P.S.

在window下，我们可以在虚拟机里面按**"F1"**打开可选菜单，包括:
- debug in chrome
- enable hot reloading
- 。。。

打包签名可发布的安卓APK,首先需要签名，然后运行命令打包。
建议阅读：http://reactnative.cn/docs/0.24/signed-apk-android.html

等等非常有用的选项。
希望你们能够好好运用。^_^