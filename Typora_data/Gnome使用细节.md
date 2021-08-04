## 目录

- [引言](#a1)
- [输入法](#a2)
- [鼠标滚轮](#a3)
- [开机自启](#a4)
- [科学上网](#a5)
	- [插件管理](#a5-b1)
- [耳机问题](#a6)
- [JAVA环境配置](#a7)
	- [JDK 11](#a7-b1)
		- [JRE](#a7-b1-c1)
- [Android Studio](#a8)
	- [镜像列表及使用方法](#a8-b1)
- [VS Code](#a9) 
- [Adobe在Linux中的替代品](#a10)

----

## <span id=a1>引言</span>

作为一个Linux初学者，在第一次接触到[Manjaro（kde）](htts://manjaro.org)时就深深地爱上了这个系统，以至于后来重装系统时选择的也是Manjaro（Gnome），之所以选择后者，是因为Gnome的风格和操作习惯更适合我。这个系统我已经用了有一段时间，有几个比较困扰人，可能大多数人都会遇到的的问题今天一起把它们解决了 ^_^ 。

## <span id=a2>输入法</span>

安装输入法，我这里选用的是`Googlepinyin`。一般现在的系统，在软件管理程序中下载输入法时就包括有相应的依赖包。

![依赖包](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/依赖包.png)

如果没有依赖包或者不全，我们只好手动安装：

```markdown
依赖包
fcitx
fcitx-im
fcitx-cconfigtool
kcm-fictx
libgooglepinyin----或lib（xxxpinyin）
```

输入法和依赖包都安装好后，然后配置环境。我们需要在`/etc/profile`中添加以下内容并关机重启：

```markdown
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

![输入法配置](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/输入法配置.png)

安装好的输入法：

![](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/安装好的输入法.png)

## <span id=a3>鼠标滚轮</span>

新系统装好后可能会遇到滚轮速度过慢的问题，不解决的话真的很影响日常使用。我们先下载`imwheel`这个程序，然后在` ~/.imwheelrc`中添加程序设置（注：在vim中，“Esc”退出，“i”插入），如下：

```markdown
".*"
None,      Up,      Button4, 2
None,      Down,    Button5, 2
Control_L, Up,      Control_L|Button4
Control_L, Down,    Control_L|Button5
Shift_L,   Up,      Shift_L|Button4
Shift_L,   Down,    Shift_L|Button5
None,      Thumb1,  Alt_L|Left
None,      Thumb2,  Alt_L|Right
————————————————
- 第二行和第三行中的“2”代表滚动的行数，可以根据需要来修改这个配置。
- 首行中“.*”用来指定在哪些应用中生效，“.*”表示全部应用生效，可以执行“man imwheel”查看更多帮助信息。
- 第四行和第五行可以让鼠标支持“左ctrl+上下滚动”（比如在浏览器中支持放大）。
- 第六行和第七行可以让鼠标支持“左shift+上下滚动”（不同应用有不同作用），实测似乎并没有什么卵用。
- 最后两行用来开启鼠标侧键功能。
————————————————
版权声明：本文为CSDN博主「15wylu」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_32767041/article/details/84034280
```

## <span id=a4>开机自启</span>

上面的`imwheel`安装好后还要设置开机自启，也有一些其他软件需要设置，比如`IDM`，`Qv2ray`等。有的Linux发行版可以直接在系统设置里面添加，我的Gnome也可以，但是却只能添加完整的应用软件，不能是单独的程序。所以针对Gnome，我们可以在`gnome-session-properties`中设置，如果没有则需要事先下载。
下载好后，打开就是这样。在添加程序时，若不知道安装位置，可以使用`whereis “name”`指令查看。

![设置界面](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/开机自启.png)

## <span id=a5>科学上网</span>

在Linux中如何科学上网这个问题困扰了我许久，解决后才知道如此简单（~~菜是原罪~~）。代理软件我用的是Qv2ray（好用不多说），我们需要下载两个东西，[v2ray-core](https://github.com/v2fly/v2ray-core)和[Qv2ray](https://github.com/Qv2ray/Qv2ray)。

![v2ray-core](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/v2ray-core.png)

![Qv2ray](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/qv2ray-appimage.png)

下载好后，我们先解压v2ray-core，然后打开Qv2ay（若无法打开，则右键属性修改权限，允许执行文件），我们现设置“首选项”，在“内核设置”里，修改“可执行文件路径”和“资源目录”。

“可执行文件路径”，点击右边按钮选择刚刚解压的文件，找到“v2ray”，点击确定。

![文件路径](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/文件路径.png)

而“资源分配”则直接选择总的解压文件，然后在“连接设置”里把“绕过中国大陆IP”勾选上，其它设置就不用管了。

![Qv2ray首选项](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/Qv2ray首选项.png)

因为我个人使用但是机场订阅链接，所以这里主要讲订阅链接的使用方法。

![Qv2ray](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/Qv2ray.png)

首先点击左下角的“分组”，在“组编辑器”里选择“订阅设置”，勾选下方的“此分组是一个订阅”并输入订阅链接，最后点击下方的“更新订阅”就可以确定并退出。

![](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.0/Gnome使用细节/添加订阅.png)

当然你可以选择手动配置，方法是点击左下角“新建”，选择“手动输入”，再点击下方的“打开连接编辑器”即可手动配置。

双击机场节点，并在设置里选择“系统代理”即可正常使用。

### <span id=a5-b1>插件管理</span>

在Qv2ray的左上角还有一个“插件”选项，点进去后我们可以在这里添加一些常用插件，比如SSR，Trojan，NaiveProxy等插件，可以让我们使用其它格式的订阅链接。

注：插件同样也要赋予可执行权限。

- [QvPlugin-SSR](https://github.com/Qv2ray/QvPlugin-NaiveProxy)
- [QvPlugin-SS](https://github.com/Qv2ray/QvPlugin-SS)
- [QvPlugin-NaiveProxy](https://github.com/Qv2ray/QvPlugin-NaiveProxy)
- [QvPlugin-Trojan](https://github.com/Qv2ray/QvPlugin-Trojan)
- [QvPlugin-Trojan-Go](https://github.com/Qv2ray/QvPlugin-Trojan-Go)

## <span id=a6>耳机问题</span>

其实在第一次使用Manjaro时就遇到耳机（有线）无法识别的问题，但并不影响，因为可以在“音量控制”里找到“Headphones”选项，虽然显示“（已拔出）”但可以正常使用。

这次在Gnome上也遇到这个问题，不过并不是像上面一样，而是根本无法识别出耳机。

![Headphones](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/Headhphones.png)

针对这种情况，我们需要下载另一个音量控制程序——“PulseAudio”。

![PulseAudio](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/PulseAudio.png)

安装好后，我们在”端口“选项中就可以找到”Headphones“，显示”（已拔出）“但不影响。

![音量控制](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.1/Gnome使用细节/耳机问题/音量控制.png)

现在，可以正常使用你的耳机啦。

**补充**：用命令行修复输出设备无法识别，在终端里输入以下命令后并重启计算机。

```
- echo "options snd-intel-dspcfg dsp_driver=1" > /etc/modprobe.d/alsa.conf
```

## <span id=a7>JAVA环境配置</span>

### <span id=a7-b1>JDK 11</span>

JAVA现在普遍用的版本是11，而最常用的是Oracle家的jdk11，所以我也下载的是这个，[点击下载](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)。

根据下方图片所示完成下载后，我们把压缩包解压到目录`/usr/java`。

![JDK下载](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/jdk11下载.png)

解压好后，我们就可以在`/etc/profile`中修改环境变量，添加以下内容：

```markdown
export JAVA_HOME=/usr/java/xxx-----XXX为你使用的jdk

export JRE_HOME=${JAVA_HOME}/jre

export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH

export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin

export PATH=$PATH:${JAVA_PATH}
```
最后重启电脑以生效。

> 出处：https://www.jianshu.com/p/12493db8a701

#### <span id=a7-b1-c1>JRE</span>

jdk9以后的版本不再自动生成jre，这时我们需要手动生成。如果java目录中没有jre，那么上面环境变量中的jre并不会生效。接下来的操作很简单，我们在终端进入到`/usr/java/XXX`，然后执行以下指令：

```
bin/jlink --module-path jmods --add-modules java.desktop --output jre
```

执行成功后，我们可以看到目录中已经有了jre。

![jre](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/JRE.png)

> 出处：https://blog.csdn.net/y_qc_lookup/article/details/99948136

## <span id=a8>Android Studio</span>

AS可以直接从Arch的仓库中下载安装，十分方便。

![as](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/android%20studio.png)

安装好后，我们在使用AS时遇到的最大问题就是SDK，AVD，以及gradle。但是，这些问题都是可以通过科学上网直接解决的，或者使用国内镜像代理。

### <span id=a8-b1>镜像列表及使用方法</span>


- 镜像列表

    <details><summary>南阳理工学院镜像服务器地址：</summary>
		- mirror.nyist.edu.cn 端口:80</details>

    <details><summary>中国科学院开源协会镜像站地址：</summary>
		- IPV4/IPV6: mirrors.opencas.cn 端口:80
		- IPV4/IPV6: mirrors.opencas.org 端口:80
		- IPV4/IPV6: mirrors.opencas.ac.cn 端口:80</details>

    <details><summary>上海GDG镜像服务器地址：</summary>
		- IPV4/IPV6: mirrors.opencas.ac.cn 端口:80</details>

    <details><summary>北京化工大学镜像服务器地址：</summary>
		- IPv4: ubuntu.buct.edu.cn/ 端口:80
		- IPv4: ubuntu.buct.cn/ 端口:80
		- IPv6: ubuntu.buct6.edu.cn/ 端口:80</details>

    <details><summary>大连东软信息学院镜像服务器地址：</summary>
		- mirrors.neusoft.edu.cn 端口:80</details>

    <details><summary>腾讯Bugly镜像：</summary>
		- android-mirror.bugly.qq.com 端口:8080
		- [腾讯镜像使用方法](http://android-mirror.bugly.qq.com:8080/include/usage.html)</details>


- 使用方法

    1. 启动Android Studio，打开主界面，依次选择“Configure”，“System Settings”，弹出“HTTP Proxy”窗口；
    2. 在“HTTP Proxy”窗口中，选择“Manual Proxy Configuration”，勾选“HTTP”，在“Host name”和“Port number”输入框内填入上面镜像服务器地址（**不包含http:// **，如下图）和端口，设置完成后单击“Check connection”按钮，最后关闭设置窗口返回到主界面；
    3. 依次选择“Packages”，“Reload”。

![configure](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/configure.png)

![http settings](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/http-settings.png)

> 出处：https://www.cnblogs.com/pingxin/p/p00078.html

## <span id=a9>VS Code</span>

最开始我并不知道Arch的仓库中有[VS Code](https://aur.archlinux.org/packages/visual-studio-code-bin)，我是根据网上的这篇[帖子](https://www.cnblogs.com/kwinwei/p/13202174.html)完成安装，虽然有点多此一举，但是也给其它Linux发行版提供了参考，下面是安装流程：

首先我们去[官网](https://code.visualstudio.com/download)下载最新的VS Code，下载时注意你所选择的文件格式以及架构。

![download vs](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/download%20VS.png)

下载完后，我们把压缩包解压到文件目录`/usr/vs_code`，此时双击“code”就已经可以运行了（可能需要给予执行权限），但是我们还是得创建一个快捷方式。

在程序根目录下，我们可以在`/resources/app/resources/linux/`这个路径中找到VS的图标“code.png”，然后把这个图标复制到`/usr/share/icons`目录下，由于在文件夹中无法直接执行这一操作，所以我们在终端中用以下指令：

```
sudo cp /usr/vs_code/VSCode-linux-x64/resources/app/resources/linux/code.png /usr/share/icons/
```

最后我们就可以在`/usr/share/applications/`目录下创建VS的快捷方式。在终端中，用编辑器创建`/usr/share/applications/VSCode.desktop`，然后在文本框中输入以下内容：

```
[Desktop Entry]
Name=Visual Studio Code
Comment=Multi-platform code editor for Linux
Exec=/usr/local/VSCode-linux-x64/code
Icon=/usr/share/icons/code.png
Type=Application
StartupNotify=true
Categories=TextEditor;Development;Utility;
MimeType=text/plain;
```

保存后退出，我们在应用菜单里就可以看到VS Code了。打开VS Code，加载插件：vscode-icons。

![插件](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.1.3/Gnome使用细节/软件安装/icons.png)

## <span id=a10>Adobe在Linux中的替代品</span>

Photoshop
- GIMP: https://www.gimp.org/
- Krita: https://krita.org/en/
- Photopea: https://www.photopea.com/

Lightroom
- Darktable: https://www.darktable.org/
- RawTherapee: https://rawtherapee.com/

Illustrator
- Inkscape: https://inkscape.org/
- Vectr: https://vectr.com/

InDesign
- Scribus: https://www.scribus.net/
- Lucidoress: https://www.lucidpress.com/pages/

Premiere Pro
- Blender: https://www.blender.org/

After Effects
- Natron: https://natrongithub.github.io/

Audition
- Audacity: https://www.audacityteam.org/

Dreamweaver
- BlueGriffon: http://bluegriffon.org/

----

> 后续若有其它内容，我再一一补充 ^_^ 。

<center>注：本文部分内容整理自互联网，文中已标明出处。</center>
