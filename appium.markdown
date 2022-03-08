# 手机APP自动化
## Appium原理与安装
本教程讲解如何使用Appium进行手机应用的自动化。
学习本课程前，强烈推荐先学习 Selenium Web 自动化课程
### Appium 用途和特点
Appium 是一个移动 App （手机应用）自动化工具。
手机APP 自动化有什么用？
- 自动化完成一些重复性的任务：比如微信客服机器人
- 爬虫：就是通过手机自动化爬取信息。为什么不通过网页、HTTP 爬取呢？有的系统没有网页，也不方便通过HTTP爬取
- 自动化测试：很多企业里面有这样的需求
Appium 自动化方案的特点：
- 开源免费
- 支持多个平台：iOS （苹果）、安卓 App 的自动化都支持。
- 支持多种类型的自动化
	- 支持 苹果、安卓 应用 原生界面 的自动化
	- 支持 应用 内嵌 WebView 的自动化
	- 支持 手机浏览器 中的 web网站自动化
	- 支持 flutter 应用的自动化
- 支持多种编程语言:像 Selenium 一样， 可以用多种编程语言 调用它 开发自动化程序。
### 自动化原理
我们先来看一下Appium自动化的原理图

![image](https://user-images.githubusercontent.com/12490550/157184605-ae896722-6ff4-4ba5-9f00-96bd24768ebf.png)

这图是不是很眼熟？
对啦，和Selenium 原理图很像。因为 Appium自动化架构就是借鉴的Selenium。
大家看看这幅图， 包含了 3个主体部分 ： 自动化程序、Appium Server、移动设备
#### 自动化程序
自动化程序是由我们来开发的，实现具体的 手机自动化 功能。
要发出具体的指令控制手机，也需要使用 客户端库。
和Selenium一样，Appium 组织 也提供了多种编程语言的客户端库，包括 java，python，js， ruby等，方便不同编程语言的开发者使用。
我们需要安装好客户端库，调用这些库，就可以发出自动化指令给手机。
#### Appium Server
Appium Server 是 Appium 组织开发的程序，它负责管理手机自动化环境，并且转发 自动化程序的控制指令 给 手机，并且转发 手机给 自动化程序的响应消息。
#### 手机设备
我们这里说的手机设备，其实不仅仅是手机，包括所有 苹果、安卓的移动设备，比如：手机、平板、智能手表等。
为了直观方便的讲解，这里我们简称： 手机
当然手机上也包含了 我们要自动化控制的 手机应用APP。
手机设备为什么能 接收并且处理自动化指令呢？
因为，Appium Server 会在手机上 安装一个 自动化代理程序， 代理程序会等待自动化指令，并且执行自动化指令
比如：要模拟用户点击界面按钮，Appium 自动化系统的流程是这样的：
1. 自动化程序 调用客户端库相应的函数， 发送 点击元素 的指令（封装在HTTP消息里）给 Appium Server
2. Appium Server 再转发这个指令给 手机上的自动化代理
3. 手机上的自动化代理 接收到 指令后，调用手机平台的自动化库，执行点击操作，返回点击成功的结果给 Appium Server
4. Appium Server 转发给 自动化程序
5. 自动化程序了解到本操作成功后，继续后面的自动化流程
其中，自动化代理控制，使用的什么库来实现自动化的呢？
如果测试的是苹果手机， 用的是苹果的 XCUITest 框架 （IOS9.3版本以后）
如果测试的是安卓手机，用的是安卓的 UIAutomator 框架 (Android4.2以后)
这些自动化框架提供了在手机设备上运行的库，可以让程序调用这些库，像人一样自动化操控设备和APP，比如：点击、滑动，模拟各种按键消息等。
### 自动化环境搭建
本教程主要讲解 安卓APP的自动化。
环境搭建需要下载安装不少软件，而且还有不少是国外网站下载的。
为了方便各位朋友，白月黑羽把这些软件 最新的安装包 都放在如下的百度网盘链接中了，请大家自行下载。
链接：https://pan.baidu.com/s/19C9fGmoXne8DgfXhrTB2TQ
提取码：kgwb
#### 安装client编程库
根据原理图， 我们知道自动化程序需要调用客户端库和 Appium Server 进行通信。
因为我们介绍Python语言开发，所以当然是用pip安装，如下
> pip install appium-python-client
#### 安装Appium Server
Appium Server 是用 nodejs 运行的，基于js开发出来的。
Appium组织为了方便大家安装使用，制作了一个可执行程序 Appium Desktop，把 nodejs 运行环境、Appium Server 和一些工具 打包在里面了，只需要简单的下载安装就可以了。
可以从 上面给出的百度网盘连接 下载安装： Appium-windows-1.15.1.exe
#### 安装JDK
本教程主要讲解 安卓APP的自动化，必须要安装安卓SDK（后面会讲到），而安卓SDK需要 JDK 环境。
可以从 上面给出的百度网盘连接 下载安装： jdk-8u211-windows-x64.exe
安装好之后，还需要添加一个环境变量 JAVA_HOME ，指定 值 为 jdk安装目录，比如
JAVA_HOME   d:\tools\java\jdk1.8.0_211
#### 安装 Android SDK
对于安卓APP的自动化，Appium Server 是需要 Android SDK的。
因为要用到里面的一些工具，比如 要执行命令设置手机、传送文件、安装应用、查看手机界面等。
可以从 上面给出的百度网盘连接 下载最新的 Android SDK文件包： androidsdk.zip ，并且解压，即可。
解压完成后，需要 配置一下 添加一个 环境变量 ANDROID_HOME ，设置值为sdk包解压目录，比如 d:\tools\androidsdk
另外，还推荐大家配置环境变量 PATH ，加入 adb所在目录， d:\tools\androidsdk\platform-tools\
注意：是 添加 该目录到环境变量PATH中， ！！！不是替换！！！ ，否则会导致系统命令都找不到的严重后果，初学者 请对照视频讲解操作。
#### 连接手机
上述的软件环境都准备好以后，要自动化手机APP，需要：
- 在你运行程序的电脑上 用 USB线 连接上 你的安卓手机
- 进入 手机设置 -> 关于手机 ，不断点击 版本号 菜单（7次以上），
- 退出到上级菜单，在开发者模式中，启动USB调试
如果手机连接USB线后，手机界面弹出 类似 如下提示。

![image](https://user-images.githubusercontent.com/12490550/157188212-c7f2121b-3365-4a24-b919-3bd533a6d9b3.png)

选择 允许USB调试。
注意：
有的手机系统，可能需要一些额外的选项需要设置好。
比如，有的手机，开发者选项里 需要打开 允许通过USB安装应用 等。
总之，给USB开发调试 尽可能方便的控制手机。
连接好以后，打开命令行窗口， 执行 `adb devices -l` 命令来列出连接在电脑上的安卓设备。
如果输出 类似如下的内容：
```
List of devices attached
4d0035dc767a50bb        device product:t03gxx model:GT_N7100 device:t03g
```
表示电脑上可以查看到 连接的设备，就可以运行自动化程序了。
#### 一个例子
下面是一段使用 Appium 自动化的打开 B站 应用，搜索 白月黑羽 发布的教程视频，并且打印视频标题的示例。
```
from appium import webdriver
from selenium.webdriver.common.by import By
from appium.webdriver.extensions.android.nativekey import AndroidKey

desired_caps = {
  'platformName': 'Android', # 被测手机是安卓
  'platformVersion': '8', # 手机安卓版本
  'deviceName': 'xxx', # 设备名，安卓手机可以随意填写
  'appPackage': 'tv.danmaku.bili', # 启动APP Package名称
  'appActivity': '.ui.splash.SplashActivity', # 启动Activity名称
  'unicodeKeyboard': True, # 使用自带输入法，输入中文时填True
  'resetKeyboard': True, # 执行完程序恢复原来输入法
  'noReset': True,       # 不要重置App
  'newCommandTimeout': 6000,
  'automationName' : 'UiAutomator2'
  # 'app': r'd:\apk\bili.apk',
}

# 连接Appium Server，初始化自动化环境
driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)

# 设置缺省等待时间
driver.implicitly_wait(5)

# 如果有`青少年保护`界面，点击`我知道了`
iknow = driver.find_elements(By.ID, "text3")
if iknow:
    iknow.click()

# 根据id定位搜索位置框，点击
driver.find_element(By.ID, 'expand_search').click()

# 根据id定位搜索输入框，点击
sbox = driver.find_element(By.ID, 'search_src_text')
sbox.send_keys('白月黑羽')
# 输入回车键，确定搜索
driver.press_keycode(AndroidKey.ENTER)

# 选择（定位）所有视频标题
eles = driver.find_element(By.ID, 'title')

for ele in eles:
    # 打印标题
    print(ele.text)

input('**** Press to quit..')
driver.quit()
```
运行代码前，要先 运行 Appium Desktop
#### Appium 2 的 find_element写法
注意：Appium Python 现在已经升级到 2.x 大版本，依赖 Selenium 4 以后， 下面这种 find_element_by\* 方法都作为过期不赞成的写法
> driver.find_element_by_id('username').send_keys('byhy')
运行会有告警，都要写成下面这种格式
```
from selenium.webdriver.common.by import By
wd.find_element(By.ID, 'username').send_keys('byhy')
```
而后续还有 Appium独有的查找方式，比如
```
driver.find_element_by_accessibility_id('byhy')
driver.find_element_by_android_uiautomator(code)
```
要改成
```
from appium.webdriver.common.appiumby import AppiumBy

driver.find_element(AppiumBy.ACCESSIBILITY_ID, 'byhy')
driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR, code)

# 这样也可以根据ID
wd.find_element(AppiumBy.ID, 'username').send_keys('byhy')
```
##### 没有apk
如果你应用已经安装在手机上了，可以直接打开手机上该应用，进入到你要操作的界面
然后执行
> adb shell dumpsys activity recents | find "intent={"
会显示如下，最近的 几个 activity 信息，
```
intent={act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=tv.danmaku.bili/.ui.splash.SplashActivity}
intent={act=android.intent.action.MAIN cat=[android.intent.category.HOME] flg=0x10000300cmp=com.huawei.android.launcher/.unihome.UniHomeLauncher}
intent={flg=0x10804000 cmp=com.android.systemui/.recents.RecentsActivity bnds=[48,1378][10322746]}
intent={flg=0x10000000 cmp=com.tencent.mm/.ui.LauncherUI}
```
其中第一行就是当前的应用，我们特别关注最后
> cmp=tv.danmaku.bili/.ui.splash.SplashActivity
应用的package名称就是 tv.danmaku.bili
应用的启动Activity就是 .ui.splash.SplashActivity
##### 有apk
如果你已经获取到了 apk，在命令行窗口执行
> d:\tools\androidsdk\build-tools\29.0.3\aapt.exe dump badging d:\tools\apk\bili.apk | find "package: name="
输出信息中，就有应用的package名称
> package: name='tv.danmaku.bili' versionCode='5531000' versionName='5.53.1' platformBuildVersionName='5.53.1' compileSdkVersion='28' compileSdkVersionCodename='9'
在命令行窗口执行
> d:\tools\androidsdk\build-tools\29.0.3\aapt.exe dump badging d:\tools\apk\bili.apk | find "launchable-activity"
输出信息中，就有应用的启动Activity
> launchable-activity: name='tv.danmaku.bili.ui.splash.SplashActivity'  label='' icon=''
