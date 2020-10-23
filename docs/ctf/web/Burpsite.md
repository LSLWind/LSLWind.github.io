## 简述

Burp Suite 是用于攻击web 应用程序的集成平台，包含了许多工具。Burp Suite为这些工具设计了许多接口，以加快攻击应用程序的过程。所有工具都共享一个请求，并能处理对应的HTTP 消息、持久性、认证、代理、日志、警报。

### 安装

### 破解安装

使用需java环境，下载链接：链接：https://pan.baidu.com/s/1RtuMgPANqjN8VpRxiuhvrg 提取码：baim 

安装：

<img src="https://oss.imangie.com/webimangie/20200530170309.png" style="zoom:67%;" />

<img src="https://oss.imangie.com/webimangie/20200530170405.png" style="zoom:67%;" />

<img src="https://oss.imangie.com/webimangie/20200530170539.png" style="zoom:67%;" />

<img src="https://oss.imangie.com/webimangie/20200530171105.png" style="zoom:67%;" />

<img src="https://oss.imangie.com/webimangie/20200530171326.png" style="zoom:67%;" />

<img src="https://oss.imangie.com/webimangie/20200530171432.png" style="zoom:67%;" />

打开Run_Chinese.bat文件 弹出的CMD命令不能关闭，关闭就是关闭软件

* 将keygen与burpsite放于同一目录下，点击Keygen中的run，会自动调用同一目录下的burpsite
* java版本冲突时可直接进入控制面板卸载
* 使用中文版可能会报错，导致某些选项无法显示，建议使用英文版

参考：[传送门](https://www.imangie.com/goods/619.html)

### 调整字体/风格

进入后选择Options→Display/用户选项→显示，可以选择字体（15即可）与风格（Windows）

### 脚本运行BurpSite

以bat脚本运行

```bash
java -Dfile.encoding=utf-8 -Xbootclasspath/p:burp-loader-keygen.jar -Xmx1024m -jar burpsuite_pro_v2.0.11.jar  pause exit
```

## Proxt模块

Proxy代理模块作为BurpSuite的核心功能，拦截HTTP/S的代理服务器，作为一个在浏览器和目标应用程序之间的中间人，允许你拦截，查看，修改在两个方向上的原始数据流。

首先需要在Target中设置代理服务器，默认为127.0.0.1:8080，之后在Win10中开启代理，此时任意Http请求都会被Burpsite所代理。

**操作**

* Forward：当你编辑信息之后，发送信息到服务器或浏览器
* Drop：当你不想要发送这次信息可以点击drop放弃这个拦截信息
* Interception is on/off：这个按钮用来切换和关闭所有拦截。如果按钮显示Interception is On，表示请求和响应将被拦截或自动转发根据配置的拦截规则配置代理选项。如果按钮显示Interception is off则显示拦截之后的所有信息将自动转发。
* Action说明一个菜单可用的动作行为操作可以有哪些操作功能。

**消息类型**

* raw：显示纯文本形式的消息。在文本窗口的底部提供了一个搜索和加亮功能，可以用它来快速地定位出消息中的感兴趣的字符串，如错误消息。在搜索的左边有一个弹出项，让你来处理大小写问题，以及是使用简单的文本搜索还是正则表达搜索。
* params：对包含参数(URL 查询字符串，cookies 消息头，或消息体)的请求，这个选项可以把参数分析成名称/值的组合，并且允许你能简单地查看和修改。
* headers：这里以名称/值的组合来显示 HTTP 的消息头，并且还以原始的形式显示消息体。
* hex：这里允许你直接编辑消息的原始二进制数据。如果在文本编辑器里修改，某些传输类型(例如，使用 MIME 编码的浏览器请求的部分)包含的二进制数据可能被损坏。为了修改这些类型的消息，应使用十六进制。

## intruder模块

重发器：**用于自动对Web应用程序自定义的攻击。它可以用来自动执行您的测试过程中可能出现的所有类型的任务。例如目录爆破，注入，密码爆破等。**

启用Proxy拦截报文后发往intruder进行重发

**1.Target（目标）设置**

* Host：目标服务器的IP地址或主机名。
* Port是目标服务的端口号。
* Use HTTPS：指定的SSL是否应该被使用。

**2.Positions设置**

**attack type：攻击模式设置**

* sniper：对变量依次进行破解。多个标记依次进行。单参数爆破，多参数时同一个字典按顺序替换各参数，总之初始值有一个参数不变。
* battering ram：对变量同时进行破解。多个标记同时进行。多参数同时爆破，但用的是同一个字典。
* pitchfork：每一个变量标记对应一个字典，取每个字典的对应项。 多参数同时爆破，但用的是不同的字典。
* cluster bomb：每个变量对应一个字典，并且进行交集破解，尝试各种组合。适用于用户名+密码的破解。多参数做笛卡尔乘积模式爆破。

进入intruder后，burp会默认将所有可能需要爆破的量都用$做标记，clear清除所有标记，选中区域点击add确定标记

**3.Payloads配置字典**

Load加载字典文件

爆破结束后我们主要看的是页面中的length（双击排序），不一样的就是成功登陆的

## Repeater模块

Burp Repeater（中继器） 是一个手动修改并补发个别 HTTP 请求，并分析他们的响应的工具。它最大的用途就是和其他 Burp Suite 工具结合起来。你可以从目标站点地图，从 Burp Proxy 浏览记录，或者从 Burp Intruder 攻击结果上的请求，发送到 Repeater 上，并手动调整这个请求来微调对漏洞的探测或攻击。