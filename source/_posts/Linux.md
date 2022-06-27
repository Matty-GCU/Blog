---
abbrlink: b30ceb9f
title: Linux学习笔记
date: 2022-05-31
updated: 
categories: Linux
tags: Linux
---
# Linux学习笔记

## 前言

### 基于教程

[3天搞定Linux，1天搞定Shell，清华学神带你通关（2022版）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1WY4y1H7d3)

（视频发布时间是2022-04-07，实际内容也非常新，大概是2022年3月份录制的）

### 参考教程

[Linux学习教程，Linux入门教程（超详细） - C语言中文网](http://c.biancheng.net/linux_tutorial/)

[Shell脚本：Linux Shell脚本学习指南（超详细） - C语言中文网](http://c.biancheng.net/shell/)

[Linux 教程 | 菜鸟教程](https://www.runoob.com/linux/linux-tutorial.html)

[Linux 中国◆开源社区](https://linux.cn/)

[Linux命令大全(手册) – 真正好用的Linux命令在线查询网站](https://www.linuxcool.com/)

### 环境版本

* CentOS 7.9.2009
* VMware Workstation 16 Pro 16.2.2

### 官方网站

[The CentOS Project](https://www.centos.org/)

## 1. 入门篇

### 1.1 Linux概述

#### 1.1.1 Linux是什么

<img src="Linux/狭义的Linux和广义的Linux.png" alt="狭义的Linux和广义的Linux" style="zoom:50%;" />

我们作为后端开发/运维人员，使用Linux时最经常接触的当然就是Shell层。

进入Shell有两种方式，一个是原生的Shell入口，即控制台模式下的**Linux控制台(Console)** ；另一个是桌面环境封装好的模拟入口（即图形界面模式下的**终端(Terminal)** 。

> 在 Linux 发展的早期，唯一能用的工具就是 Shell，Linux 用户都是在 Shell 中输入文本命令，并查看文本输出；如果有必要的话，Shell 也能显示一些基本的图形。
>
> 而如今 Linux 的环境已经完全不同，<u>几乎所有的 Linux 发行版都使用某种图形桌面环境（例如 GNOME、KDE、Unity 等），这使得原生的 Shell 入口被隐藏了</u>，进入 Shell 仿佛变得困难起来。
>
> 1.
>
> 一种进入 Shell 的方法是让 Linux 系统退出图形界面模式，进入控制台模式，这样一来，显示器上只有一个简单的带着白色文字的“黑屏”，就像图形界面出现之前的样子。这种模式称为 Linux 控制台（Console）。
>
> <u>现代 Linux 系统在启动时会自动创建几个虚拟控制台（Virtual Console），其中一个供图形桌面程序使用，其他的保留原生控制台的样子。虚拟控制台其实就是 Linux 系统内存中运行的虚拟终端（Virtual Terminal）。</u>
>
> 从图形界面模式进入控制台模式也很简单，往往按下`Ctrl + Alt + Fn(n=1,2,3,4,5...)`快捷键就能够来回切换。
>
> 例如，CentOS 在启动时会创建 6 个虚拟控制台，按下快捷键`Ctrl + Alt + Fn(n=2,3,4,5,6)`可以从图形界面模式切换到控制台模式，按下`Ctrl + Alt + F1`可以从控制台模式再切换回图形界面模式。也就是说，1 号控制台被图形桌面程序占用了。
>
> 2.
>
> 进入 Shell 的另外一种方法是使用 Linux 桌面环境中的终端模拟包（Terminal emulation package），也就是我们常说的终端（Terminal），这样在图形桌面中就可以使用 Shell。
>
> 以 CentOS 为例，可以在“应用程序”菜单中找到终端，打开终端后，就可以输入 Shell 命令了。
>
> CentOS 默认的图形界面程序是 GNOME，该终端模拟包也是 GNOME 自带的。
>
> 参考资料：[进入Shell的两种方式](http://c.biancheng.net/view/733.html)

#### 1.1.2 Linux的发行版

<img src="Linux/Linux的发行版.png" alt="Linux的发行版" style="zoom:50%;" />

#### 1.1.4 Linux vs Windows

![Windows.vs.Linux](Linux/Windows.vs.Linux.png)

### 1.2 Linux安装

CentOS官网的下载页面：https://www.centos.org

**CentOS Linux**（有别于CentOS Stream）当前最新版本是7-2009，即7.9.2009：http://isoredirect.centos.org/centos/7/isos/x86_64/

VMWare官网：https://www.vmware.com/cn.html

**VMware Workstation Pro**当前最新版本是16.2.3：https://customerconnect.vmware.com/cn/downloads/details?downloadGroup=WKST-1623-WIN-NEW&productId=1038&rPId=85389

## 2. 基础篇

### 2.1 Linux文件系统

在Linux中，一切皆文件。

下面的截图是我的Linux虚拟机的根目录结构，总共有19个一级目录，接下来我们全都过一遍。

![Linux根目录结构](Linux/Linux根目录结构.png)

> ### 目录
>
> #### /bin (binary)
>
> <u>`/bin` 目录是包含一些二进制文件的目录，即可以运行的一些应用程序。</u> 你会在这个目录中找到上面提到的 `ls` 程序，以及用于新建和删除文件和目录、移动它们基本工具。还有其它一些程序，等等。文件系统树的其他部分有更多的 *bin* 目录，但我们将在一会儿讨论这些目录。
>
> #### /sbin (system binary)
>
> <u>`/sbin` 与 `/bin` 类似，但它包含的应用程序只有超级用户（即首字母的 `s` ）才需要。</u>你可以使用 `sudo` 命令使用这些应用程序，该命令暂时允许你在许多 Linux 发行版上拥有超级用户权限。`/sbin` 目录通常包含可以安装、删除和格式化各种东西的工具。你可以想象，如果你使用不当，这些指令中有一些是致命的，所以要小心处理。
>
> #### /usr (unix software resource)
>
> `/usr` 目录是在 UNIX 早期用户的主目录所处的地方。然而，正如我们上面看到的，现在 `/home` 是用户保存他们的东西的地方。如今，`/usr` 包含了大量目录，而这些目录又包含了应用程序、库、文档、壁纸、图标和许多其他需要应用程序和服务共享的内容。
>
> <u>你还可以在 `/usr` 目录下找到 `bin`，`sbin`，`lib` 目录，它们与挂载到根目录下的那些有什么区别呢？现在的区别不是很大。</u>在早期，`/bin` 目录（挂载在根目录下的）只会包含一些基本的命令，例如 `ls`、`mv` 和 `rm` ；这是一些在安装系统的时候就会预装的一些命令，用于维护系统的一个基本的命令。 而 `/usr/bin` 目录则包含了用户自己安装和用于工作的软件，例如文字处理器，浏览器和一些其他的软件。
>
> <u>但是许多现代的 Linux 发行版只是把所有的东西都放到 `/usr/bin` 中，并让 `/bin` 指向 `/usr/bin`，以防彻底删除它会破坏某些东西。</u>因此，Debian、Ubuntu 和 Mint 仍然保持 `/bin` 和 `/usr/bin` （和 `/sbin` 和 `/usr/sbin` ）分离；其他的，比如 Arch 和它衍生版，只是有一个“真实”存储二进制程序的目录，`/usr/bin`，其余的任何 `bin` 目录是指向 `/usr/`bin` 的“假”目录。
>
> #### /lib (library)
>
> `/lib` 是库文件所在的地方。库是包含应用程序可以使用的代码文件。它们包含应用程序用于在桌面上绘制窗口、控制外围设备或将文件发送到硬盘的代码片段。
>
> 在文件系统周围散布着更多的 `lib` 目录，但是这个直接挂载在 `/` 的 `/lib` 目录是特殊的，除此之外，它包含了所有重要的内核模块。 内核模块是使你的显卡、声卡、WiFi、打印机等工作的驱动程序。
>
> #### /boot
>
> `/boot` 目录包含启动系统所需的文件。我必须要说吗？ 好吧，我会说：**不要动它**！ 如果你在这里弄乱了其中一个文件，你可能无法运行你的 Linux，修复被破坏的系统是非常痛苦的一件事。 另一方面，不要太担心无意中破坏系统：你必须拥有超级用户权限才能执行此操作。
>
> > ***Boot* 存放开机文件，一般为100M左右（50M~200M）。**
>
> #### /dev (device)
>
> `/dev` 目录包含设备文件。 其中许多是在启动时或甚至在运行时生成的。 例如，如果你将新的网络摄像头或 USB 随身碟连接到你的机器中，则会自动弹出一个新的设备条目。
>
> #### /etc (everything to configure)
>
> `/etc` 的目录名称会让人变得非常的困惑。`/etc` 得名于最早的 Unix 系统们，它的字面意思是 “etcetera”（诸如此类） ，因为它是系统文件管理员不确定在哪里放置的文件的垃圾场。
>
> 现在，说 `/etc` 是“要配置的所有内容Everything To Configure”更为恰当，因为它包含大部分（如果不是全部的话）的系统配置文件。 例如，包含系统名称、用户及其密码、网络上计算机名称以及硬盘上分区的安装位置和时间的文件都在这里。 再说一遍，如果你是 Linux 的新手，最好是不要在这里接触太多，直到你对系统的工作有更好的理解。
>
> #### /home
>
> `/home` 是你可以找到用户个人目录的地方。在我的情况下，`/home` 下有两个目录：`/home/paul`，其中包含我所有的东西；另外一个目录是 `/home/guest` 目录，以防有客人需要使用我的电脑。
>
> #### /root
>
> `/root` 是系统的超级用户（也称为“管理员”）的主目录。 它与其他用户的主目录是分开的，**因为你不应该动它**。 所以把自己的东西放在你自己的目录中，伙计们。
>
> #### /media
>
> 在 `/media` 目录中，当你插入外部存储器试图访问它时，将自动挂载它。与此列表中的大多数其他项目不同，`/media` 并不追溯到 1970 年代，主要是因为当计算机正在运行而动态地插入和检测存储（U 盘、USB 硬盘、SD 卡、外部 SSD 等)，这是近些年才发生的事。
>
> > *Media* 一般是**系统自动挂载**可移除的装置，挂载后装置图标**会**出现在桌面窗口的左边栏。如软碟、光碟、DVD、U盘、移动硬盘（/media/floppy, /media/cdrom）。
>
> #### /mnt (mount)
>
> 然而，`/mnt` 目录是一些过去的残余。这是你手动挂载存储设备或分区的地方。现在不常用了。
>
> > *Mount* 一般是用于让**用户自己挂载**其他文件系统，挂载后装置图标**不会**出现在桌面窗口的左边栏。
>
> #### /opt (optional)
>
> `/opt` 目录通常是你编译软件（即，你从源代码构建，并不是从你的系统的软件库中安装软件）的地方。应用程序最终会出现在 `/opt/bin` 目录，库会在 `/opt/lib` 目录中出现。
>
> 稍微的题外话：应用程序和库的另一个地方是 `/usr/local`，在这里安装软件时，也会有 `/usr/local/bin` 和 `/usr/local/lib` 目录。开发人员如何配置文件来控制编译和安装过程，这就决定了软件安装到哪个地方。
>
> #### /proc (process)
>
> `/proc`，就像 `/dev` 是一个虚拟目录。它包含有关你的计算机的信息，例如关于你的 CPU 和你的 Linux 系统正在运行的内核的信息。与 `/dev` 一样，文件和目录是在计算机启动或运行时生成的，因为你的系统正在运行且会发生变化。
>
> > *Processes* 正在运行的内核信息映射，主要包括进程信息、内存资源信息、磁盘分区信息等。（虚拟文件系统，不占内存）
>
> #### /run
>
> `/run` 是另一个新出现的目录。系统进程出于自己不可告人的原因使用它来存储临时数据。这是另一个**不要动它**的文件夹。
>
> #### /srv (service)
>
> `/srv` 目录包含服务器的数据。如果你正在 Linux 机器上运行 Web 服务器，你网站的 HTML文件将放到 `/srv/http`（或 `/srv/www`）。 如果你正在运行 FTP 服务器，则你的文件将放到 `/srv/ftp`。
>
> > **/srv ：主要用来存储本机或本服务器提供的服务或数据。**（用户主动生产的数据、对外提供服务）
> >
> > 所以，服务器被用作Web开发时，html文件更应该被放在/srv/www下，而不是/var/www下（因为/srv目录是新标准中才有的，出现较晚；而且Apache将/var/www设为了web默认目录，所以现在绝大多数人都把web文件放在/var/www，这是个历史遗留问题）。
> >
> > 如ftp、流媒体服务等也应该被放在/srv对应的目录下。如果对应目录太大，应该另外挂载分区。
>
> #### /sys
>
> `/sys` 是另一个类似 `/proc` 和 `/dev` 的虚拟目录，它还包含连接到计算机的设备的信息。
>
> 在某些情况下，你还可以操纵这些设备。 例如，我可以通过修改存储在 `/sys/devices/pci0000:00/0000:00:02.0/drm/card1/card1-eDP-1/intel_backlight/brightness` 中的值来更改笔记本电脑屏幕的亮度（在你的机器上你可能会有不同的文件）。但要做到这一点，你必须成为超级用户。原因是，与许多其它虚拟目录一样，在 `/sys` 中打乱内容和文件可能是危险的，你可能会破坏系统。直到你确信你知道你在做什么。否则不要动它。
>
> > *System* 正在运行的硬件设备的驱动程序信息。（虚拟文件系统，不占内存）
>
> #### /tmp (temporary)
>
> `/tmp` 包含临时文件，通常由正在运行的应用程序放置。文件和目录通常（并非总是）包含应用程序现在不需要但以后可能需要的数据。
>
> 你还可以使用 `/tmp` 来存储你自己的临时文件 —— `/tmp` 是少数挂载到根目录下而你可以在不成为超级用户的情况下与它进行实际交互的目录之一。
>
> > **/tmp ：保存在使用完毕后可随时销毁的缓存文件。**（有可能是由系统或程序产生、也有可能是用户主动放入的临时数据、系统会自动清理）
>
> #### /var (variable)
>
> `/var` 最初被如此命名是因为它的内容被认为是可变的variable，因为它经常变化。今天，它有点用词不当，因为还有许多其他目录也包含频繁更改的数据，特别是我们上面看到的虚拟目录。
>
> 不管怎样，`/var` 目录包含了放在 `/var/log` 子目录的日志文件之类。日志是记录系统中发生的事件的文件。如果内核中出现了什么问题，它将被记录到 `/var/log` 下的文件中；如果有人试图从外部侵入你的计算机，你的防火墙也将记录尝试。它还包含用于任务的假脱机程序。这些“任务”可以是你发送给共享打印机必须等待执行的任务，因为另一个用户正在打印一个长文档，或者是等待递交给系统上的用户的邮件。
>
> > **/var ：系统产生的不可自动销毁的缓存文件、日志记录。**（系统和程序运行后产生的数据、不对外提供服务、只能用户手动清理）（包括mail、数据库文件、日志文件）
>
> ### 更深入的研究
>
> 这里仅仅谈了根目录，但是许多子目录都指向它们自己的一组文件和子目录。图 2 给出了基本文件系统的总体概念（图片是在 Paul Gardner 的 CC BY-SA 许可下提供的），Wikipedia 对每个目录的用途进行了总结。
>
> ![图 2：标准 Unix 文件系统](Linux/094428ko2tc8saplc2apto.png)
>
> *图 2：标准 Unix 文件系统*
>
> 要自行探索文件系统，请使用 `cd` 命令：`cd`将带你到你所选择的目录（ `cd` 代表更改目录）。
>
> 如果你不知道你在哪儿，`pwd`会告诉你，你到底在哪里，（ `pwd` 代表打印工作目录 ），同时 `cd`命令在没有任何选项或者参数的时候，将会直接带你到你自己的主目录，这是一个安全舒适的地方。
>
> 最后，`cd ..`将会带你到上一层目录，会使你更加接近根目录，如果你在 `/usr/share/wallpapers` 目录，然后你执行 `cd ..` 命令，你将会跳转到 `/usr/share` 目录
>
> 要查看目录里有什么内容，使用 `ls` 或这简单的使用 `l` 列出你所在目录的内容。
>
> 当然，你总是可以使用 `tree` 来获得目录中内容的概述。在 `/usr/share` 上试试——里面有很多有趣的东西。
>
> ### 总结
>
> 尽管 Linux 发行版之间存在细微差别，但它们的文件系统的布局非常相似。 你可以这么说：一旦你了解一个，你就会都了解了。 了解文件系统的最好方法就是探索它。 因此，伴随 `tree` ，`ls` 和 `cd` 进入未知的领域吧。
>
> 你不会只是因为查看文件系统就破坏了文件系统，因此请从一个目录移动到另一个目录并进行浏览。 很快你就会发现 Linux 文件系统及其布局的确很有意义，并且你会直观地知道在哪里可以找到应用程序，文档和其他资源。
>
> 参考资料：
>
> * [技术|Linux 文件系统详解](https://linux.cn/article-9798-1.html)
> * [关于linux下/srv、/var和/tmp的职责区分_frcoder的博客-CSDN博客_linux中srv](https://blog.csdn.net/u012107143/article/details/54972544)
> * [linux常见分区及各目录作用与全称_frcoder的博客-CSDN博客](https://blog.csdn.net/u012107143/article/details/54973028)

### 2.2 vi/vim编辑器

VI是Unix操作系统和类Unix操作系统中最通用的文本编辑器。（白底黑字）

VIM编辑器是从VI发展出来的一个性能更强大的文本编辑器，VIM与VI编辑器完全兼容。（字体颜色区分）

基本上 vi/vim 共分为三种模式，分别是**命令模式（Command mode）**，**输入模式（Insert mode）**和**底线命令模式（Last line mode）**。

*具体的命令和操作方式非常多，这里就不写了，用得多了自然会记住。用到的时候直接百度或者查一些速记手册即可。*

### 2.3 网络配置

#### 2.3.1 ifconfig

要使Linux虚拟机与物理主机相互连通，就必须得到虚拟机的ip地址。

可以在Shell中使用`ifconfig`(networks interfaces configuring)命令查看和配置网卡接口。

扩展资料：

[Linux下 ifconfig 命令的使用（Linux命令）_hrx-@@的博客-CSDN博客_linuxifconfig用法](https://blog.csdn.net/lang523493505/article/details/107242769)

[理解物理网卡、网卡接口、内核、IP等属性的关系 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1662501)

#### 2.3.2 网络连接方式

VMware提供了三种网络连接方式。

* *VMware-编辑-虚拟网络编辑器-更改设置*

  ![VMware-编辑-虚拟网络编辑器-更改设置](Linux/VMware-编辑-虚拟网络编辑器-更改设置.png)

* *控制面板-网络和Internet-网络连接*

  ![控制面板-网络和Internet-网络连接](Linux/控制面板-网络和Internet-网络连接.png)

##### 桥接模式

虚拟机直接连接外部物理网络，主机起到了网桥的作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的。

![桥接模式](Linux/桥接模式.png)

##### NAT模式

虚拟机和主机构建一个专用网络，并通过虚拟网络地址转换（NAT）设备对IP进行转换。虚拟机通过共享主机IP可以访问外部网络，但外部网络无法访问虚拟机。

![NAT模式](Linux/NAT模式.png)

##### 仅主机模式

虚拟机只与主机共享一个专用网络，与外部网络无法通信。

![仅主机模式](Linux/仅主机模式.png)

#### 2.3.3 配置静态IP

为什么要配置静态IP？为什么不用默认的“自动获取IP”？因为我们不希望作为服务器的（被访问的）机器的IP是变化的。

##### 图形化界面方式

*应用程序-系统工具-设置-网络-有线-设置图标-IPv4-IPv4 Method*

选择“手动”，即可配置IP地址、子网掩码、网关。

##### Shell方式

修改ifcfg-ens33，即修改"ens33"这个"if"(network interface)的"cfg"(configuration)。

```shell
vim /etc/sysconfig/network-srcipts/ifcfg-ens33
```

注意静态IP的网络号要符合实际

```
...
# 这里原来是dhcp
BOOTPROTO="static"
...
# 下面几行是为了配置静态IP而新增的
IPADDR=192.168.150.100
NETMASK=255.255.255.0
GATEWAY=192.168.150.2
DNS1=192.168.150.2
```

重启网络

```shell
service network restart
```

#### 2.3.4 配置主机名

##### 查看主机名

其实在Shell中已经自然地显示主机名了，就像这样：

`[用户名@主机名 当前目录]#或$ 光标所在处`

当然你也可以主动通过命令查看：

```shell
# 显示hostname
hostname
# 显示hostname相关信息（hostname control）
hostnamectl	#hostnamectl status
```

##### 修改配置文件方式

```shell
# 里面唯一的内容就是主机名，直接修改即可
vim /etc/hostname
# 重启
reboot
```

##### 命令方式

```shell
# 修改主机名，立即生效
hostnamectl set-hostname newhostname
```

> hostnamectl命令来自于英文词组“hostname control”的缩写，其功能是用于显示与设置主机名称。基于/etc/hostname文件修改主机名称需要重启服务器后才可生效，而hostnamectl命令设置过的主机名称可以立即生效，效率更高。
>
> **语法格式**：hostnamectl [参数]
>
> **常用参数**：
>
> | 参数         | 含义               |
> | ------------ | ------------------ |
> | -H           | 操作远程主机       |
> | status       | 显示当前主机名设置 |
> | set-hostname | 设置系统主机名     |
>
> 参考资料：[hostnamectl命令 – 显示与设置主机名称 – Linux命令大全(手册)](https://www.linuxcool.com/hostnamectl)

##### 配置主机名有什么用？

首先要知道，主机名host name和域名domain name是有区别的，而且这里还有一个完整域名(Fully Qualified Domain Name, FQDN)的概念。

举个例子：

* baidu.com——域名
* www——主机名（默认，可缺省）
* www.baidu.com——完整域名

> 讲一个故事：刚开始，我发明了互联网通信(即在网上传输东西)，互联网通信是发生在TCP/IP协议之上(即这个协议是互联网通信的基础之一，TCP像车子，IP类似房子的地址)。IP就是上面说的这个TCP/IP协议的产物，有了他就可以想和谁通信就和谁通信，于是很多人开始把IP记住来通信，但是我发现很多人对数字不敏感，他们都说IP不好记。后来，我就发明了DNS解析，域名就是DNS的产物，如baidu.com就是一个域名，这个比那些IP的数字好记忆很多，得到了很多人的认可，他们不用再输入IP了，直接输入baidu.com就可以通信了，很开心。那么有人很好奇，就问：“域名能用了，那IP是不是就没法用了啊？”，我回答：“有啊，你回去输入IP试一试，和以前一样的。”。有人问：“那域名怎么就能通信了？”，我说：“因为我发明了的DNS帮了大忙，他把域名变成IP了啊:D”。有人问：“那域名不会重复吗？”，我回答：“不可能，我规定了不能重复，注册的时候都记录在本子上了，绝对不会重复。一句话，DNS让域名不会重复，懂了吗？”。有一天，有人跑过来问我，兴奋地说：“我发现了一个好玩的事情，我输入[www.baidu.com](http://www.baidu.com/)和baidu.com都能访问，为什么啊？”，我说：“你是否发现了多出一个www的东西？其实它叫'主机名'。”。 他又问：“那主机名有什么用？我不加www这个主机名，也可以通信(这里指上网)，那这个主机名不是没有作用了吗？还占用了位置呢，害我多输入几个字母。”。我说：“哈哈，不是没用，而是你用的机会少，但你用的机会少，你没用过，不代表他没有用啊。认知限制了你的想法啊~。在这里，我先把'主机'比作'房间'，你用baidu.com这个域名可以找到我的'房子'，但是你开了大门，进来的是我的'房子'的一个'房间'，这个就是'www房间'，是我在DNS规定的默认的缺省的'房间'，即你不填上主机(可以比作房间名或房间号)，默认就到这个'www房间'。而我还有其他的'房间'啊，我还有'a房间'、'b房间'、'Z房间'等等。你说你要进来'a房间'?容易啊，你加上主机(房间号)就可以啦，就是说你输入a.baidu.com就行啦。”。最后，还有人问我：“为什么默认是'www'啊？”。我说：“打个比方，我反问你一个问题：为什么你会说话的时候，叫你父亲作'爸爸'啊？”。 有人会回答是习惯。对，是习惯，习惯用'www'作为默认。 补充一点"不知道顶级域名的定义的请自行查阅资料"。好了，我把我的理解用文字说出来了，还有不懂，请追问：D
> 参考资料：[有一个人说网址前面的www是主机名？主机？我懵了_百度知道](https://zhidao.baidu.com/question/461162404144932445.html)

其次，虽然在客户机的hosts文件中配置的主机名并不一定要和Linux服务器的实际主机名一致——其实哪怕完全不一样也是可以进行远程登录的，客户机只要输对了自己本机的hosts文件中配置的主机名就行——但是通常我们还是把两者设置成一样的，方便检验是否登录到了正确的机器上。

### 2.4 远程管理

> 提到远程管理，通常指的是远程管理服务器，而非个人计算机。个人计算机可以随时拿来用，服务器通常放置在机房中，用户无法直接接触到服务器硬件，只能采用远程管理的方式。
>
> 远程管理，实际上就是计算机（服务器）之间通过网络进行数据传输（信息交换）的过程，与浏览器需要 HTTP 协议（超文本传输协议）浏览网页一样，远程管理同样需要远程管理协议的支持。
>
> 目前，常用的远程管理协议有以下 4 种：
>
> - RDP（remote desktop protocol）协议：远程桌面协议，大部分 Windows 系统都默认支持此协议，Windows 系统中的远程桌面管理就基于该协议。
> - RFB（Remote FrameBuffer）协议：图形化远程管理协议，VNC 远程管理工具就基于此协议。
> - Telnet：命令行界面远程管理协议，几乎所有的操作系统都默认支持此协议。此协议的特点是，在进行数据传送时使用明文传输的方式，也就是不对数据进行加密。
> - SSH（Secure Shell）协议：命令行界面远程管理协议，几乎所有操作系统都默认支持此协议。和 Telnet 不同，该协议在数据传输时会对数据进行加密并压缩，因此使用此协议传输数据既安全速度又快。
>
> 参考资料：[Linux远程管理协议（RFB、RDP、Telnet和SSH）](http://c.biancheng.net/view/2845.html)

远程管理Linux服务器虽然有多种方式，但大多是基于SSH协议。

当然，在连接之前需要在客户机的hosts文件中配置好主机名与IP地址的映射，方便访问。

#### 2.4.1 Windows命令行方式

`ssh 用户名@IP地址/已在hosts中映射的主机名/已在hosts中映射的域名`

#### 2.4.2 远程管理工具

市面上有很多，但要么是收费的，要么界面难看，要么不仅是收费的而且界面难看。

我推荐一个国产的免费工具——**Aechoterm，免费、UI简单好看、基本功能齐全（支持SSH和SFTP）**，上手体验大有当初第一次使用Apifox的感觉，如沐春风，十分舒适。

官网：https://ec.nantian.com.cn/#/home

官网文档/功能演示：https://ec.nantian.com.cn/#/about

### 2.5 系统管理

#### 2.5.1 进程与服务

系统中正在执行的程序，叫做“进程”（process）

系统启动后一直存在并常驻内存的进程，叫做“服务”（service）。这样的进程又称“守护进程”（daement process），这样的服务又称“系统服务”，在Linux系统中，这两个概念说的其实是一回事。

#### 2.5.2 service命令

适用于CentOS 6之前的版本，虽然新版本的CentOS仍然向下兼容该命令，但实际上能管理的服务很少，所以了解即可。

**基本语法**

```shell
service 服务名 start|stop|restart|status    # 开启服务|停止服务|重启服务|查看服务状态
```

**查看服务**

```shell
ls /etc/init.d    # init.d的d就是daemon
```

![SysV.initscripts](Linux/SysV.initscripts.png)

#### 2.5.3 systemctl命令

**基本语法**

```shell
systemctl start|stop|restart|status 服务名.service    # 开启服务|停止服务|重启服务|查看服务状态
systemctl start|stop|restart|status 服务名            # .service可以省略
```

**查看服务**

```shell
ls /usr/lib/systemd    # systemd的d就是daemon
ls /usr/lib/systemd/system
```

![systemd services](Linux/systemd services.png)

> RHEL/CentOS 7.0中一个最主要的改变，就是切换到了systemd。它用于替代红帽企业版Linux前任版本中的SysV和Upstart，对系统和服务进行管理。systemd兼容SysV和Linux标准组的启动脚本。
>
> Systemd是一个Linux操作系统下的系统和服务管理器。它被设计成向后兼容SysV启动脚本，并提供了大量的特性，如开机时平行启动系统服务，按需启动守护进程，支持系统状态快照，或者基于依赖的服务控制逻辑。
>
> <u>先前的使用SysV初始化或Upstart的红帽企业版Linux版本中，使用位于/etc/rc.d/init.d/目录中的bash初始化脚本进行管理。而在RHEL 7/CentOS 7中，这些启动脚本被服务单元取代了。服务单元以.service文件扩展结束，提供了与初始化脚本同样的用途。要查看、启动、停止、重启、启用或者禁用系统服务，你要使用systemctl来代替旧的service命令。</u>
> systemd是服务管理程序，他整合了service和chkconfig的功能为一体，可以永久性或只在当前会话中启用/禁用服务。
> 参考资料：https://blog.csdn.net/wzyzzu/article/details/44171319

#### 2.5.4 系统运行级别

**CentOS 6**的7个运行级别：

* **runlevel 0 Halt 停机模式**。系统默认运行级别不能设置为0，否则电脑一开机就会关机，无法正常启动。
* **runlevel 1 Single user mode 单用户模式**，只允许root用户登录，但也可以绕过登录验证。主要用于系统维护，禁止远程登陆。
* **runlevel 2 Multiuser, without NFS 多用户模式**，没有网络文件系统（NFS）支持。
* **runlevel 3 Full multiuser mode 完全多用户模式**，有网络文件系统（NFS）支持。用户登录后会进入控制台模式的黑框框，在没有网络的环境下等同于运行级别2。
* **runlevel 4 Unused 系统未使用**。在某些特殊情况下会用到，例如在笔记本电脑的电池用尽时，可以切换到这一模式来做一些设置。
* **runlevel 5 X11 图形界面模式**
* **runlevel 6 Reboot 重启模式**。系统默认运行级别不能设置为6，否则电脑一开机就进入重启模式，会不停地重启，无法正常启动。

**CentOS 7**简化为两个比较主要的运行级别：

* **multi-user.target** 等价于原运行级别3（多用户有网，无图形界面）
* **graphical.target** 等价于原运行级别5（多用户有网，有图形界面）

> 注意这两种的运行级别并不是一一对应关系！
>
> 比如SysVinit系统的级别2、3、4到了systemd系统其实都是multi-user.target，即原来的级别3；
>
> 比如SysVinit系统的级别0原来叫做停机halt模式，但到了systemd系统则是shutdown.target，变成了关机，并不是原来的停机。经过实测，在CentOS 7里执行`init 0`的效果确实是关机，而我猜测在CentOS 6里执行`init 0`会停机但不是关机。

| 运行级别（runlevel） | SysVinit系统        | systemd系统       |
| -------------------- | ------------------- | ----------------- |
| 0                    | 关闭或暂停系统      | shutdown.target   |
| 1                    | 单用户模式          | rescue.target     |
| 2                    | 多用户，没有NFS     | multi-user.target |
| 3                    | 完全多用户，有NFS   | multi-user.target |
| 4                    | 未使用              | multi-user.target |
| 5                    | X11（图形用户界面） | graphical.target  |
| 6                    | 重启系统            | reboot.target     |

```shell
# 查看当前系统默认的运行级别
systemctl get-default
# 设置当前系统默认的运行级别
systemctl set-default multi-user.target	# 开机为黑框框，我喜欢
systemctl set-default graphical.target	# 开机为图形桌面
# 切换系统当前的运行级别
init 0 # 关机
init 1 # 进入单用户模式
init 3 # 进入完全多用户模式，即控制台模式
init 5 # 进入图形界面模式、
init 6 # 重启
```

本小节参考资料：

* [Linux的运行级别 - 知乎](https://zhuanlan.zhihu.com/p/352164966)

* [Linux运行级别概述 以及 CentOS 7以上系统修改密码的方式_菜菜的大数据开发之路的博客-CSDN博客](https://blog.csdn.net/nmsLLCSDN/article/details/117234454)
* [在Linux中检查当前运行级别的五种方法？_target](https://www.sohu.com/a/315115025_495675)

#### 2.5.5 三种Init系统

##### sysvinit

技术|浅析 Linux 初始化 init 系统: sysvinit
https://linux.cn/article-4422-1.html

##### UpStart

技术|浅析 Linux 初始化 init 系统: UpStart
https://linux.cn/article-4423-1.html

##### Systemd

技术|浅析 Linux 初始化 init 系统: Systemd
https://linux.cn/article-4424-2.html?tt

#### 2.5.6 配置服务自启动

##### 通过ntsysv工具配置

ntsysv工具提供了一个基于文本界面的菜单操作方式。

首先在Shell输入`setup`命令进入Text Mode Setup Utility工具窗口，

![Text.Mode.Setup.Utility](Linux/Text.Mode.Setup.Utility.png)

再选中system services进入ntsysv工具（也可以在Shell直接输入`ntsysv`命令进入），

![ntsysv](Linux/ntsysv.png)

对每一个service选择是否automatically started，按空格选中或取消选中。

##### 通过chkconfig或systemctl命令配置

配置SysV服务（CentOS 6）自启动：

```shell
# 列出所有SysV服务服务
chkconfig --list
# 设置服务在2345运行级别下是否开启（016运行级别下默认关闭服务）
chkconfig 服务名 on或off
# 设置服务在特定运行级别下是否开启
chkconfig --level 1|2|3|4|5|6 服务名 on或off
```

配置systemd服务（CentOS 7）自启动：

```shell
# 列出所有systemd单元（以查看服务是否开机启动）
systemctl list-unit-files
# 设置服务是否自启动
systemctl enable或diable 服务名
```

`systemctl list-unit-files`的输出结果：

![systemctl.list-unit-files](Linux/systemctl.list-unit-files.png)

Systemctl接受服务（.service），挂载点（.mount），套接口（.socket）和设备（.device）作为单元，所以UNIT FILE这个概念包括但不限于service。不妨加上| grep .service筛选一下。

STATE为enable代表开机自启，disable代表开机不自启，static代表未配置，表示该服务与其他服务相关联，不能单独设置该服务的启动状态。

##### 实践：关闭和打开防火墙服务

CentOS 6中是iptables服务，CentOS 7中已经没有这个服务了，取而代之的显然是**firewalld**服务。注意forewall后面有个d，即daemon，没错，这又是一个系统服务&守护进程。

首先通过`systemctl status`查看服务信息：

```
[root@hadoop100 ~]# systemctl status firewalld
...
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-06-20 22:21:23 CST; 4min 36s ago
...
[root@hadoop100 ~]# 
```

![systemctl status](Linux/systemctl status.png)

使用`systemctl stop`关闭服务，

```
[root@hadoop100 ~]# systemctl stop firewalld
[root@hadoop100 ~]# systemctl status firewalld
...
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Mon 2022-06-20 22:28:27 CST; 19s ago
...
[root@hadoop100 ~]# 
```

再使用`systemctl start`开启服务，

```
[root@hadoop100 ~]# systemctl start firewalld
[root@hadoop100 ~]# systemctl status firewalld
...
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-06-20 22:30:28 CST; 3s ago
...
[root@hadoop100 ~]# 
```

使用`systemctl disable`关闭服务开机自启动，此时可以`init 6`重启一下，发现firewalld服务确实没有自启动。

```
[root@hadoop100 ~]# systemctl disable firewalld
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
[root@hadoop100 ~]# systemctl status firewalld
...
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-06-20 22:30:28 CST; 7min ago
...
[root@hadoop100 ~]# init 6

 X  shell close
Press any key to reconnect 

Last login: Tue Jun 21 14:22:36 2022 from 192.168.150.1
[root@hadoop100 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
[root@hadoop100 ~]# 
```

最后使用`systemctl enable`开启服务开机自启动，毕竟防火墙还是有用的，哈哈。

```
[root@hadoop100 ~]# systemctl enable firewalld
Created symlink from /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service to /usr/lib/systemd/system/firewalld.service.
Created symlink from /etc/systemd/system/multi-user.target.wants/firewalld.service to /usr/lib/systemd/system/firewalld.service.
[root@hadoop100 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-06-20 22:30:28 CST; 9min ago
...
[root@hadoop100 ~]# 
```

##### 扩展阅读：使用systemctl查看Linux服务状态

[How to view status of a service on Linux using systemctl - nixCraft](https://www.cyberciti.biz/faq/systemd-systemctl-view-status-of-a-service-on-linux/)（精品好文，原文更详细）

> ## Viewing the Status of a Service
>
> The syntax is as follows for the systemctl command
>
> `systemctl status {service-name}`
>
> `systemctl status {unit-name}`
>
> ### How to view status of a service called nginx
>
> Type:
> `$ systemctl status nginx.service`
>
> `## ssh server status ##`
>
> `$ systemctl status sshd.service`
>
> `## Lighttpd web server status ##`
>
> `$ systemctl status lighttpd.service`
>
> ```
> ● lighttpd.service - Lighttpd Daemon
>      Loaded: loaded (/lib/systemd/system/lighttpd.service; enabled; vendor preset: enabled)
>      Active: active (running) since Tue 2020-08-04 04:29:19 UTC; 3 weeks 2 days ago
>    Main PID: 105 (lighttpd)
>       Tasks: 1 (limit: 115783)
>      Memory: 56.5M
>      CGroup: /system.slice/lighttpd.service
>              └─105 /usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
> 
> Aug 04 04:29:19 utls-bash-wiki systemd[1]: Starting Lighttpd Daemon...
> Aug 04 04:29:19 utls-bash-wiki systemd[1]: Started Lighttpd Daemon.
> ```
>
> The dot (“●“) uses color on supported terminals to summarize the unit state at a glance. White color indicates an “inactive” or “deactivating” state. Red color indicates a “failed” or “error” state. Green indicates an “active”, “reloading” or “activating” state.
>
> ### Understanding systemd service/unit states
>
> The status of Linux service depends upon various states such as follows:
>
> | Service status   | Description                                                  |
> | ---------------- | ------------------------------------------------------------ |
> | active (running) | Service or daemon is running in the background. For example, sshd or nginx/apache web server and listing for incoming traffic. |
> | active (exited)  | Service successfully started from the config file. Typically one time services configuration read before Service was exited. For example, AppArmor or Firewall service. |
> | active (waiting) | Our service is running but waiting for an event such as CPUS/printing event. |
> | inactive         | Service is not running.                                      |
> | enabled          | Service is enabled at boot time.                             |
> | disabled         | Service is disbled and will not be started at Linux server boot time. |
> | static           | Service cannot be enabled on Linux, but mostly started by another systemd unit automatically. In other words, the unit file is not enabled and has no provisions for allowing in the [Install] unit file section. |
> | masked           | Service is completely disabled and any start operation on it always fails. |
> | alias            | Service name is an alias. It means service is symlink to another unit file. |
> | linked           | Made available through one or more symlinks to the unit file (permanently in /etc/systemd/system/ or transiently in /run/systemd/system/), even though the unit file might reside outside of the unit file search path. |
>
> ...
>
> 
>
> We can list all services unit as follows:
> `$ sudo systemctl --type=service`
> Want to see mount type units? Try:
> `$ sudo systemctl --type=mount`
> ...
>
> 
>
> ### To show all installed unit files use:
>
> ```
> $ sudo systemctl list-unit-files
> ```
>
> ...
>
> 
>
> ## What to do if the service such as nginx is not running?
>
> Turn on the systemd service:
> `$ sudo systemctl enable nginx.service`
> Start the nginx service:
> `$ sudo systemctl start nginx.service`
> We can stop or restart the service as follows:
> `$ sudo systemctl stop nginx.service$ sudo systemctl restart nginx.service`
> Verify that if a service enabled or not, run:
> `$ sudo systemctl is-enabled nginx.service`
> See status again:
> `$ sudo systemctl status nginx.service`
>
> ...

#### 2.5.7 关机

```shell
shutdown			# 无参数，默认1分钟后关机
shutdown -H			# halt，默认1分钟后停机（还有个-h表示关机后停机，不要）
shutdown --halt 	# halt，默认1分钟后停机
shutdown -P			# poweroff，默认1分钟后关机
shutdown --poweroff	# poweroff，默认1分钟后关机
shutdown -r			# reboot，默认1分钟后重启
shutdown --reboot	# reboot，默认1分钟后重启

shutdown -c			# cancel，取消关机
shutdown now		# now，立即关机
shutdown 10			# 10分钟后关机。时间单位是分钟。

sync			# 手动将数据从内存同步到磁盘中（系统关机时会自动执行sync操作）
halt			# 立即停机，等同于shutdown -H now
poweroff		# 立即关机，等同于shutdown -P now
reboot			# 立即重启，等同于shutdown -r now

init 0			# 切换到关机模式（shutdown.target）
init 6			# 切换到重启模式（reboot.target）
```

**sync有什么用？**

> Linux 系统中为了提高磁盘的读写效率，对磁盘采取了 “预读迟写”操作方式。当用户 保存文件时，Linux 核心并不一定立即将保存数据写入物理磁盘中，而是将数据保存在缓 冲区中，等缓冲区满时再写入磁盘，这种方式可以极大的提高磁盘写入数据的效率。但是， 也带来了安全隐患，如果数据还未写入磁盘时，系统掉电或者其他严重问题出现，则将导 致数据丢失。使用 sync 指令可以立即将缓冲区的数据写入磁盘。

**halt与poweroff的区别？**

halt——关机但不关闭电源，即“停机”

poweroff——关机且关闭电源，即真正意义上的“关机”

## 3. 实操篇（常用命令）

Shell可以看作是一个命令解释器，为我们提供了交互式的基于文本的控制台界面。

Shell有很多种，Bash（Bourne Again Shell）就是其中之一，在Red Hat系的Linux发行版中常用。

> 原文：[Linux中的shell和bash - 墨鱼菜鸡 - 博客园](https://www.cnblogs.com/csnd/p/11807739.html)
>
> ## shell
>
>  Shell 俗称壳（用来区别于核），是指“为使用者提供操作界面”的软件（命令解析器）。它类似于DOS下的command.com和后来的cmd.exe。它接收用户命令，然后调用相应的应用程序。
>
> 我们先来看一些Linux系统的结构
>
> ![20181129225709122](Linux/20181129225709122.png)
>
> 位于最内层的是硬件，然后是Linux系统内核。shell介于用户和系统内核之间。
>
> **那么shell的功能是什么呢？**
>
> shell用来接收我们用户的输入，并且解释我们的命令。然后将其传给系统内核，内核再调用硬件来操作。
>
> ### shell script 
>
> 我们很容易知道“Shell Script”指的是针对shell所写的脚本。我们将一些shell规定的语法与命令，再搭配正则表达式、管道命令与数据流重定向等功能，写成一个纯文本文件以达到我们想要的处理目的，再配以“.sh”的扩展名，这便是“Shell script”了。
>
> 简言之，我们只有通过Shell 这个工具来解释我们的命令等请求，我们才能成功实现与计算机的交流，同时再搭配Shell script可以批量处理命令的“程序”，我们就可以与计算机更优雅的交流哟！
>
> 我们可能会问：既然shell是解释命令的工具，那么这个工具可不可以多样化呢？不同的解释工具可不可以遵从不同的规则呢？ 
> 这是必然的咯，何况是像Linux这种开源的好东西，怎么会缺乏多样性呢？！ 
> 所以，我们就不难理解Linux中的shell有多种类型了吧，这其中最常用的几种是 **Bourne shell（sh）**、**Bourne Again shell(bash)** 和 **Debian Almquist Shell（dash）**。其中三种shell各有优缺点：
>
> ### sh
>
> **sh**(Bourne shell)是UNIX最初使用的shell，并且在每种UNIX上都可以使用。Bourne shell在shell编程方面相当优秀，但在处理与用户的交互方面做得不如其他几种shell。
>
> ### bash
>
> **bash** (Bourne Again shell)，它是Linux操作系统缺省的shell，是Bourne shell的扩展，简称**Bash**，与Bourne shell完全向后兼容，并且在Bourne shell的基础上增加、增强了很多特性。Bash放在/bin/bash 中，它有许多特色，可以提供如命令补全、命令编辑和命令历史表等功能，有灵活和强大的编程接口，同时又有很友好的用户界面。 而且在Redhat系列的Linux 操作系统中的 /bin/sh 是 /bin/bash 的符号链接。所以，用 sh执行脚本和bash 执行脚本，效果是一样的。所以我们通常会在脚本第一行写 #!/bin/bash ，意思就是用 /bin/bash 去执行这个脚本。
>
> ![20181129230731366](Linux/20181129230731366.png)
>
> ### Dash
>
> 虽然bash好用，功能也多。但是鉴于bash过于复杂，有人把bash从NetBSD移植到Debian Linux并更名为 **dash** (Debian Almquist Shell)，并建议将/bin/sh指向它，以获得更快的脚本执行速度。Dash Shell 比Bash Shell小的多，符合POSIX标准。也就是若脚本第一行为“#!/bin/sh”，则我们使用命令：sh script_name.sh 时是调用的dash去解释脚本；Debian下默认使用的还是bash，只不过sh指向的是dash
>
> ![20181129231219243](Linux/20181129231219243.png)

### 3.1 帮助信息

一部分具有基本功能的命令是内置在Shell中的，在系统启动后会随着Shell一起加载，并常驻在内存中，这就是“内置命令”。例如：`history`、`cd`。

而其他的命令就叫做“外部命令”，外部命令要等到被调用时才会被加载到内存中。例如`ls`、`grep`。

`man 命令名`

获得该命令的帮助手册（manual），官方、权威、完整、详细。

`man -f 命令名`

查看该命令位于帮助手册的哪一页（同一命令可能有多处不同版本的解释）。

`man 页数 命令名`

指定手册的页数，查看命令解释。

`type 命令名`

。查看一个命令是不是Shell的内置命令（built-in），如果不是，则显示其别名。

`help 内置命令名`

获得Shell内置命令的帮助信息。

`外部命令名 --help`

获得外部命令的帮助信息。

### 3.2 文件目录类

#### 3.2.1 查看和切换工作目录

`pwd`

print working directory

`pwd -P`

显示真实目录。在进入软链接目录时才能体现出和`pwd`的差别。

`cd -`

返回到上一次所在目录

`cd` / `cd ~`

切换到当前用户的主文件夹

#### 3.2.2 列出目录内容

`ls`

list

`ls -a`

包括隐藏文件和隐藏目录，即带前缀“.”的。

`ls -a`

> -a, --all
>               do not ignore entries starting with .

`ls --block-size=SIZE`

> --block-size=SIZE
>               scale sizes by SIZE before printing them; e.g., '--block-size=M' prints sizes in units of  1,048,576  bytes;  see  SIZE
>               format below

`ls -h`

human-readable，以容易理解的格式列出文件大小 (例如 1K 234M 2G)

> **SIZE is an integer and optional unit (example: 10M is 10\*1024\*1024).  Units are K, M, G, T, P, E, Z, Y (powers of 1024) or KB,
>        MB, ... (powers of 1000).**
>
> ——这个单位制非常重要！

`ls -l` / `ll`

显示详细信息

> use a long listing format

`ls -i`

> -i, --inode
>               print the index number of each file

#### 3.2.3 创建和删除目录

`mkdir 目录名`

make directories

`mkdir -p 多级目录`

递归创建多级目录。从外往里，比如`mkdir -p a/b/c` == `mkdir a a/b a/b/c`

`rmdir 目录名`

remove directory

`rmdir -p 多级目录`

用递归的方式删除指定的目录路径中的所有父级目录。从里往外，比如`rmdir a/b/c` == `rmdir a/b/c a/b a`

#### 3.2.4 创建文件

`touch 文件名`

touch命令的功能是用于创建空文件与修改时间戳。如果文件不存在，则会创建出一个空内容的文本文件；如果文件已经存在，则会对文件的Atime（访问时间）和Ctime（修改时间）进行修改操作，管理员可以完成此项工作，而普通用户只能管理主机的文件。

#### 3.2.5 复制文件或文件夹

`cp source dest `

复制目录或文件

`cp -r sourcedir destdir`

递归地复制整个目录

`\cp source dest`

**任意命令前面加上反斜杠表示原生命令**，在此例中，`cp`实际上是`cp -i`的别名（可通过`type cp`验证），`-i`参数表示在覆盖文件时需要询问并确认，而`\cp`相当于是纯粹的、真正意义上不带任何参数的`cp`命令，即“原生”的`cp`命令，在覆盖文件时不会提醒。

#### 3.2.6 删除和移动文件

`rm file/dir`

remove

`rm -f file/dir`

强制删除（不二次询问）

`rm -r dir`

递归删除，适用于删除整个目录

> `rm -rf /*`
>
> 经典！强制且递归地删除根目录下所有文件和目录，并且没有任何提醒。

`mv file/dir file/dir `

move。剪切目录或文件后粘贴，在同一目录下进行则是重命名操作。

#### 3.2.7 查看文件

`cat filename`

concatenate。cat命令适合查看内容较少的、纯文本的文件。

`cat -n filename`

 显示行数（空行也编号）。

`more filename`

比cat更加强大的文本查看工具。

more命令的功能是用于分页显示文本文件内容。如果文本文件中的内容较多较长，使用cat命令读取后则很难看清，这时使用more命令进行分页查看就更加合适了，可以把文本内容一页一页的显示在终端界面上，用户每按一次回车即向下一行，每按一次空格即向下一页，直至看完为止。按b跳转到上一页，按f或空格跳转到下一页，按q退出。

`less filename`

比more更加强大的文本查看工具。

按b（还支持PageUp键）跳转到上一页，按f或空格（还支持PageDown键）跳转到下一页，按q退出。

按=号展示文件详情和当前阅读进度详情。

按g跳到全文最开始，按shift+g跳到全文最末尾。

按/+字符串向下查找，按n下一个，按N上一个

按?+字符串向上查找，按n上一个，按N下一个

`echo aString`

echo是用于在终端设备上输出指定字符串或变量提取后值的命令，能够给用户一些简单的提醒信息，也可以将输出的指定字符串内容同管道符一起传递给后续命令作为标准输入信息再来进行二次处理，又或者同输出重定向符一起操作，将信息直接写入到文件中。

如需提取变量值，需在变量名称前加入$符号做提取，变量名称一般均为大写形式。

`echo -e aString`

支持反斜杠控制的字符转换。

#### 3.2.8 控制台显示和输出重定向

`命令 > filename`

输出重定向

`命令 >> filename`

不覆盖file的输出重定向

#### 3.2.9 监控文件变化

`head filename`

显示文件头部的部分内容，默认显示前10行

`head -n 行数 filename`

指定显示行数

`tail filename`

显示文件尾部的部分内容，默认显示后10行，加上参数-n同理。

`tail -f filename`

实时监控（刷新）文件内容变化，常用于监控日志变化。ctrl+s暂停刷新显示，ctrl+q恢复刷新显示。

#### 3.2.10 软链接

`ln -s 真实文件/目录所在路径 软链接文件保存路径`

ln即link，-s即soft，创建软链接，类似Windows中的快捷方式。

注意，“真实文件/目录所在路径”如果是相对路径，那么其参照系就是软链接文件的保存路径，而不是当前执行命令时所在目录！

而软链接的文件如果用相对路径，那么其参照系就是当前执行命令所在目录。

#### 3.2.11 硬链接

`ln 源文件 链接文件`

不加-s参数那就是创建硬链接。

扩展阅读：

[详解 Linux 中的硬链接与软链接 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1690636)（精品好文！）

#### 3.2.12 查看历史命令

`history`

查看已经执行过的所有命令，可加数字参数表示只显示最后多少行。

### 3.3 时间日期类

`date`

显示当前时间。

> 打印出来的“CST”是中国标准时间（China Standard Time）的缩写。

```
[root@hadoop100 ~]# date
Fri Jun 24 14:48:33 CST 2022
[root@hadoop100 ~]# date +%Y
2022
[root@hadoop100 ~]# date +%y
22
[root@hadoop100 ~]# date +%m
06
[root@hadoop100 ~]# date +%d
24
[root@hadoop100 ~]# date +%H
14
[root@hadoop100 ~]# date +%M
49
[root@hadoop100 ~]# date +%S
12
[root@hadoop100 ~]# date +%s
1656053354
[root@hadoop100 ~]# 
```

所有参数的最前面一定要有个加号，在那之后就无论怎么连接都行，可以用减号、冒号等特殊符合。

```
[root@hadoop100 ~]# date +%Y-%m
2022-06
[root@hadoop100 ~]# date +%Y+%m
2022+06
[root@hadoop100 ~]# date +%Y=%m
2022=06
[root@hadoop100 ~]# date +%Y:%m
2022:06
[root@hadoop100 ~]# date +%Y：%m
2022：06
[root@hadoop100 ~]# date +%Y.%m
2022.06
[root@hadoop100 ~]# date +%Y-%m+%d=%H:%M：%S.%s
2022-06+24=14:54：06.1656053646
[root@hadoop100 ~]# date "+%Y-%m-%d %H:%M:%S"
2022-06-24 14:55:02
[root@hadoop100 ~]# 
```

但有些特殊符号不行，比如括号和空格——解决方法就是用双引号括起来。

```
[root@hadoop100 ~]# date +%Y(%m
-bash: syntax error near unexpected token `('
[root@hadoop100 ~]# date "+%Y(%m"
2022(06
[root@hadoop100 ~]# date +%Y %m
date: extra operand ‘%m’
Try 'date --help' for more information.
[root@hadoop100 ~]# date "+%Y %m"
2022 06
[root@hadoop100 ~]# 
```

`date -d "数字 hours/minutes/... ago"`

如果填数字n，n是正数就表示多少时间单位之前，如果是负数则代表之后。

`date -s datestr`

将系统时间设为datestr中所设定的时间，格式示例：`2077-06-24 02:00:00`。

设定时间后如果想要用回原来的时间怎么办？需要用到`ntpdate `命令来更新（要加参数的），这里了解即可。

`cal`

Calendar。显示当前月的日期（等同于`cal -1`），默认将星期天作为月的第一天。

`cal -m`

将星期一作为月的第一天

`cal -3`

显示最近三个月的日历，只能是-1或-3，没有别的数字可以选择。

`cal -y`

显示当前年的日历

`cal [options] [[[day] month] year]`

可以指定显示某年某月某日的日历。

### 3.4 用户权限类

> 以下大部分操作必须在管理员账户下进行，普通用户没有这种级别的权限。

#### 3.4.1 添加和查看用户

`useradd username`

创建新用户，默认将该用户分配到同名用户组，默认在/home目录下新建一个同名目录作为该用户的个人目录。

`useradd -d dir username`

创建新用户并指定其个人目录

`passwd username newpassword`

设定或更改用户密码

`id username`

id命令的功能是用于显示用户与用户组信息。

显示的UID是该用户身份的唯一识别号码，GID是该用户的“主”用户组（即该用户的基本组）的唯一识别号码，GROUPS则是该用户所在的所有组（包括扩展组）。用户仅有一个基本组，但可以有多个扩展组。

`cat/more/less /etc/passwd`

查看所有用户。

不过这个文件里面有很多系统用户（“伪用户”），是系统自动创建的，用来运行系统服务的，并不能作为真正的用户去登录。

`su username`

switch user。管理员切换至任意用户身份而无需密码验证，而普通用户切换至任意用户身份均需密码验证。

切换用户之后，可以用`exit`退出当前会话，回到上一个会话，不需要输入密码。

`who`

显示当前登录用户信息，包含登录的用户名、终端、日期时间、进程等信息。

若从root用户切换到user1用户，执行此命令会打印出**前者**的用户信息，也就是说它只认一开始登录会话的那个用户，不管嵌套登录的。

`whoami`

打印当前登录用户，若从root用户切换到user1用户，执行此命令会打印出**后者**的用户名。

#### 3.4.2 获取root权限和删除用户

`sudo 任何命令及其参数`

sudo命令来自于英文词组“super user do”的缩写，中文译为“超级用户才能干的事”，其功能是用于授权普通用户执行管理员命令。

使用sudo服务可以授权某个指定的用户去执行某些指定的命令，在满足工作需求的前提下尽可能少的放权，保证服务器的安全。配置sudo服务可以直接编辑配置文件/etc/sudoers。

`userdel username`

删除用户。

Linux系统中一切都是文件，用户信息被保存到了/etc/passwd、/etc/shadow以及/etc/group文件中，因此使用userdel命令实际就是帮助我们删除了指定用户在上述三个文件中的对应信息。

`userdel -r username`

删除用户，顺便删除该用户的主目录。

#### 3.4.3 用户组管理

> 组的增加、删除和修改实际上就是对**/etc/group**文件的更新。

`groupadd groupname`

创建新的用户组

`usermod -g newgroupname username`

user modify。更改用户所在的用户组。

`groupmod -n newname`

groupmod命令用于更改群组的识别码或名称时。不过大家还是要注意，用户名不要随意修改，组名和 GID 也不要随意修改，因为非常容易导致管理员逻辑混乱。如果非要修改用户名或组名，则建议大家先删除旧的，再建立新的。

#### 3.4.4 文件属性和权限

<u>***文件基本属性介绍***</u>

文件所有者**默认**为文件创建者，但是是可以修改的。

文件所属组**默认**为文件创建者所属组，但也是可以修改的。

![文件基本属性介绍](Linux/文件基本属性介绍.png)

<u>***文件权限10位介绍***</u>

- 第1位：  表示文件类型，`-`表示文件，`d`表示目录，`l`表示软链接，`b`表示供存储周边设备，`c`表示一次性读取装置
- 2-4位：  表示文件所有者的权限，即U（User）权限
- 5-7位：  表示文件所属组的成员的权限，即G（Group）权限
- 8-10位：表示文件所有者和所属组的成员之外的用户的权限，即O（Others）权限
- 2-10位：A（All）权限

<u>***rwx权限作用到文件和目录的不同效果***</u>

![rwx权限作用到文件和目录的不同效果](Linux/rwx权限作用到文件和目录的不同效果.png)

#### 3.4.5 chmod

chmod命令来自于英文词组”change mode“的缩写，其功能是用于改变文件或目录权限的命令。默认只有文件的所有者和管理员可以设置文件权限，普通用户只能管理自己文件的权限属性。

设置权限时可以使用数字法，亦可使用字母表达式，对于目录文件建议加入-R参数进行递归操作，意味着不仅对于目录本身，也对目录内的子文件/目录都进行新权限的设定。

<u>***方式一***</u>

`chmod {u/g/o/a}{+/-/=}{r/w/x} filename/dirname`

示例：

* `chmod u+r aText`：该文件权限第2位变为`r`

* `chmod u-r aText`：该文件权限第2位变为`-`

* `chmod u=r aText`：该文件权限第2位变为`r`，第3、第4位变为`--`

* `chmod a+r aText`：该文件权限第2、5、7位变为`r`

<u>***方式二***</u>

`chmod 八进制数字 filename/firname`

把9位权限以二进制表示，再转换成3位八进制。

示例：

* `chmod 777 aText`：该文件权限的第2-10位变为`rwxrwxrwx`，即`111111111`
* `chmod 644 aText`：该文件权限的第2-10位变为`rw-r--r--`，即`110100100`

#### 3.4.6 chown、chgrp

`chown 新属主 filename/dirname`

`chown 新属主:新属组 filename/dirname`

`chgrp 新属组 filename/dirname`

chmod、chown、chgrp这几个命令都可以加上`-R`参数，表示递归处理，将指定目录下的所有文件及子目录一并处理。

示例：

```
[root@hadoop100 桌面]# ll
total 4660
-rw-rw-rw-. 1 root root 4764088 Jun 23 22:11 aText
-rw-r--r--. 1 root root      38 Jun 24 13:54 info
[root@hadoop100 桌面]# chown user1 info
[root@hadoop100 桌面]# ll
total 4660
-rw-rw-rw-. 1 root  root 4764088 Jun 23 22:11 aText
-rw-r--r--. 1 user1 root      38 Jun 24 13:54 info
[root@hadoop100 桌面]# chgrp user2 info
[root@hadoop100 桌面]# ll
total 4660
-rw-rw-rw-. 1 root  root  4764088 Jun 23 22:11 aText
-rw-r--r--. 1 user1 user2      38 Jun 24 13:54 info
[root@hadoop100 桌面]# chown root:root info
[root@hadoop100 桌面]# ll
total 4660
-rw-rw-rw-. 1 root root 4764088 Jun 23 22:11 aText
-rw-r--r--. 1 root root      38 Jun 24 13:54 info
[root@hadoop100 桌面]# 
```

### 3.5 搜索查找类

#### 3.5.1 查找定位文件

`find [路径] [参数]`

不加路径则默认查找当前路径！

-name匹配名称（可用模式匹配，常用`*`通配符）

-user匹配所有者

-group匹配所有组

-size匹配文件的大小（+50KB为查找超过50KB的文件，而-50KB为查找小于50KB的文件）

`find /root -name info`

递归寻找/root目录下所能够匹配info（完全匹配）的文件和目录

`find /root -name *.txt`

递归寻找/root目录下所能够匹配"*.txt"的文件和目录

`find /root -user username`

不必多说

`find /root -user groupname`

不必多说

`find -size n[cwbkMG]`

>               File uses n units of space.  The following suffixes can be used:
>
>               `b'    for 512-byte blocks (this is the default if no suffix is used)
>
>               `c'    for bytes
>
>               `w'    for two-byte words
>
>               `k'    for Kilobytes (units of 1024 bytes)
>
>               `M'    for Megabytes (units of 1048576 bytes)
>
>               `G'    for Gigabytes (units of 1073741824 bytes)
>
>               The  size  does  not  count  indirect blocks, but it does count blocks in sparse files that are not actually allocated.
>               Bear in mind that the \`%k\' and \`%b\' format specifiers of -printf handle  sparse  files  differently.   The  \`b\'  suffix
>               always denotes 512-byte blocks and never 1 Kilobyte blocks, which is different to the behaviour of -ls.

`updatedb`

updatedb命令用来创建或更新 slocate/locate 命令所必需的数据库文件。

updatedb命令的执行过程较长，因为在执行时它会遍历整个系统的目录树，并将所有的文件信息写入 slocate/locate 数据库文件中。 注意：slocate 本身具有一个数据库，里面存放了系统中文件与目录的相关信息。

使用updatedb命令可以创建或更新locate所使用的数据库。如果数据库已经存在，它的数据是重复使用，以避免重读并没有改变的目录。updatedb通常每天由cron运行来更新默认的数据库。

`locate xxx`（模糊匹配）

locate命令的功能是用于快速查找文件或目录。与find命令进行全局搜索不同，locate命令是基于了数据文件（/var/lib/locatedb）进行的定点查找，由于缩小了搜索范围，因此快速快很多。

Linux系统需定期执行下updatedb命令对数据库文件进行更新，然后再使用locate命令进行查找，这样才会更加准确。

`which 命令名`

which命令的功能是用于查找命令文件，能够快速搜索二进制程序所对应的位置。如果我们既不关心同名文件（find与locate），也不关心命令所对应的源代码和帮助文件（whereis），仅仅是想找到命令本身所在的路径，那么这个which命令就太合适了。

`whereis 命令名`

which命令的功能是用于查找命令文件，能够快速搜索二进制程序所对应的位置。如果我们既不关心同名文件（find与locate），也不关心命令所对应的源代码和帮助文件（whereis），仅仅是想找到命令本身所在的路径，那么这个which命令就太合适了。

#### 3.5.2 内容过滤查找和管道操作

`grep -n 查找内容 文件名`

列出文件中所有的匹配行，显示行号。

grep来自于英文词组“global search regular expression and print out the line”的缩写，意思是用于全面搜索的正则表达式，并将结果输出。人们通常会将grep命令与正则表达式搭配使用，参数作为搜索过程中的补充或对输出结果的筛选，命令模式十分灵活。

与之容易混淆的是egrep命令和fgrep命令。如果把grep命令当作是标准搜索命令，那么egrep则是扩展搜索命令，等价于“grep -E”命令，支持扩展的正则表达式。而fgrep则是快速搜索命令，等价于“grep -F”命令，不支持正则表达式，直接按照字符串内容进行匹配。

`其他命令及其参数 | grep 各种参数`

“|”是管道符，也就是将前一个命令原本的输出内容输入到“管道”中，然后通过grep过滤后再输出。

这里的关键并不是grep，因为管道符并不一定要和grep相搭配。

`wc filename`

Word count。统计文件的行数、字节数

`其他命令及其参数 | wc`

将前一个命令原本的输出内容输入到“管道”中，然后通过wc过滤后再输出。

也就是统计前一个命令原本输出结果的行数、单词数、字节数

### 3.6 压缩解压类

#### 3.6.1 gzip / gunzip

`gzip filename`

Gnu zip，压缩文件。

* 只能压缩为.gz文件
* 只能压缩文件，不能压缩目录
* 不保留原来的文件
* 如果同时压缩多个文件，会产生多个压缩包

`gunzip filename.gz`

Gnu unzip，解压文件。只能解压.gz文件。

`zip zipfile.zip orifile/oridir`

压缩文件后缀为.zip（Windows系统常用）

* 会保留源文件
* 既可以压缩文件，也可以压缩目录
* 压缩目录时可以加上`-r`参数表示递归

* 压缩比没有gzip高

`unzip filename -d desdir`

unzip命令用于解压缩zip格式文件，虽然Linux系统中更多的使用tar命令进行对压缩包的管理工作，但有时也会收到同Windows系统常用的.zip和.rar格式的压缩包文件，unzip格式便派上了用场。直接使用unzip命令解压缩文件后，压缩包内原有的文件会被提取并输出保存到当前工作目录下。

* 加上-d参数指定解压目录

`tar`命令

| 常用参数 | 作用                                              | man手册解释                                                  |
| -------- | ------------------------------------------------- | ------------------------------------------------------------ |
| -c       | 建立新的归档文件，打包成.tar文件                  | -c, --create<br/>              create a new archive          |
| -x       | 从归档文件中提取文件，解包.tar文件                | -x, --extract, --get<br/>              extract files from an archive |
| -v       | 显示指令执行过程                                  | -v, --verbose<br/>              verbosely list files processed |
| -z       | 通过gzip指令压缩/解压缩文件，文件名最好为*.tar.gz | -z, --gzip<br/>              filter the archive through gzip |
| -f       | 指定文件名                                        | -f, --file=ARCHIVE<br/>              use archive file or device ARCHIVE |
| -C       | 指定目录                                          | -C, --directory=DIR<br/>              change to directory DIR |





