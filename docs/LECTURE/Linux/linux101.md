# Linux101
# 一. 前言
1. 服务器端的运维需要掌握 Linux（或者其他类 Unix 系统）的基本使用以及进程、设备、网络等系统相关的基本概念:
- Reference1: [USTC《Linux101》](https://101.lug.ustc.edu.cn/)
2. 如果需要学习某个具体的概念或工具，比如常见的运维的基础知识和教程，例如 Docker, Kubernetes, Linux, CI-CD, GitHub Actions...
- Reference2: [DevOps-Guide](https://github.com/Tikam02/DevOps-Guide)
3. 这是笔者的学习笔记，由于很多Linux运维知识已进行模块化学习，本教程旨在“将碎片编成网”，因此这份教程是对知识的梳理总结，面向非零基础同学
4. 这份教程主要参考[Linux101_USTC](https://101.lug.ustc.edu.cn/)（引用率超过90%），但会在此基础上补充一些针对Mac命令的说明
5. 建议学习时长：100h
# 二. 教程内容
## Chapter 1 - 计算机系统背景、Linux初介绍、环境配置
**Key Words：**
>OS / Unix / GNU / Linux Kernal / Linux发行版 / 虚拟机 / 服务器 / ssh远程连接 / WSL / Ubuntu环境配置 (Win + Apple Silicon)
### 1. 现代操作系统的功能
现代的计算机操作系统的功能已经十分复杂，远非当初所能比，且不同的操作系统可能在各方面都有着差异。但总体来说，其一直是==用户与底层硬件交流的桥梁==。*用户可以通过操作系统的用户界面向计算机发出命令，操作系统则对输入的命令进行解释并驱动相关的设备来实现用户的要求*。

通常来说，一个现代个人计算机操作系统会包含以下的功能：
- 进程管理：操作系统之上运行的各类程序都以进程的形式存在（有关进程的内容将在本书[第四章](https://101.lug.ustc.edu.cn/Ch04/)中展开），而通常计算机的中央处理器只有几个。为了能让许多进程并发执行，需要操作系统进行调度。
- 内存管理：内存是操作系统和进程用来临时存储数据的介质。计算机的内存是有限的，而且通常还是多层次的，因此操作系统需要合理分配和回收内存。
- 文件系统：为了管理计算机上的文件和数据，操作系统需要建立一个合适的数据结构来存储它们，即文件系统。
- 网络通信：为了能和互联网上的其它计算机进行有效的联络，一个公认的通信协议（如当今互联网主流的 TCP/IP）是必要的。操作系统需要有能力实现各种必需的网络通信方式。
- 安全机制：很多个人计算机和商业服务器上的数据都很敏感，因此操作系统必须配备一个安全机制用于保护数据不被未授权的人士获取，以及保护计算机免于各类计算机病毒的攻击。
- 用户界面：现代的个人计算机操作系统通常都会包含一个图形化的用户界面，从而方便用户与计算机打交道，并且提升用户体验。
- 驱动程序：驱动程序是直接与硬件交互的软件，使用驱动程序才能利用好计算机的硬件，因此操作系统要有能力与驱动程序对接以发挥硬件的功能。
### 2. Linux的起源
1969 年，美国 AT&T 公司的贝尔实验室开发了 UNIX 操作系统，并在此后的 10 年里在学术机构和大型企业中得到了广泛的应用。在这段时间，许多计算机从业者开发了很多基于 UNIX 的变种，统称为“类 UNIX 操作系统”。最初 AT&T 公司将 UNIX 的源码以低价甚至免费的许可授权给学术机构做研究和教学之用，然而后来它开始意识到了其商业价值，改变了之前的策略，取消了授权并对代码进行闭源，并对之前在 UNIX 之上研究出来的各类衍生组件和变种系统全部声明了著作权，随后开始了一场旷日持久的诉讼。虽然 UNIX 在此前 10 年在学界和业界起着十分重要的作用，但 AT&T 公司的这种行为对诸多使用 UNIX 和其变种的学术机构和商业厂家造成了巨大的负面影响，许多曾经的用户对 AT&T 公司的行径十分不满。

1983 年 9 月 27 日，理查德·斯托曼在麻省理工学院发起了 GNU 计划，它的目标是创建一套类似 UNIX 但完全自由的操作系统，因此这套系统不会包括任何 UNIX 的代码。在这个计划中诞生了之后十分有名的“GNU 通用公共许可证”（英文简称 GPL），这份许可证把使用该许可的软件的所有权利授予任何使用它的人。这种授权方式与通常的版权授权方式相左，它十分慷概地让出了几乎所有权利，让基于它的软件成为了自由且开源的软件，因此这种权利又被称为著作传（相对于通常的“著作权”）。

1991 年，正在大学内进修的林纳斯·托瓦兹对他使用的一个类 UNIX 操作系统 MINIX 十分不满，因为当时 MINIX 仅可用于教育但不允许任何商业用途。于是他在他的大学时期编写并发布了自己的操作系统，也就是后来所谓的 “Linux 内核”，成为了如今各类 Linux 发行版的基础。

Linux 内核并不是一个完整的操作系统，因为它过于精简，单单从它的功能上来说就已经不符合通常的现代的操作系统的定义了。为了能让这个内核拥有更多功能、完善的用户界面和更佳的使用体验，许多自由软件社区的开发人员和一些计算机商业公司便开始把各种组件添加到这个内核之上，这才构建成了一个完整的 Linux 操作系统。因为 Linux 内核是一个开源软件，所以通过这种方式组合出来的 Linux 操作系统会有许许多多的形式，不像 Windows 或者 macOS 这种受到公司统一规定的商业操作系统。正是因为开源社区的诸多成员以及许多商业公司的去中心化的贡献，让 Linux 充满了多样性。因为这种独特的属性，或许我们可以说 ==Linux 操作系统从来都不是指哪一种操作系统==。取而代之地，==为了指代某一个基于 Linux 内核构造出来的操作系统，我们通常都将其称之为“Linux 发行版”==。
#### 2.1 GNU/Linux & Android/Linux
GNU 计划最初是为了对抗 UNIX 而建立的，这可以从 GNU 的全称 “GNU's Not UNIX”（GNU 不是 UNIX）中十分直观地看出来。GNU 计划的创始人理查德·斯托曼希望通过这个计划开发一个自由的操作系统 GNU，他将 GNU 操作系统视作“达成社会目的的技术方法”。

虽然 GNU 计划产生出了许多自由开源的组件和软件，但其核心的 GNU 操作系统却至今没有开发完成。在实际操作中，GNU 计划的组件通常都会依赖 Linux 内核作为系统核心来承载它们，它们在一起合称 GNU/Linux。在这里，Linux 内核是功能核心，而 GNU 组件则是这块核心的外设，也是用户操作和使用 Linux 内核的工具。众多当今通用计算机和服务器上主流的 Linux 发行版都属于 GNU/Linux。

如今智能手机上一个常见的系统是谷歌公司开发的 Android，它其实也是一个基于 Linux 内核开发的操作系统。不过它没有采用 GNU 组件作为工具，而是谷歌自行研发的另一套体系。基于这套体系构成的组合则被称为 Android/Linux，它和我们接下来讨论的 GNU/Linux 大相径庭，感兴趣的读者可以参考本章的拓展阅读：[Android/Linux](https://101.lug.ustc.edu.cn/Ch01/supplement/#android-linux)。
#### 2.2 GNU自由软件
- gcc: GNU 的 C 和 C++ 编译器
- gdb: GNU 程序调试器
- gzip: gz 格式压缩与解压缩工具
- GNOME: 隶属于 GNU 项目的桌面环境
- gimp: GNU 图像编辑工具

它们的首字母 g 都是 GNU 的缩写（当然不是所有以 g 开头的都是 GNU 软件）。许多 Linux 上的系统管理命令虽然未必以 g 开头，但都属于自由软件；还有[更多优秀的软件](https://www.gnu.org/software/)，被自由软件爱好者维护、分享……选择 Linux，很大程度上是一种对极客精神与开源文化的认同。
### 3. Linux 发行版
一个典型的 Linux 发行版除了 Linux 内核以外，通常还会包括一系列 GNU 工具和库、一些附带的软件、说明文档、一个桌面系统、一个窗口管理器和一个桌面环境。不同的发行版之间除了 Linux 内核以外的其它部分都有可能不一样，因此有的时候我们对比某两种发行版的时候会觉得它们看起来像是完全不一样的操作系统，然而实质上它们却拥有着相同的核心，即 Linux 内核。

这里给读者介绍若干桌面和服务器环境中主流的发行版分支：
- Debian 分支：debian + ubuntu
- Red Hat 分支：Red Hat Enterprise Linux（RHEL）+ Fedora + CentOS
- Arch Linux 分支：Arch Linux + Manjaro

==回到本章的标题：什么是 Linux？==这个问题在不同的语境下有不同的答案：它可以指代 Linux 内核，也可能指代一个或者多个 Linux 发行版。在日常领域或是作为新手接触到的情境来看，这个词通常都是指代后者，而且往往指的是 GNU/Linux 的发行版。
### 4. 配置与使用 SSH 连接远程的 Linux 服务器
在本章节正文中，我们介绍了如何在本地运行 Linux 操作系统。但是在实际操作时，一个非常常见的需求是连接到远程的服务器。Linux 提供了非常方便安全的 SSH 功能，可以让用户连接到远程的 Linux 服务器命令行执行操作。

在这里，我们将简单介绍在服务器上配置 SSH，以及如何使用 SSH 连接到远程的服务器：
#### 4.1 服务器配置
```bash
$ sudo apt install openssh-server
```

```bash
$ sudo systemctl start ssh
$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-02-24 16:47:39 CST; 1min 15s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1689 (sshd)
      Tasks: 1 (limit: 2250)
     Memory: 2.0M
     CGroup: /system.slice/ssh.service
             └─1689 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups

2月 24 16:47:39 ustclug-linux101 systemd[1]: Starting OpenBSD Secure Shell server...
2月 24 16:47:39 ustclug-linux101 sshd[1689]: Server listening on 0.0.0.0 port 22.
2月 24 16:47:39 ustclug-linux101 sshd[1689]: Server listening on :: port 22.
2月 24 16:47:39 ustclug-linux101 systemd[1]: Started OpenBSD Secure Shell server.
```

```bash
$ ssh ustc@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:czt1KYx+RIkFTpSPQOLq+GqLbLRLZcD1Ffkq4Z3ZR2U.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
ustc@localhost's password:
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.13.0-28-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 updates can be applied immediately.

Your Hardware Enablement Stack (HWE) is supported until April 2025.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ustc@ustclug-linux101:~$
```
#### 4.2 服务器IP地址获取
- 可以使用 `ip a` 命令查看服务器的 IP 地址
- 如果无法连接，请检查服务器的防火墙是否放行了 TCP 22 端口
#### 4.3 配置密钥登录
弱密码会导致黑客能够轻而易举从 SSH 入侵服务器，但是每次登录输入复杂密码会很烦，怎么办呢？其实，SSH 提供了一种相当方便、简单、安全的连接方式：密钥认证。它的原理是，用户生成一对密钥，将公钥放在服务器上，登录时 SSH 自动使用私钥认证，两者相符则允许用户登录。

1. 在自己的机器上使用 `ssh-keygen` 生成密钥：
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ustc/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ustc/.ssh/id_rsa
Your public key has been saved in /home/ustc/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:/+4tXnjnilyLQvwa+qEKx0IK2jOzHRj0Nbarr3Vot1E ustc@ustclug-linux101
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|                 |
|  .   +          |
| . . o o         |
|. . o . S.E      |
|.o = . o oo  .   |
|. B + B +.+.. + .|
|   * O o =.=oB + |
|  . +oo.+.o*O.+..|
+----[SHA256]-----+

# 这里的 passphrase 是密钥的密码，设置之后即使私钥被别人拿到也无法使用，可以不输入。最终得到的 id_rsa 是私钥（千万不要分享给别人！），id_rsa.pub 是公钥（可以公开）
```

2. 在本地使用 `ssh-copy-id` 命令将公钥拷贝到服务器上：
```bash
$ ssh-copy-id ustc@localhost
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ustc@localhost's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ustc@localhost'"
and check to make sure that only the key(s) you wanted were added.

# 如果服务器不允许使用密码登录，可以将用户目录下 `.ssh/id_rsa.pub` 文件的内容复制到机器对应用户的 `.ssh/authorized_keys` 文件中
```

3. 配置完成后，可以考虑关闭 SSH 服务器的密码验证。做法是编辑 `/etc/ssh/sshd_config` 文件，进行如下修正：
```bash
#PasswordAuthentication yes                 （ origin ）
->
PasswordAuthentication no                   （   now  ）
# 建议除非有特殊原因，否则所有正式生产环境服务器（例如实验室服务器）都应该关闭 SSH 密码验证
```

4. 让 SSH 服务器重新加载配置
```bash
$ sudo systemctl reload ssh
```

>PS: 在本地进行ssh连接的配置指南，见笔者的另一文档：[SSH公私钥连接指南.md]

### 5. 在使用 Apple Silicon 处理器的机型上配置 Linux 虚拟机
**什么是 Apple Silicon？**
Apple Silicon（苹果硅）是对苹果公司使用 ARM 架构设计的单芯片系统（SoC）和封装体系（SiP）处理器之总称。它广泛运用在 iPhone、iPad、Mac 和 Apple Watch 以及 HomePod 和 Apple TV 等苹果公司产品

若想查看你的 Mac 是否使用了 Apple Silicon，请参照[这个网页](https://support.apple.com/en-us/HT211814)

**x86-64 架构和 ARM64 两种架构都是什么？它们有什么区别？**
ARM64（也被称为 AArch64）是 ARM 公司推出的 64 位处理器架构。它被广泛用于移动设备、嵌入式系统和服务器领域。与之前的 32 位架构相比，它具有更高的性能和更好的功耗管理。

x86-64 也被称为 x64 或者 AMD64。它广泛应用于 PC 和服务器领域，并且兼容大部分之前的 32 位 x86 应用程序。

在使用上，这两种架构是不兼容的，即针对一种架构编译的程序无法直接在另一种架构上运行。这也是使用 Apple Silicon 处理器的 Mac 无法通过正文中的流程配置 Ubuntu 虚拟机的原因

**配置方式**
《Linux101》原书中采取的是[VMWare](https://www.vmware.com/products/fusion/fusion-evaluation.html)虚拟机配置，==笔者采用的是基于[Orbstack](https://orbstack.dev/)的配置==
很显然此处省略了针对Orbstack的配置

**对比 VMWare and Orbstack**

| VMWare | Orbstack |
| :--: | :--: |
| 可以显示图形化界面（桌面端） | 只能显示终端命令行（CLI） |
| 有针对不同原系统的发行版（Windows + Mac） | 仅限Macintosh用户（Only Apple Silicon） |
| 太占内存，很“冗余”（50 G+/-） | 极其轻量级（～100 Mb） |
## Chapter 2 - 个性化配置
**Key Words:**
>shell / 桌面环境
### 1. 什么是 shell
上面所说的命令行，实际上指的就是 shell

shell 是 Linux 中的一类程序，它可以接受通过键盘输入的命令，然后把命令交给系统执行，并将命令的输出返回给用户。现在几乎所有的 Linux 发行版都提供了一个叫 Bash 的 shell 程序，是传统 shell 的“增强版”。
### 2. 桌面环境的选择
Linux 的桌面环境可不止一种，下面介绍几个流行的桌面环境:

1. KDE Plasma
KDE 软件社区提供的 Plasma Linux 桌面环境是最可定制的图形桌面环境之一。此功能丰富且功能强大的桌面环境还拥有许多小部件。它允许用户自由地添加桌面的控制面板。 [Plasma 官方网站](https://www.kde.org/plasma-desktop)

2. GNOME
GNOME 的设计目标是为用户提供简单性，易于访问性和可靠性。正因为这些，GNOME 得到了普及。 [GNOME 官网](https://www.gnome.org/)

3. Xfce
Xfce 是一款快速、轻量，界面美观和对用户友好的桌面环境。 [Xfce 官网](https://www.xfce.org/)
## Chapter 3 - 软件安装与文件操作
**Key Words:**
>包管理系统 / 安装包 / 文件操作 / 模式匹配 / 文件(解)压缩
### 1. 软件安装
#### 1.1 使用应用商店安装软件 (software)
在 Ubuntu 下，我们可以使用 Ubuntu Application Store 来进行安装
#### 1.2 使用包管理系统安装软件 (software)
**1）软件包管理器是一系列工具的集合，它自动化地完成软件的安装、更新、配置和移除功能**

软件包管理器的一个重要组成部分是软件仓库。软件仓库是收藏了互联网上可用软件包（应用程序）的图书馆，里面往往包含了数万个可供下载和安装的可用软件包。有了软件仓库，我们不需要手动下载大量的软件包再通过包管理器安装。只需要知道软件在软件仓库中的名称，即可让包管理器从网络中抓取到相应的软件包到本地，自动进行安装。

==包管理系统==有很多，比如管理 Debian (.deb) 软件包的 `dpkg` 以及它的前端 `apt`（用于 Debian 系的发行版）；`rpm` 包管理器以及它的前端 `dnf`（用于 Fedora 和新版的 CentOS 和 RHEL）、前端 `yum`（用于 CentOS 7 和 RHEL 7 等）；`pacman` 包管理器（用于 Arch Linux 和 Manjaro）......

**2）apt 的全称是 Advance Package Tool，是一个处理在 Debian、Ubuntu 或者其他衍生发行版的 Linux 上安装和移除软件的自由软件**

搜索：
```bash
apt search <...>
```
安装：
```bash
apt install <...>

# 如果当前用户的权限无法满足安装软件所需的权限：在命令前面添加 sudo
```

**3）在计算机本地，系统会维护一个包列表，在这个列表里面，包含了软件信息以及软件包的依赖关系，在执行 `apt install` 命令时，会从这个列表中读取出想要安装的软件信息，包括下载地址、软件版本、依赖的包，同时 apt 会对依赖的包递归执行如上操作，直到不再有新的依赖**

更新软件列表：
```bash
# 为了将这个列表进行更新，就会用到 `apt update` 命令。获取到新的软件版本、软件依赖关系
sudo apt update
```
更新软件：
```bash
# 根据软件列表中的版本信息与当前安装的版本进行对比，解决新的依赖关系，完成升级
sudo apt upgrade
```
#### 1.3 使用包管理器手动安装软件包 (software package)
**软件包**
软件包是将软件安装升级中需要地多个数据文件合并得到的一个单独文件，便于传输和减少存储空间。软件包中包括了所有需要的元数据，如软件的名称、软件的说明、版本号以及要运行这个软件所需要的依赖包等

**安装方法**
安装软件包需要相应的软件包管理器，deb 格式的软件包对应的是 `dpkg`。

相对于 `apt` 而言，`dpkg` 会更加底层，`apt` 是一个包管理器的前端，并不直接执行软件包的安装工作，相反的则是交由 `dpkg` 完成。`dpkg` 反馈的依赖信息则会告知 `apt` 还需要安装的其他软件包，并从软件仓库中获取到相应的软件包进行安装，从而完成依赖管理问题。

直接通过 `dpkg` 安装 deb 并不会安装需要的依赖，只会报告出相应的依赖缺失了。
```bash
# dpkg -i 安装 software package
sudo dpkg -i (xxx).deb
```
### 2. 操作文件与目录
#### 2.1 cat 查看文件
```bash
$ # 输出 FILE 文件的全部内容
$ cat [OPTION] FILE
```
1. 输出 file.txt 的全部内容 : `$ cat file.txt`
2. 查看 file1.txt 与 file2.txt 连接后的内容 : `$ cat file1.txt file2.txt`
#### 2.2 less 查看文件
```bash
$ # 在可交互的窗口内输出 FILE 文件的内容
$ less FILE
```
**less 和 cat 的区别**在于，cat 会一次性打印全部内容到终端中并退出，而 less 一次只显示一页，且支持向前/后滚动、搜索等功能。如果要在一个大文件中（例如 man page）查找一部分内容，less 通常要比 cat 方便得多
#### 2.3 编辑文件
1) nano: 启动后，用户可以直接开始输入需要的内容，使用方向键移动光标。在终端最下方是 nano 的快捷键，`^` 代表需要按下 Ctrl 键（例如，`^X` 就是需要同时按下 Ctrl + X）。在编辑完成后，按下 Ctrl + X，确认是否保存后即可
2) vim: 详见笔者的另一份笔记《Vim配置指南》
```bash
nano file.txt
vim file.txt
```
#### 2.4 赋值文件和目录
```bash
$ # 将 SOURCE 文件拷贝到 DEST 文件，拷贝得到的文件即为 DEST
$ cp [OPTION] SOURCE DEST

$ # 将 SOURCE 文件拷贝到 DIRECTORY 目录下，SOURCE 可以为不止一个文件
$ cp [OPTION] SOURCE... DIRECTORY
```

|选项|含义|
|---|---|
|`-r`, `-R`, `--recursive`|递归复制，常用于==复制目录== |
|`-f`, `--force`|==覆盖==目标地址==同名==文件 |
|`-u`, `--update`|仅当源文件比目标文件==新才进行复制== |
|`-l`, `--link`|创建硬链接|
|`-s`, `--symbolic-link`|创建软链接|
```bash
# examples of COPY(cp)
将 file1.txt 复制一份到同目录，命名为 file2.txt
    $ cp file1.txt file2.txt
将 file1.txt、file2.txt 文件复制到同目录下的 file 目录中
    $ cp file1.txt file2.txt ./file/
将 dir1 文件夹及其所有子文件复制到同目录下的 test 文件夹中    
    $ cp -r dir1 ./test/
```
#### 2.5 移动文件和目录
```bash
$ # 将 SOURCE 文件移动到 DEST 文件
$ mv [OPTION] SOURCE DEST

$ # 将 SOURCE 文件移动到 DIRECTORY 目录下，SOURCE 可以为多个文件
$ mv [OPTION] SOURCE... DIRECTORY
```

|选项|含义|
|---|---|
|`-f`, `--force`|覆盖目标地址同名文件|
|`-u`, `--update`|仅当源文件比目标文件新才进行移动|
#### 2.6 删除文件和目录
```bash
$ # 删除 FILE 文件，FILE 可以为多个文件。 $ # 如果需要删除目录，需要通过 -r 选项递归删除目录 $ rm [OPTION] FILE...

# My Preference: OPTION='-rf'
```

|选项|含义|
|---|---|
|`-f`, `--force`|无视不存在或者没有权限的文件和参数|
|`-r`, `-R`, `--recursive`|递归删除目录及其子文件|
|`-d`, `--dir`|删除空目录|
```bash
1. 删除 file1.txt 文件：   $ rm file1.txt
2. 删除 test 目录及其下的所有文件：   $ rm -r test/
3. 删除 test1/、test2/、file1.txt 这些文件、目录。其中，这些文件或者目录可能不存在、写保护或者没有权限读写：   $ rm -rf test1/ test2/ file1.txt
```
#### 2.7 创建目录与文件
```bash
$ # 创建一个目录，名为 DIR_NAME
$ mkdir [OPTION] DIR_NAME...
```

```bash
$ # 创建一个文件，名为 FILE_NAME
$ touch FILE_NAME...
```

**Ques: 创建文件为什么名字叫 touch 而非 create（或者类似意思的单词）呢？**
touch 工具实际上的功能是修改文件的访问时间（access time, atime）和修改时间（modification time, mtime），可以当作是摸（touch）了一下文件，使得它的访问与修改时间发生了变化。当文件不存在时，touch 会创建新文件，所以创建文件也就成为了 touch 最常见的用途

`stat` 命令可以显示文件的属性信息，可以来看看 touch 对已有文件的操作：
```bash
$ touch test  # 创建文件 test
$ stat test  # 查看信息
File: test
Size: 0             Blocks: 0          IO Block: 4096   regular empty file
Device: 801h/2049d  Inode: 1310743     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/     ustc)   Gid: ( 1000/     ustc)
Access: 2022-02-25 18:12:28.403981478 +0800
Modify: 2022-02-25 18:12:28.403981478 +0800
Change: 2022-02-25 18:12:28.403981478 +0800
Birth: 2022-02-25 18:12:28.403981478 +0800

$ # 等待一段时间
$ touch test  # 「摸」一下文件 test
$ stat test  # 查看 touch 之后的信息
  File: test
Size: 0             Blocks: 0          IO Block: 4096   regular empty file
Device: 801h/2049d  Inode: 1310743     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/     ustc)   Gid: ( 1000/     ustc)
Access: 2022-02-25 18:15:16.625288185 +0800
Modify: 2022-02-25 18:15:16.625288185 +0800
Change: 2022-02-25 18:15:16.625288185 +0800
Birth: 2022-02-25 18:12:28.403981478 +0800
$ # 可以看到 Access time 和 Modify time 变化了。
```
#### 2.8 搜索目录与文件
```bash
$ # 在 PATH（路径）中根据 EXPRESSION（表达式）搜索文件
$ find [OPTION] PATH [EXPRESSION]
```

| EXPRESSION选项    | 对应含义                                     |
| --------------- | ---------------------------------------- |
| `-name '*.ext'` | 文件名后缀为 ext。其中 `*` 是任意匹配符                 |
| `-type xxx`     | 文件类型为目录(xxx=d)，其他的类型例如 xxx=f（普通文件）       |
| `-size +1M`     | 大于 1M 的文件，`+` 代表大于这个大小，对应地，`-` 代表小于之后的大小 |
| `-or`           | 或运算符，代表它前后两个条件满足一个即可                     |
```bash
1. 在当前目录搜索名为 report.pdf 的文件：
$ find . -name 'report.pdf'

2. 全盘搜索大于 1G 的文件：
$ find / -size +1G

3. 在用户目录搜索所有名为 node_modules 的文件夹：
$ find ~/ -name 'node_modules' -type d
```

#### 2.9 模式匹配
许多现代的 shell 都支持一定程度的模式匹配。举个例子，bash 的匹配模式被称为 [glob](https://mywiki.wooledge.org/glob)，支持的操作如下：

|模式|匹配的字串|
|---|---|
|`*`|任意字串|
|`foo*`|匹配 foo 开头的字串|
|`*x*`|匹配含 x 的字串|
|`?`|一个字符|
|`a?b`|`acb`、`a0b` 等，但不包含 `a00b`|
|`*.[ch]`|以 .c 或 .h 结尾的文件|
和上面提到的命令结合，可以显著提高操作效率。例如：

| 命令 | 效果 |
| ---- | ---- |
| `rm *.tar.gz` | 删除所有以 `tar.gz` 结尾的压缩文件 |
| `mv *.[ch] /path` | 将当前及子目录下所有以 `.c` 或 `.h` 结尾的代码移动到 `/path` |
| rm -rf ** | 删除当前工作目录下所有文件 |
除了上面提到的 glob，bash 还支持 [extglob](https://www.linuxjournal.com/content/bash-extended-globbing)，不过需要先用 `shopt -s extglob` 启用。
其他 shell 例如 zsh 还支持正则表达式（会在后续章节讲解）。
### 3. 使用tar存档/压缩文件
**Process：**
lots of files => tar => 存档文件(a set of files) => gzip/bzip2 => 压缩文件

**Model：**
```bash
$ # 命令格式如下，请参考下面的使用样例了解使用方法
$ tar [OPTIONS] FILE...
```
常用选项：

|OPTIONS选项 |含义|
|---|---|
|`-A`|将一个存档文件中的内容追加到另一个存档文件中|
|`-r`, `--append`|将一些文件追加到一个存档文件中|
|`-c`, `--create`|从一些文件==创建存档文件== |
|`-t`, `--list`|列出一个存档文件的内容|
|`-x`, `--extract, --get`|从存档文件中提取出文件|
|`-f`, `--file=ARCHIVE`|==使用指定的存档文件== |
|`-C`, `--directory=DIR`|指定输出的目录|
添加压缩选项可以使用压缩算法进行创建压缩文件或者解压压缩文件：

|选项|含义|
|---|---|
|`-z`, `--gzip`, `--gunzip`, `--ungzip`|使用 gzip 算法处理存档文件|
|`-j`, `--bzip2`|使用 bzip2 算法处理存档文件|
|`-J`, `--xz`|使用 xz 算法处理存档文件|
**tar 使用实例**(==常用==)
- 将 file1、file2、file3 打包为 target.tar：
    `$ tar -c -f target.tar file1 file2 file3`
- 将 target.tar 中的文件提取到 test 目录中：
    `$ tar -x -f target.tar -C test/`
- ==将 file1、file2、file3 打包，并使用 gzip 算法压缩，得到压缩文件 target.tar.gz ==：
    `$ tar -cz -f target.tar.gz file1 file2 file3` （*-cz的解释见“组合tar的选项”*）
- ==将压缩文件 target.tar.gz 解压到 test 目录中==：
    `$ tar -xz -f target.tar.gz -C test/`
- 将 archive1.tar、archive2.tar、archive3.tar 三个存档文件中的文件追加到 archive.tar 中
    `$ tar -Af archive.tar archive1.tar archive2.tar archive3.tar`
- 列出 target.tar 存档文件中的内容
```bash
$ tar -t -f target.tar  
$ # 打印出文件的详细信息 
$ tar -tv -f target.tar
```

**组合 tar 的选项**
与大部分 Linux 命令相同，tar 命令允许将多个单字母（使用单个 `-` 符号的）选项组合为一个参数，便于用户输入。例如，以下命令是等价的：
```bash
$ tar -c -z -v -f target.tar test/ 
$ tar -czvf target.tar test/ 
$ tar -f target.tar -czv test/
```

**存档文件的后缀名**
后缀名并不能决定文件类型，但后缀名通常用于帮助人们辨认这个文件的可能文件类型，从而选择合适的打开方法:
1. 在第一个例子中，创建得到的文件名为 `target.tar`，后缀名为 `tar`，表示这是一个没有进行压缩的存档文件
2. 在第二个例子中，创建得到的文件名为 `target.tar.gz`。将 `tar.gz` 整体视为后缀名，可以判断出，为经过 gzip 算法压缩（`gz`）的存档文件（`tar`）。可知在提取文件时，需要添加 `-z` 选项使其经过 gzip 算法处理后再进行正常 tar 文件的提取
同样地，通过不同压缩算法得到的文件应该有不同的后缀名，以便于选择正确的参数。如经过 `xz` 算法处理得到的存档文件，其后缀名最好选择 `tar.xz`，这样可以知道为了提取其中的文件，应该添加 `--xz` 选项

**为什么使用 tar 创建压缩包需要“两次处理”**
tar 名字来源于英文 **t**ape **ar**chive，原先被用来向只能顺序写入的磁带写入数据。

tar 格式本身所做的事情非常简单：把所有文件（包括它们的“元数据”，包含了文件权限、时间戳等信息）放在一起，打包成一个文件。**注意，这中间没有压缩的过程！**

为了得到更小的打包文件，方便存储和网络传输，就需要使用一些压缩算法，缩小 tar 文件的大小。这就是 tar 处理它自己的打包文件的逻辑。在 Windows 下的一部分压缩软件中，为了获取压缩后的 tar 打包文件的内容，用户需要手动先把被压缩的 tar 包解压出来，然后再提取 tar 包中的文件。
### 4. 查看命令指南
#### 4.1 man 命令
```bash
# 得到大部分安装在 Linux 上的软件的用户手册
man (xxx)
```

大部分软件在安装时会将它的软件手册安装在系统的特定目录， `man` 命令就是读取并展示这些手册的命令。在软件手册中，会带有软件的每一个参数的含义、退出值含义、作者等内容，大而全。但一般较少带有使用样例，需要根据自身需要拼接软件参数
#### 4.2 tldr 命令
```bash
# 精简版文档，通过几个简单的例子让用户可以快速地一窥软件的使用方法
tldr (xxx)
```
### 5. tar 的替代与其他压缩软件
#### 5.1 unar
unar 是 macOS 上的软件 [The Unarchiver](https://theunarchiver.com/) 的命令行工具，能够同样用于 Windows 和 Linux。
软件介绍：[Unar and Lsar | Command Line Tools for The Unarchiver](https://theunarchiver.com/command-line)。

Ubuntu 上直接使用 apt 安装即可：`$ sudo apt install unar`

对于其他 Linux 发行版，请参照网站的介绍，安装合适的依赖以进行编译安装。
安装之后会得到两个命令：`unar` 和 `lsar`，分别用来解压存档文件以及浏览存档文件内容：
```bash
$ unar archive.zip -o output # 将存档文件提取到 output 文件夹中 
$ lsar archive.zip           # 浏览存档文件内容 
$ lsar -l archive.zip        # 查看详细信息 
$ lsar -L archive.zip        # 查看特别详细的信息
```
#### 5.2 处理 ZIP 压缩包：`zip` 与 `unzip`
`zip` 和 `unzip` 工具分别负责 ZIP 压缩包的压缩与解压缩，使用以下命令安装：
```bash
$ sudo apt install zip unzip
```

以下提供一些命令例子，更多的功能需要查看对应的文档：
```bash
$ zip -r archive.zip path/file1 path/dir1  # （递归地）压缩文件和目录 
$ zip archive.zip path/file2               # 添加文件到已有的压缩包 
$ unzip archive.zip                        # 解压缩 
$ unzip archive.zip -d path/               # 解压缩到指定目录 
$ unzip -l archive.zip                     # 浏览压缩包内容`
```
## Chapter 4 - 进程、前后台、服务、例行性服务
### 1. 进程
在导言中提到的「桌面环境、浏览器、聊天软件、办公软件、游戏、终端，以及后台运行着的系统服务」，它们都是进程

不太严谨地说，进程就是正在运行的程序：当我们==启动一个程序==的时候，操作系统会**从硬盘中读取程序文件**，将程序**内容加载入内存**中，之后 **CPU 就会执行**这个程序

进程是现代操作系统中必不可少的概念。在 Windows 中，我们可以使用「任务管理器」查看系统运行中的进程；Linux 中同样也有进程的概念。下面我们简单介绍 Linux 中的进程：     
#### 1.1 查看当前运行的进程
##### 1.1.1 htop
Htop 可以简单方便查看==当前运行的所有进程==，以及系统 CPU、内存占用情况与系统负载等信息

使用鼠标与键盘都可以操作 htop。Htop 界面的最下方是一些选项，使用鼠标点击或按键盘的 F1 至 F10 功能键可以选择这些功能，常用的功能例如搜索进程（F3, Search）、过滤进程（F4, Filter，使得界面中只有满足条件的进程）、切换树形结构/列表显示（F5, Tree/List）等等
##### 1.1.2 ps
ps（**p**rocess **s**tatus）是常用的输出进程状态的工具
直接调用 `ps` 仅会显示==本终端中运行的相关进程==。如果需要显示所有进程，对应的命令为 `ps aux`
#### 1.2 进程标识符
首先，有区分才有管理：**进程标识符**（PID，Process Identifier（是一个数字，是进程的唯一标识。在 htop 中，最左侧一列即为 PID。当用户想挂起、继续或终止进程时可以使用 PID 作为索引。

在 htop 中，直接单击绿色条内的 PID 栏，可以将进程顺序按照 PID 升序排列，再次点击为降序排列，同理可应用于其他列

>**Linux 进程启动顺序**
>按照 PID 排序时，我们可以观察系统启动的过程：
>1. Linux 系统内核从引导程序接手控制权后，开始==内核初始化==，随后变为 **init_task**，初始化自己的 ==PID 为 0==。
>2. 随后创建出 ==1 号进程==（init 程序，目前一般为 systemd）衍生出==用户空间的所有进程==，创建 ==2 号进程== kthreadd 衍生出所有==内核线程==。
>3. 随后 ==0 号进程成为 idle 进程==，1 号、2 号并非特意预留，而是产生进程的自然顺序使然。
由于 kthreadd 运行于内核空间，故需按大写 K（Shift + k）键显示内核进程后才能看到。然而无论如何也不可能在 htop 中看到 0 号进程本体，只能发现 1 号和 2 号进程的 PPID 是 0
#### 1.3 进程优先级与状态
我们平时使用操作系统的时候，可能同时会开启浏览器、聊天软件、音乐播放器、文本编辑器……前面提到它们都是进程，但是单个 CPU 核心一次只能执行一个进程。为了让这些软件看起来「同时」在执行，操作系统需要用非常快的速度将计算资源在这些进程之间切换，这也就引入了进程优先级和进程状态的概念
##### 1.3.1 优先级与 nice 值[¶](https://101.lug.ustc.edu.cn/Ch04/#nice "Permanent link")
有了进程，谁先运行？谁给一点时间就够了，谁要占用大部分 CPU 时间？这又是如何决定的？这些问题之中体现着优先权的概念。如果说上面所介绍的的那些进程属性描述了进程的控制信息，那么**优先级**则反映操作系统调度进程的手段。

在 htop 的显示中有两个与优先级有关的值：Priority（PRI）和 **nice（NI）**。

**NI**
==Nice 值越高==代表一个进程==对其它进程越 "nice"（友好）==，对应的==优先级也就更低==。Nice 值最高为 19，最低为 -20。**通常，我们运行的程序的 nice 值为 0**。我们可以打开 htop 观察与调整每个进程的 nice 值。

用户可以使用 `nice` 命令在运行程序时指定优先级，而 `renice` 命令则可以重新指定优先级。当然，若想调低 nice 值，还需要 `sudo`（毕竟不能随便就把自己的优先级设置得更高，不然对其他的用户不公平）。

**PRI**
如果你在 htop 中测试调整进程的 nice 值，可能会发现一个公式：`PRI = nice + 20`。这对于普通进程是成立的——普通进程的 PRI 会被映射到一个非负整数。

但在正常运行的 Linux 系统中，我们可能会发现有些进程的 PRI 值是 RT，或者是负数。这表明对应的进程有更高的实时性要求（例如内核进程、音频相关进程等），采用了与普通进程不同的调度策略，优先级也相应更高。
##### 1.3.2 进程状态
配合上面的进程调度策略，我们可以粗略地将进程分为三类：

1. 正在运行的程序，即处于**运行态**（running）；[可运行 + 在运行]
2. 可以运行但正在排队等待的程序，即处于**就绪态**（ready）；[可运行 + 在排队]
3. 正在等待其他资源（例如网络或者磁盘等）而无法立刻开始运行的程序，即处于**阻塞态**（blocked）[不可运行]

调度时操作系统轮流选择可以运行的程序运行，构成就绪态与运行态循环。运行中的程序如果需要其他资源，则进入阻塞态；阻塞态中的程序如果得到了相应的资源，则进入就绪态。

在实际的 Linux 系统中，进程的状态分类要稍微复杂一些。在 htop 中，按下 H 键到帮助页，可以看到对进程状态的如下描述：

`Status: R: running; S: sleeping; T: traced/stopped; Z: zombie; D: disk sleep`

其中 R 状态对应上文的运行和就绪态（即表明该程序可以运行），S 和 D 状态对应上文阻塞态。

需要注意的是：
- S 对应的 sleeping 又称 interruptible sleep，字面意思是「可以被中断」；
- D 对应的 disk sleep 又称 uninterruptible sleep，不可被中断，一般是因为阻塞在磁盘读写操作上。
- Zombie 是僵尸进程，该状态下进程已经结束，只是仍然占用一个 PID，保存一个返回值。
- traced/stopped 状态正是下文使用 Ctrl + Z 导致的挂起状态（大写 T），或者是在使用 gdb 等调试（Debug）工具进行跟踪时的状态（小写 t）。

|状态|缩写表示|说明|
|---|---|---|
|Running|R|正在运行/可以立刻运行|
|Sleeping|S|可以被中断的睡眠|
|Disk Sleep|D|不可被中断的睡眠|
|Traced / Stopped|T|被跟踪/被挂起的进程|
|Zombie|Z|僵尸进程|
### 2. 用户进程控制
要想控制进程，首先要与进程对话，那么必然需要了解进程间通信机制。由于进程之间不共享内存空间，也就无法直接发送信息，必须要操作系统帮忙，于是**信号机制**就产生了。

**信号（signal）** 是 Unix 系列系统中进程之间相互通信的一种机制。发送信号的 Linux 命令叫作 `kill`。被称作 "kill" 的原因是：早期信号的作用就是关闭（杀死）进程

信号列表可以使用 `man 7 signal` 查看，下面展示常用signal：

| Signal | Meaning | Order As |
| :--: | :--: | :--: |
| SIGINT | 终止进程（别干了，一边歇着） | Ctrl -C |
| SIGTERM | 终止进程（你该dead了） | kill < PID > |
| SIGKILL | 终止进程（你现在必须dead） | kill -9 < PID > |
| SIGSTOP | 停止进程（成为植物人吧） | Ctrl -Z |
| SIGCONT | 继续进程（植物人复苏） | fg / bg |
#### 2.1 前后台切换
1. 使用场景：
> 在 shell 中**直接运行命令，将挂到前台**，而如果不希望无力地看着屏幕输出不能做其他事情，那么便需要将程序切换到后台了

2. 直接后台运行：
> 默认情况下，在 shell 中运行的命令都在前台运行，如果需要在后台运行程序，需要在最后加上 `&`：

3. 前台->后台：
>如果需要将前台程序切换到后台，则需要按下 Ctrl + Z 发送 SIGTSTP 使进程挂起，控制权还给 shell，此时屏幕输出如下所示，即（刚才挂起的进程）代号为 2，状态为 stopped，命令为 `ping localhost`

4. 查看当前shell上所有进程：
> 可以使用 `jobs` 命令，看到当前 shell 上所有相关的进程

5. 指定使用对象：
> 任务前的代号在 fg，bg，乃至 kill 命令中发挥作用。使用时需要在前面加 `%`，如将 2 号进程放入后台，则使用 `bg %2`
#### 2.2 终止进程
正如上所述，许多信号都会引发进程的终结，然而标准的终止进程信号是 SIGTERM，意味着一个进程的自然死亡。
##### 2.2.1 在 htop 中发送信号
htop 中自带向进程发送信号的功能。按下 K 键，在左侧提示栏中选择需要的信号，按下回车发送。同时可以使用空格对进程进行标记，被标记的进程将改变显示颜色。此时重复上述过程，可对被标记进程批量发送信号。
##### 2.2.2 kill
如前所述，Linux 上最常用的发送信号的程序就是 kill
```bash
$ kill -l # 显示所有信号名称
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```
如果不加任何参数，只有 PID，`kill` 命令将自动使用 15（SIGTERM）作为信号参数

**PS: 立刻结束进程**
>在信号中，9 代表 SIGKILL，收到这个信号之后，程序会立刻退出。
在使用时，直接 `kill -9 PID` 即可。

**PS: kill命令审查**
>`man builtins` 可以查看 bash 中的内建命令文档。使用 `type <命令名>` 可以查看实际运行的命令是否为内建命令
#### 2.3 脱离终端
如果你使用过 SSH 连接到远程服务器执行任务，那么你会发现：在 ==shell 中执行的程序在 SSH 断开之后会被关闭==。
这是因为==终端一旦被关闭，就会向其中每个进程发送 SIGHUP==（Signal hangup），而 SIGHUP 的==默认动作即退出程序==运行。

*nohup*
nohup，字面含义，就是「不要被 SIGHUP 影响」。

```bash
$ nohup ping 101.ustclug.org 
& [1] 19258 
nohup: ignoring input and appending output to '/home/ustc/nohup.out'
```

在需要屏蔽 SIGHUP 的程序前添加 nohup，则运行时的输出将被==重定向==到 nohup.out，也可以通过重定向手段自定义输出的文件。
#### 2.4 多终端解决方案—tmux
##### 2.4.1 问题产生
一个终端（硬件概念）只有一套鼠标键盘，只能有一个 shell 主持一个 session，那如果我在 SSH 连接的时候有几个程序需要同时交互的话，只有一个前台进程很不方便。而且上面说过如果 SSH 网络断开，视为终端被关闭，也就意味着前后台一起收到 SIGHUP 一起退出，好不容易设置好的临时变量什么的还得重设。

开启多个 SSH 连接似乎可以解决这个问题。但是如果程序既需要交互，又想保证不因意外断线而停止程序，就是 nohup 也帮不了。

这时 tmux 的出现，解决了 *会话保持与窗口复用* 的问题。正如上图所示，tmux 是一个分屏的、运行在命令行的模拟终端，意味着只要有命令行可用，就可以将多个交互进程集成在在一个窗口上。该窗口不因断开连接或者暂时登出而消失，而是会保存在后台，下一次登录时可以立即还原。
##### 2.4.2 tmux入门
tmux 由会话（session），窗口（window），面板（pane）组织起每个 shell 的输入框。会话用于区分不同的工作；窗口是会话中以显示屏为单位的不同的页；而面板则是一个窗口上被白线分割的不同区域。熟练掌握会话，窗口，面板之间的切换，可以极大提高使用效率。

在命令行中敲击：`tmux` => 打开了第一个 tmux 窗口

==原装语法&自定义配置看个人了，建议自己配一遍==

更多功能，可以到 [cheatsheet](https://cheatography.com/bechtold/cheat-sheets/tmux-the-terminal-multiplexer/pdf/) 中查询。

关于 tmux 的更多介绍，可以参见[这篇博客](http://louiszhai.github.io/2017/09/30/tmux/)。
### 3. 服务
#### 3.1 守护进程
服务的特征，意味着服务进程必须独立于用户的登录，不能随用户的退出而被终止。根据前面的讲解，只有启动时脱离会话才能避免因为终端的关闭而消失。而这类一直默默工作于后台的进程被称为**守护进程** (daemon)。
#### 3.2 服务管理
略
#### 3.3 自定义服务
如果我想将一个基于 Web 的应用（如基于 Web 的 Python 交互应用）作为局域网内 Web 服务，以便于在其他设备上访问。那么如何将其注册为 systemd 服务呢？

其实只需要编写一个简单的 .service 文件即可。（可以参考系统中其他 service 文件，以及 [systemd.service(5)](https://www.freedesktop.org/software/systemd/man/systemd.service.html) 手册页编写配置文件）

```bash
Jupyter Notebook 是基于浏览器的交互式编程平台，在数据科学领域非常常用。

首先使用文本编辑器在 `/etc/systemd/system` 目录下创建一个名为 `jupyter.service` 的文件。并填入以下内容：

[Unit] 
Description=Jupyter Notebook    # 该服务的简要描述  
[Service] 
PIDFile=/run/jupyter.pid        # 用来存放 PID 的文件 
ExecStart=/usr/local/bin/jupyter-notebook --allow-root                       # 使用绝对路径标明的命令及命令行参数
WorkingDirectory=/root          # 服务启动时的工作目录 
Restart=always                  # 重启模式，这里是无论因何退出都重启
RestartSec=10                   # 退出后多少秒重启  
[Install] 
WantedBy=multi-user.target      # 依赖目标，这里指进入多用户模式后再启动该服务

将写好的配置文件保存为 `/etc/systemd/system/jupyter.service`，然后运行 `systemctl daemon-reload`，就可以使用 `systemctl` 命令来管理这个服务了，例如：

`$ systemctl start jupyter $ systemctl stop jupyter $ systemctl enable jupyter  # enable 表示标记服务的开机自动启动 $ systemctl disable jupyter # 取消自启`
```
#### 3.4 例行性任务
##### 3.4.1 at 命令
at 命令负责单次计划任务，当前许多发行版中，并没有预装该命令，需要 `sudo apt install at` 进行安装。

随后使用 tldr 查看该命令使用方法：
```bash
at

Execute commands once at a later time.
Service atd (or atrun) should be running for the actual executions.
More information: <https://manned.org/at>.

- Execute commands from `stdin` in 5 minutes (press `Ctrl + D` when done):
    at now + 5 minutes（过t时间再操作）
- Execute a command from `stdin` at 10:00 AM today:
    echo "./make_db_backup.sh" | at 1000（在几点进行操作）
- Execute commands from a given file next Tuesday:
    at -f path/to/file 9:30 PM Tue
```

设置完任务之后，我们需要管理任务，我们可以用 `at -l` 列出任务，`at -r <编号>` 删除任务。它们分别是 atq 和 atrm 命令的别名
##### 3.4.2 crontab 命令
cron 命令负责周期性的任务设置，与 at 略有不同的是，cron 的配置大多通过配置文件实现。
大多数系统应该已经预装了 crontab。

个人感觉用的比较少，暂略
### 4. 进程组织结构
#### 4.1 进程父子关系
Fork 是类 UNIX 中创建进程的基本方法：将当前的进程完整复制一份。新进程和旧进程唯一的区别是 `fork( )` 的返回值不同。程序员可以根据其返回值为新旧进程设置不同的逻辑。

除了最开始的 0 号进程外，绝大多数情况下其他进程是由另一个进程通过 fork 产生的。这里产生进程的一方为**父进程**，被产生的是**子进程**。在 Linux 中，父进程可以等待子进程，接收子进程退出信号以及返回值。

**孤儿进程 and 僵尸进程**
父子关系引出了两种特殊的运行情况——==父进程先退出==，它的子进程成为**孤儿进程** (orphan)；
==子进程先退出，而父进程未作出回应== (wait)，子进程则变为**僵尸进程** (zombie)。

孤儿进程（即留下的子进程）==由操作系统回收，交给 init「领养」==。
僵尸进程的进程==资源大部分已释放，但占用一个 PID，并保存返回值==。系统中大量僵尸进程的存在将导致无法创建进程。
#### 4.2 进程组
**进程组**大体上是执行同一工作的进程形成的一个团体，通常是由于父进程 fork 出子进程后子进程继承父进程的组 ID 而逐渐形成。
设计进程组机制主要是为了面向协作任务，比如 Firefox 工作是网页浏览，那么其相关的进程一定属于一个进程组。
进程组的出现方便了系统信号管理，后面可以看到，==发给一个进程组的信号将被所有属于该组的进程接收==，意义就是停止整个任务整体。
#### 4.3 会话
**会话** (session) 可以说是面向用户的登录出现的概念。当用户从终端登录进入 shell，就会以该 shell 为会话首进程展开本次会话。一个会话中通常包含着多个进程组，分别完成不同的工作。用户退出时，这个会话会结束，但有些进程仍然以该会话标识符 (session ID) 驻留系统中继续运行。

说到会话，就必然涉及到 Linux 会话中的前后台管理机制。**前台** (foreground) 与**后台**(background)，本质上决定了是否需要与用户交互，对于单独的一个 shell，只能有一个前台进程（组），其余进程只能在后台默默运行，上述中若干进程组，正是前台进程组和后台进程组的概称。在稍后部分中我们将学习前后台切换的相关操作。

| 进程属性  | 意义/目的                                                          |
| ----- | -------------------------------------------------------------- |
| PID   | Process ID，标识==进程的唯一性==。                                       |
| PPID  | Parent PID，标识==进程父子关系==。                                       |
| PGID  | Process Group ID，标识共同完成一个任务的整体。                                |
| TPGID | 标识一组会话中处于前台（与用户交流）的进程（组）。                                      |
| SID   | Session ID，标识一组会话，传统意义上标识一次登录所做的任务的集合，如果是与具体登录无关的进程，其 SID 被重置。 |
### 5. Linux下进程查看原理
正文提到，==ps 做为查看进程的基本命令==，仅仅提供**静态输出**，并不能提供实时监控。但是它足够简单，可以供我们进行分析。直接阅读 ps 的源代码是最直接的方法，但是成本可能过高，更好的方法是用 strace 命令来分析 ps 运行过程中使用到的系统调用（注：strace是在Linux下的，MacOS对等命令是dtruss）
#### 5.1 strace 命令
strace 可以 (追踪) 程序使用的 (系统调用) ，输出在屏幕上，是一个程序调试工具。此处用来追踪 ps 打开的文件。

strace 开头字母为 s 是由于该命令为 Sun™ 系统移植而来的调用追踪程序。

注意 strace 会输出到标准错误 (stderr)，需要将输出重定向到标准输出之后通过管道后才能使用 grep 等工具。

**针对MacOS的dtruss常见错误：**
>你的 macOS 系统启用了系统完整性保护（System Integrity Protection，SIP）。SIP 限制了对某些特权命令的使用，似乎影响了在这种情况下执行 `dtrace` 或 `dtruss`。
为了解决这个问题，你可以尝试以下方法：
**1. 暂时禁用 SIP（不建议常规使用）:**
   需要注意的是，禁用 SIP 通常不被推荐，因为它增强了系统的安全性。如果你选择继续，你可以通过进入恢复模式并在那里使用终端来禁用 SIP。
   (1) 重新启动你的 Mac 并按住 `Command + R` 直到出现苹果标志。
   (2) 进入恢复模式后，从“实用工具”菜单中打开终端。
   (3) 运行以下命令以禁用 SIP：`csrutil disable`
   (4) 重新启动你的 Mac。
   (5) 完成任务后，考虑在恢复模式中使用 `csrutil enable` 重新启用 SIP。
   **2. 使用 `sudo` 运行 `dtrace` 或 `dtruss`：**
   尝试运行 `sudo dtrace` 或 `sudo dtruss`。这可能会提供执行命令所需的权限，尽管 SIP 处于启用状态。请注意禁用 SIP 或在 `sudo` 中使用特权命令可能带来的安全风险和潜在风险。如果可能的话，考虑使用不需要禁用 SIP 的替代方法或工具来满足你的调试需求。
#### 5.2 现代系统调用
现代==操作系统有效地隔离了进程==，不允许进程越过操作系统与外界交互。

操作系统提供的==和外界交互的方式就是系统调用==。**操作系统提供的系统调用可以让进程对文件、内存、进程等的状态进行控制**，从而在安全**隔离进程的前提下允许程序实现丰富多彩的功能**。

系统调用运行在操作系统核心，为内核与用户层提供了一种通信的方式，是各用户进程「使用」操作系统的统一接口。如果没有系统调用，用户程序就需要直接操作硬件来进行需要的操作，而这对于现代操作系统来说显然是无法接受的。
#### 5.3 ps如何获取进程信息
ps 通过打开 `/proc/1` 文件夹下的 `stat` 和 `status` 文件，获得 1 号进程的信息。我们也可以试着打开它：
注：此处笔者使用的是MacOS下的ubuntu虚拟机
```bash
huluobo@ubuntu:/Users/huluobo$ cat /proc/1/stat
1 (systemd) S 0 1 1 0 -1 4194560 4963 233071 57 314 8 8 360 125 20 0 1 0 46 170094592 2272 18446744073709551615 1 1 0 0 0 0 671173123 4096 1260 0 0 0 17 8 0 0 0 0 0 0 0 0 0 0 0 0 0
```

```bash
huluobo@ubuntu:/Users/huluobo$ cat /proc/1/status
Name:	systemd
Umask:	0000
State:	S (sleeping)
Tgid:	1
Ngid:	0
Pid:	1
PPid:	0
TracerPid:	0
Uid:	0	0	0	0
Gid:	0	0	0	0
FDSize:	128
Groups:
NStgid:	1
NSpid:	1
NSpgid:	1
NSsid:	1
Kthread:	0
VmPeak:	  231436 kB
VmSize:	  166108 kB
VmLck:	       0 kB
VmPin:	       0 kB
VmHWM:	    9088 kB
VmRSS:	    9088 kB
RssAnon:	    1920 kB
RssFile:	    7168 kB
RssShmem:	       0 kB
VmData:	   18568 kB
VmStk:	     132 kB
VmExe:	    1416 kB
VmLib:	   12532 kB
VmPTE:	      96 kB
VmSwap:	       0 kB
CoreDumping:	0
THP_enabled:	1
untag_mask:	0xffffffffffffff
Threads:	1
SigQ:	1/112048
SigPnd:	0000000000000000
ShdPnd:	0000000000000000
SigBlk:	7be3c0fe28014a03
SigIgn:	0000000000001000
SigCgt:	00000001000004ec
CapInh:	0000000000000000
CapPrm:	000001ffffffffff
CapEff:	000001ffffffffff
CapBnd:	000001ffffffffff
CapAmb:	0000000000000000
NoNewPrivs:	0
Seccomp:	2
Seccomp_filters:	1
Speculation_Store_Bypass:	thread vulnerable
SpeculationIndirectBranch:	unknown
Cpus_allowed:	fff
Cpus_allowed_list:	0-11
Mems_allowed:	1
Mems_allowed_list:	0
voluntary_ctxt_switches:	694
nonvoluntary_ctxt_switches:	386
```
我们 👀 proc里面究竟装了啥：
```bash
huluobo@ubuntu:/Users/huluobo$ cd /proc
huluobo@ubuntu:/proc$ ls
1    175  765        device-tree  ioports      loadavg       self           tty
116  179  buddyinfo  devices      irq          locks         softirqs       uptime
142  191  bus        diskstats    kallsyms     meminfo       stat           version
160  208  cgroups    driver       key-users    misc          swaps          vmallocinfo
161  213  cmdline    execdomains  keys         modules       sys            vmstat
164  214  config.gz  filesystems  kmsg         mounts        sysrq-trigger  zoneinfo
165  215  consoles   fs           kpagecgroup  net           sysvipc
167  216  cpuinfo    interrupts   kpagecount   pagetypeinfo  thread-self
169  23   crypto     iomem        kpageflags   partitions    timer_list
``` 
结论：
根目录下 `/proc` 文件夹储存进程信息，而 htop 等命令通过对该文件夹下的文件进行自动读取来监视进程。实际上，`/proc` 是一个虚拟的文件系统，存在于内存中，反映着系统的运行状态。
### 6. 守护进程的产生
原理略

打开 htop，按 PID 顺序排列，排在前面的用户进程历来都是守护进程，它们大多数先于用户登录而启动。可以注意到，守护进程的 SID 与自身 PID 相同。
### 7. 关于fork( )
以一个例子形象说明：
```bash
#include <stdio.h>
#include <unistd.h>  // Unix standard header，提供 POSIX 标准 API

int main() {
    for (int i = 0; i < 3; i++)
    {
        int pid = fork();   // fork 系统调用，全面复制父进程所有信息。
        if (pid == 0) {
            // 子进程返回 pid=0。
            printf("I'm child, forked in %d turn\n", i);
        } else if (pid < 0)  {
            // fork 失败，pid 为负值。
            printf("%d turn error\n", i);
        } else  {
            // 父进程返回子进程 pid。
            printf("I'm father of %d turn, child PID = %d\n", i, pid);
        }
        sleep(3);
    }
    sleep(1000);
    return 0;
}
```
开两个终端，terminalA & terminalB

A运行命令：
```bash
gcc forking.c -o forking && ./forking
```

B使用htop查看：
![[Pasted image 20240226150845.png|450]]
按下 ==T 键，界面显示的进程将转化为树状结构==，直观描述了父子进程之间的关系。
此处可以明显观察到树梢*子进程的 PID 等于父进程的 PPID*。（顺便我们也可以在这里看见虚拟机的运行原理——orbstack-helper）

同时由 shell 进程创立的 forking 进程的进程组号 (PGRP) 为自己的 PID，剩余进程的 PGRP 则继承自最开始的 forking 进程，PGRP 可以通过系统调用修改为自身，从原进程组中独立出去另起门户。
### 8. terminal和console
在上世纪六十年代，个人计算机尚未开始发展，用户使用计算机的一种常见方式就是通过终端，与远程的服务器连接交互。当时键盘和显示器连为一体，称为终端（terminal）。而主机自带的一套键盘与屏幕只能给系统管理员使用，称为控制台 (console)，用来输出启动 debug 信息（现在的 Linux 系统如果因故障而不得不进入单用户修复模式，则只有一个终端 `/dev/console` 开启）。

然而随着时代的发展，这种模式逐渐被家庭电脑的分布式主机取代，我们不需要，也没有多套终端了，只有显示器、键盘、鼠标。但是为了向前兼容性，我们需要假装这是一个（甚至多个）终端，所以一般发行版 `/dev` 目录下有 7 个终端 `tty1 ~ tty7`，通过 `Ctrl + Alt + F1 ~ F7` 切换键盘与显示器与哪个终端相对应。

再后来，随着时代发展，终端需要出现在图形界面上了，然而承载图形界面的也是终端，所以终端里的终端就需要终端模拟器来实现了。由此，出现在图形界面上的终端才叫终端模拟器。

没有图形界面时，shell 一般为控制台 (tty) 的子进程，在图形界面上 shell 建立在虚拟终端 (pty, pseudo tty) 之上。顺带一提，服务器常用的远程连接工具 `ssh` 的父进程也是一个 pty。

**注意：终端不是 Shell，尽管它们经常被弄混淆。**

**Ref：[你真的知道什么是终端吗](https://www.linuxdashen.com/你真的知道什么是终端吗？)**
# Chapter 9 - 用户组、文件权限与层次结构
## 1. 用户与用户组
### 1.1 为何需要用户
早期的操作系统没有用户的概念（如 MS-DOS），或者有「用户」的概念，但是几乎不区分用户的权限（如 Windows 9x）。而现在，这不管对于服务器，还是个人用户来说，都是无法接受的。

在服务器环境中，「用户」的概念是明确的：服务器的管理员可以为不同的使用者创建用户，分配不同的权限，保障系统的正常运行；也可以为网络服务创建用户（此时，用户就不再是一个有血有肉的人），通过权限限制，以减小服务被攻击时对系统安全的破坏。

而对于个人用户来说，他们的设备不会有第二个人在使用。此时，现代操作系统一般区分使用者的用户与「系统用户」，并且划分权限，以尽可能保证系统的完整性不会因为用户的误操作或恶意程序而遭到破坏。
### 1.2 几个重要的tips
我们知道，`root` 用户可以对系统做极其危险的操作。当使用 `root` 权限执行命令时（如使用 `sudo`），一定要**小心、谨慎，理解命令的含义之后再按下回车**。
以下是一些会对系统带来毁灭性破坏的例子。 **再重复一遍，不要执行下面的命令！**
- `rm -rf /`（删除系统中的所有可以删除的文件，**包括被挂载的其他分区**。**即使不以 `root` 权限执行，也可以删掉自己的所有文件。**）
- `mkfs.ext4 /dev/sda`（将系统的第一块硬盘直接格式化为 ext4 文件系统。这会破坏其上所有的文件。）
- `dd if=/dev/urandom of=/dev/sda`（对系统的第一块硬盘直接写入伪随机数。这会破坏其上所有的文件，并且找回文件的可能性降低。）
- `:(){ :|: & };:`（被称为「Fork 炸弹」，会消耗系统所有的资源。在未对进程资源作限制的情况下，只能通过重启系统解决，所有未保存的数据会丢失。）
## 2. 文件权限
### 2.1 如何查看、如何看懂当前目录下的文件权限
在 Linux 中，每个文件和目录都有自己的权限。可以使用 `ls -l` 查看当前目录中文件的详细信息：

```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ ls -l
total 104
drwxr-xr-x 17 huluobo huluobo   544 Oct 13 15:38 Unet-Segmentation-Pytorch-Nest-of-Unets
-rwxr-xr-x  1 huluobo huluobo 70400 Feb 26 15:04 forking
-rw-r--r--  1 huluobo huluobo   673 Feb 26 15:02 forking.c
-rw-r--r--  1 huluobo huluobo   154 Oct  8 16:21 hello.cpp
-rw-r--r--  1 huluobo huluobo  3401 Oct  8 16:47 hello.o
-rw-r--r--  1 huluobo huluobo  2939 Oct  8 16:40 hello.s
-rwxr-xr-x  1 huluobo huluobo 13777 Oct  8 16:29 prog
```

**第一列的字符串从左到右意义分别是：**
- 文件类型（一位）
- 文件==所属用户==的权限（三位）
- 文件==所属用户组==的权限（三位）
- ==其他人==的权限（三位）。
对于每个权限，第一位 `r` 代表读取 (**R**ead)，第二位 `w` 代表写入 (**W**rite)，第三位 `x` 代表执行 (E**x**ecute)，`-` 代表没有对应的权限。

**第三、四列为文件所属用户和用户组。**

```bash
- rw- r-- r--  1 huluobo huluobo   673 Feb 26 15:02 forking.c
```
例如，上面的文件 `forking.c` 为普通文件 (`-`)，所属用户权限为 `rw-`，所属用户组权限为 `r--`，其他人的权限为 `r--`，文件所属用户和用户组均为 `huluobo`。
### 2.2 执行权限意味着什么
读取和写入权限是很容易理解的。但是执行权限是什么意思？

对于一个文件来说，拥有执行权限，它就**可以被操作系统作为程序代码执行**。如果某个程序文件*没有执行权限，你仍然可以查看这个程序文件本身，修改它的内容，但是无法执行它*。

而==对于目录来说，拥有执行权限，你就可以访问这个目录下的所有文件的内容==。以下是一个例子：
```bash
$ ls -l
total 8
-rwxrw-r-- 1 ustc ustc   40 Feb  3 22:37 a_file
drw-rw-r-- 2 ustc ustc 4096 Feb  3 22:38 a_folder
$ （与上面不同，我们去掉了 a_folder 的执行权限）
$ cd a_folder
-bash: cd: a_folder/: Permission denied
$ （失败了，这说明，如果没有执行权限，我们无法进入这个目录）
$ ls a_folder
ls: cannot access 'a_folder/test': Permission denied
test
$ （列出了这个目录中的文件列表，但是因为没有执行权限，我们没有办法访问到里面的文件 test）
$ cat a_folder/test
cat: a_folder/test: Permission denied
$ cp a_folder/test test
cp: cannot stat 'a_folder/test': Permission denied
$ mv a_folder/test a_folder/test2
mv: failed to access 'a_folder/test2': Permission denied
$ touch a_folder/test2
touch: cannot touch 'a_folder/test2': Permission denied
$ rm a_folder/test
rm: cannot remove 'a_folder/test': Permission denied
$ （可以看到，即使我们有写入权限，在此目录中进行添加、删除、重命名的操作仍然是不行的）
```

为了更好地理解目录权限的含义:
>可以把目录视为一个「文件」来看待，这个文件包含了目录中下一层的文件列表——「读取」对应读取文件列表的权限，「写入」对应修改文件列表（添加、删除、重命名文件）的权限，「执行」对应实际去访问列表中文件、以及使用 `cd` 切换当前目录到此目录的权限。
### 2.3 如何修改一个程序的权限
 - `chmod` (**ch**ange file **mod**e bits) 修改权限
 - `chown` (**ch**ange file **own**er) 修改文件所有者
 
有时候，我们会从网上下载一些二进制的程序，或者根据网络的教程编写脚本程序，但当你想执行的时候却发现：
```bash
$ ./program
-bash: ./program: Permission denied
```
大多数情况下，这说明这个文件缺少执行 (`x`) 权限。可以使用 `chmod +x` 命令添加执行权限。
```bash
$ chmod +x program
$ ./program
（添加完权限后，就可以执行了！）
```
## 3. 文件系统层次结构
相信到现在你应该已经发现了：Linux 下文件系统的结构和 Windows 的很不一样。在 Windows 中，分区以盘符的形式来标识（如「C 盘」、「D 盘」），各个分区的分界线是很明确的。

在系统所在的分区（一般为 C 盘）中，存储着程序文件 (`Program Files`)，系统运行需要的文件 (`Windows`)，用户文件 (`Users`) 等。这种组织形式源于 DOS 和早期的 Windows，并一直传承下来。

![[Pasted image 20240226154240.png|400]]
其他的分区以挂载 (mount) 的形式「挂」在了这棵树上，如图中的 `/mnt/windows_disk/`。

```bash
huluobo@ubuntu:/$ la
Applications  Users    bin   dev  home  lib64  mnt  private  root  sbin  sys  usr
Library       Volumes  boot  etc  lib   media  opt  proc     run   srv   tmp  var
```

那么在根目录下的这些目录各自有什么含义呢？这就由文件系统层次结构标准 (FHS, Filesystem Hierarchy Standard) 来定义了。这个标准定义了 Linux 发行版的标准目录结构。大部分的 Linux 发行版遵循此标准，或由此标准做了细小的调整。以下进行一个简要的介绍。也可以在[官网](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)查看标准的具体内容。

当然，实际情况不一定会和以下介绍的内容完全一致。可以使用 `man hier` 和 `man file-hierarchy` 查看你的系统中关于文件系统层次结构的文档。

- `/bin`
存储必须的程序文件，对所有用户都可用。

- `/boot`
存储在启动系统时需要的文件。

- `/dev`
存储设备文件。

*PS：什么是设备文件*
>在 Linux 的哲学中，存在着「一切皆文件」这样的思想。设备文件就是计算机设备抽象成文件的形式，程序和用户可以以读写普通文件的方式向这些文件输入内容，或者从文件中获取内容。系统驱动程序会相应处理用户对对应设备文件的输入和输出。
有一些常用的设备文件如：
(1) `/dev/null`：总是返回空（EOF）数据。
(2) `/dev/zero`：总是返回零数据。
(3) `/dev/urandom`：输出随机数据。
这些设备文件可以帮助我们做到丢弃程序输出等操作。

- `/etc`
存储系统和程序的配置文件。

*PS：win常用注册表*
>Windows 系统与其上很多程序都使用注册表，而非文件的形式存储配置信息。注册表是一个数据库，拥有数据库的优点（如原子性），集中地管理配置项。而 Linux 下的程序更喜欢将配置以文件的形式存储，保持配置简单，便于用户编辑与备份。

- `/home`
用户的家目录。存储用户自己的信息。

- `/lib`
存放系统运行必须的**程序库文件**。

- `/media` 和 `/mnt`
这两个目录都用于**挂载**其他的文件系统。`/media` 用于可移除的文件系统（如光盘），而 `/mnt` 用于临时使用。

- `/opt`
存放**额外的程序包**。一般将一些大型的、商业的应用程序放置在这个目录。

- `/root`
`root` 用户的家目录。

- `/run`
**系统运行时的数据**。在每次启动时，里面的数据都会被删除。

- `/sbin`
存储用于**系统管理**，以及仅允许 `root` 用户使用的程序。如 `fsck`（文件系统修复程序）、`reboot`（重启系统）等。

- `/srv`
存储**网络服务**的数据。

- `/tmp`
**临时目录**，所有用户都可使用。

- `/usr`
大多数软件都会安装在此处。其下有一些目录与 `/` 下的结构相似，如：
>(1)  `/usr/bin`
>(2) `/usr/lib`
>(3) `/usr/sbin`
>
>PS: 此外，还有一些目录：
(ps1) `/usr/include`: 存储**系统通用的 C 头文件**。当然，里面会有你非常熟悉的头文件，如 `stdio.h`。
(ps2) `/usr/local`: 存储系统**管理员自己安装的程序**，这些文件不受系统的软件管理机制（如 `apt`）控制。`/usr/local` 里面的层次结构和 `/usr` 相似。
(ps3) `/usr/share`: 存储**程序的数据文件（如 `man` 文档、GUI 程序使用的图片等）**。

- `/var`
存储会发生变化的程序相关文件。例如下面的目录：
>(1) `/var/log`：存储程序的日志文件。
>(2) `/var/lib`：存储程序自身的状态信息（如 lock file）。
>(3) `/var/run`：存储程序运行时的数据（部分发行版会将该目录符号链接到 `/run` 目录）。
>(4) `/var/spool`：存储「等待进一步处理」的程序数据。
# Chapter 10 - 网络、文本处理工具与 Shell 脚本
## 1. I/O重定向与管道
### 1.1 重定向
一般情况下命令从标准输入（stdin）读取输入，并输出到标准输出（stdout），默认情况下两者都是你的终端。

使用重定向可以让命令从文件读取输入/输出到文件。下面是以 `echo` 为例的重定向输出：
```bash
$ echo "Hello Linux!" > output_file # 将输出写入到文件（覆盖原有内容）
$ cat output_file
Hello Linux!
$ echo "rewrite it" > output_file
$ cat output_file # 可以看到原来的 Hello Linux! 被覆盖了。
rewrite it
$ echo "append it" >> output_file # 将输出追加到文件（不会覆盖原有内容）
$ cat output_file
rewrite it
append it
```
1. rewrite：[command] "text" > file （将输出写入到文件，覆盖原有内容）
2. append：[command] "text" >> file（将输出追加到文件，不会覆盖原有内容）
- **无论是 `>` 还是 `>>`，当输出文件不存在时都会自动创建该文件！**
- 重定向输入使用符号 `<`
### 1.2 管道
管道（pipe），操作符 `|`，作用为将符号左边的命令的 stdout 接到之后的命令的 stdin。
PS：管道不会处理 stderr
![[Pasted image 20240226171942.png|400]]
管道是类 UNIX 操作系统中非常强大的工具。通过管道，我们可以将实现各类小功能的程序拼接起来干大事。
```bash
$ ls / | grep bin  # 筛选 ls / 输出中所有包含 bin 字符串的行
bin                      # 输出/文件夹下的所有包含bin字符串的行
sbin
```

```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ ls . | grep "c"  # 筛选 ls . 输出中所有包含 c 字符串的行
Unet-Segmentation-Pytorch-Nest-of-Unets                                              # 输出当前文件下下所有包含 c 字符串的行
forking.c
hello.cpp
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$
```

```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ history | grep "hbx" # 输出所有包含 “hbx” 字符串的搜索历史记录
  155  vi hbx.cpp
  186  vim hbx1.cpp
  189  gcc -g hbx1.cpp -o hello
  190  gcc -g hbx1.cpp -o
  191  gcc -g hbx1.cpp hbx1
  192  gcc -g hbx1.cpp 666
  194  gcc -g hbx1.cpp
  197  gdb hbx1
  199  gdb hbx1.cpp
  200  vim hbx1.cpp
  201  gcc hbx1.cpp
  202  g++ hbx1.cpp
  208  gdb hbx1.cpp
  240  rm hbx1.cpp
 1131  ls / | grep "hbx"
 1139  history | grep "hbx"
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$
```
## 2. 网络下载
### 2.1 Wget
`wget` 是强力方便的下载工具，可以通过 HTTP 和 FTP 协议从因特网中检索并获取文件。

使用 `man wget` 得到的结果为 `wget [option]... [URL]...`，其中的更多参数可以通过查看帮助 `wget -h` 来获取。

**格式：**
```bash
wget [option]... [URL]...
```

**常用的选项：**

| 选项                           | 含义                     |
| ---------------------------- | ---------------------- |
| `-i`, `--input-file=文件`      | 下载本地或外部文件中的 URL        |
| `-O`, `--output-document=文件` | 将输出写入文件                |
| `-b`, `--background`         | 在后台运行 wget             |
| `-d`, `--debug`              | 调试模式，打印出 wget 运行时的调试信息 |
**示例：**
```bash
# 批量下载 filelist.txt 中给出的链接：
$ wget -i filelist.txt
```
### 2.2 cURL
cURL (`curl`) 是一个利用 URL 语法在命令行下工作的文件传输工具，其中 c 意为 client。

虽然 cURL 和 Wget 基础功能有诸多重叠，如下载。但 cURL 由于可自定义各种请求参数，所以在模拟 web 请求方面更擅长；wget 由于支持 FTP 协议和递归遍历，所以在下载文件方面更擅长。

**格式：**
```bash
wget [option]... [URL]...
```

**常用的选项：**

| 选项   | 含义                                    |
| ---- | ------------------------------------- |
| `-o` | 把远程下载的数据**保存到文件**中，需要*指定文件名*          |
| `-O` | 把远程下载的数据**保存到文件**中，*直接使用 URL 中默认的*文件名 |
| `-I` | 只展示响应头内容                              |
**示例：**
```bash
# 1. 输出github主页的代码
curl  "https://github.com"   

# 2. 使用重定向把Github页面保存至 本地`github.html` 文件内
curl "https://github.com" > github.html   

# 3. 同 2.
curl -o github.html "https://github.com"

# 4. 下载 USTCLUG 的 logo
curl -O "https://ftp.lug.ustc.edu.cn/misc/logo-whiteback-circle.png"

# 5. 只展示 HTTP 响应头内容
curl -I "https://github.com" 

# 6. 只展示 HTTP 响应头内容 并 导入html文件
curl -I "https://github.com" > github_head.html
```

```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ ls
'#curl-usage'                              bing.html   forking.c     github_head.html   hello.o   logo-whiteback-circle.png   prog
 Unet-Segmentation-Pytorch-Nest-of-Unets   forking     github.html   hello.cpp          hello.s   output_file
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$
```
## 3. 文本处理
### 3.1 文本统计：wc
`wc` 是文本统计的常用工具，它可以输出文本的行数、单词数与字符（字节）数
```bash
wc file
```
### 3.2 文本比较：diff
diff 工具用于比较两个文件的不同，并列出差异。
- 加参数 `-w` 可忽略所有空白字符， `-b` 可忽略空白字符的数量变化。
- 假如比较的是两个**文本文件**，差异之处*会被列出*；假如比较的是**二进制文件**，只会*指出是否有差异*。
```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ cat file1
hbx I love you
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ cat file2
cqy I love you
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$
```

```bash
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ diff file1 file1
huluobo@ubuntu:/Users/huluobo/code_projects/ubuntu$ diff file1 file2
1c1
< hbx I love you
---
> cqy I love you
```
### 3.3 文本始末：head&tail
顾名思义，head 和 tail 分别用来显示开头和结尾指定数量的文字。

以 head 为例，这里给出共同的用法：
- 不加参数的时候默认显示前 10 行
- `-n <NUM>` 指定行数，可简化为 `-<NUM>`
- `-c <NUM>` 指定字节数

```bash
$ head file  # 显示 file 前 10 行
$ head -n 25 file  # 显示 file 前 25 行
$ head -25 file  # 显示 file 前 25 行
$ head -c 20 file  # 显示 file 前 20 个字符
$ tail -10 file  # 显示 file 最后 10 行
```

我的常用：
```
head -100 file1 # 显示file1文件的前100行
tail -20 file2      # 显示file2文件的末20行
```
### 3.4 文件查找：grep
`grep` 命令可以查找文本中的字符串：

```bash
$ grep 'hello' file    # 查找（文件 file） 中（包含 hello 的行）
$ ls | grep 'file'       # 查找（当前目录下）（文件名包含 file 的文件）
$ grep -i 'Systemd' file  # 查找文件 file 中包含 Systemd 的行（忽略大小写）
$ grep -R 'hello' .           # 递归查找当前目录下内容包含 hello 的文件
```
### 3.5 文本替换：sed
`sed` 命令可以替换文本中的字符串：

```bash
$ sed 's/hello/world/g' file        # 将文件 file 中的 hello 全局（global）替换为 world 后输出
$ sed 's/hello/world/' file          # 将文件 file 的每一行第一个出现的 hello 替换为 world 后输出
$ echo 'helloworld' | sed 's/hello/world/g'  # 管道也是可以的
$ sed -i 's/hello/world/g' file    # -i 参数会直接写入文件，操作前记得备份哦！
$ sed -i.bak 's/hello/world/g' file  # 当然，也可以让 sed 帮你备份到 file.bak
```

我的常用：
```bash
# 替换软件源
$ sudo sed -i 's/cn.archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
$ sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
$ sudo sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```
## 4. shell脚本
### 4.1 Shell重点内容复习
详见笔者的另一份教程《Shell脚本编程》

这里仅罗列重点命令！
==hhh==

### 4.2 review：Fork炸弹原理解析
在Chapter5中，我们介绍过 fork 炸弹。它通过创建大量进程消耗大量的系统资源，从而拖慢系统与正常进程的运行速度，增大响应时间，使得操作系统的正常运作受到较大影响。

现在解析原理：
```bash
# Fork Boom
:(){ :|: & };:
```

这是一个函数定义以及对其的调用语句，可以格式化为：
```bash
:()
{
    :|: &
};
:
```

在 Bash 中，`:`、`.`、`/` 等一些字符也能够被用于函数命名，因此，上面的代码等价于：
```bash
func()
{
    func | func &
};
func
```

fork 炸弹的核心是函数内容：`func | func &`

- 第一个 func 代表递归执行这个函数。
- | 代表要将第一个函数的数据结果通过管道传输给后一个函数。
- & 代表要在后台执行这一条命令，如果其中一个函数被操作系统回收，其调用产生的子函数并不会被回收。

于是运行一次这个函数就会创建两个 func 函数的实例，并不断地反复调用。实例的数量会指数爆炸式地增长，最终耗尽系统的资源。
## Chapter 11 - Linux上的编程
### 1. C语言开发
#### 1.1 单语言开发
在 Windows 或 Mac OS X 这样带 GUI 的系统上，通过安装 IDE，我们可以使用 IDE 中的编译功能来编译出目标。 实际上，这些带有图形界面的 IDE 的编译往往是封装了各种提供命令行接口的编译器。 自然，在众多无 GUI 的 Linux 上，我们同样可以调用这些提供命令行接口的编译器进行编译。

**各平台常用的编译器**
1. Linux 上常用的编译器是 gcc 和 clang。 其中 gcc 是由 GNU 组织维护的，而 clang 是由 LLVM 组织维护的。
2. Mac OS X 本身由 BSD 发展而来，也以 gcc 和 clang 为主。 值得一提的是，Mac OS X 上自带的 gcc 其实是 clang 的别名，在 Terminal 输入 `gcc -v` 即可发现。
3. windows不说

这里我们使用 gcc 对这个文件进行编译，生成二进制文件：
```bash
$ gcc main.c -o main
$ ./main
Hello World!
```
这里用 `-o` 指定了输出的==二进制文件的文件名== `main`。

应当注意到 `gcc main.c -o main` 这条指令没有打印出任何内容。 这是因为整个编译过程是成功的，gcc 没有需要报告的内容，因此保持沉默。(这是 Unix 哲学的一部分)
#### 1.2 多文件状态
只在一个文件中编写代码，对于稍微大的开发都是不够的： 对于个人维护的小项目尚可，但当你面临的是一个多人开发、模块复杂、功能繁多的大项目时（无论是在公司工程还是在实验室科研中，这都是普遍的情况）， 拆分代码到多个文件才是一个明智（或者说可行）的做法。
##### 1.2.1 前置基础
假设你拥有一下两个文件：
```bash
// main.c
#include "print.h"

int main() {
  print();
  return 0;
}
```

```bash
// print.c
#include "print.h"

#include <stdio.h>

void print() {
  printf("Hello World!\n");
}
```
那为了在 main.c 中调用 `void print()` 这个函数，你需要做以下几件事：

- 在当前目录下新建一个头文件 print.h；
- 在 print.h 中填入以下内容：
```bash
// print.h
#ifndef PRINT
#define PRINT

void print();

#endif  // PRINT
```

这里的 `#ifndef ... #define ... #endif` 是头文件保护，防止同一头文件被 `#include` 两次造成重复声明的错误， 如果你不理解这部分也没关系，只需保证 `void print();` 这一行声明存在即可。

- 在 main.c 和 print.c 中同时 `#include "print.h"`。

这样，程序就可以被编译运行了！
##### 1.2.2 补充说明宏定义
```bash
#ifndef A
#define B
C
#endif
D
```
如果标识A没有定义，则定义B，执行C，直到endif结束
如果A已经被定义，则直接跳过B/C，直接执行D
##### 1.2.3 具体执行过程
假设文件就是上面的三个
我们将依次**编译链接**，生成目标的**二进制可执行程序**。让我们看一下命令：
```bash
$ gcc main.c -c  # 生成 main.o
$ gcc print.c -c  # 生成 print.o
$ gcc main.o print.o -o main
$ ./main
Hello World!
```

这里我们使用了 `gcc -c`： `-c` 会将源文件编译为*对象文件*（Object file，.o 这一后缀就源自单词 object 的首字母）。 
==对象文件是二进制文件，不过它不可执行==，因为其中需要引用外部代码的地方，是用占位数替代的，无法真正调用函数。

注意到我们没有添加 `-o` 选项，因为 `-c` 存在时 gcc 总会生成相同文件名（这里特指 basename，main.c 中的 main 部分）的 .o 对象文件。

生成了对象文件后，我们来进行*链接*，在相应函数调用的位置填上函数真正的地址，从而生成*二进制可执行文件*。 `gcc` 这一指令会根据输入文件的类型调用相应的程序完成整个编译流程。 在这里，虽然同样是 gcc 指令，但是**由于输入的为 .o 文件，gcc 将调用链接器进行链接，从而生成最终的可执行文件。

##### 1.2.4 gcc编译的四个过程
gcc 的编译其实是四个过程的集合，分别是预处理（preprocessing）、编译（compilation）、汇编（assembly）、链接（linking）， 分别由 cpp、cc1、as、ld 这四个程序完成，gcc 是它们的封装。

这四个过程分别完成：处理 `#` 开头的预编译指令、将源码编译为汇编代码、将汇编代码编译为二进制代码、组合众多二进制代码生成可执行文件， 也可分别调用 `gcc -E`、`gcc -S`、`gcc -c`、`gcc` 来完成。

在这一过程中，文件经历了如下变化：`main.c` 到 `main.i` 到 `main.s` 到 `main.o` 到 `main`。
#### 1.3 构建工具的使用
上述方法在源文件较少时是比较方便的，但当我们面对的是数以千计万计的源文件（同样的，在工作或科研中这也是常见状况），我们将面临以下困难：

- 手动地一一编译实在太麻烦，太浪费精力；
- 这些源文件的编译有顺序要求，为了满足此依赖关系需要设计一个流程；
- 编译整个项目需要难以忍受的大量时间，应当考虑到一部分未更改的源文件不需要重新编译。

为了让机器帮助程序员解决这些困难，构建工具应运而生。 同样的，由于需求巨大，构建工具在 Linux 上亦获得了强力支持：
##### 1.3.1 makefile
Makefile 是中小型项目常用的构建工具。 让我们考虑以下例子：

假设前述 3 份源文件已存在在当前目录下。 创建以下内容的文件，并命名为 `Makefile`：
```bash
# 写法1
main.o: main.c print.h
print.o: print.c print.h
main: main.o print.o
```

然后在当前目录下执行：
```bash
$ make main
$ ./main
Hello World!
```

为了解释这一过程，我们来分析一下 Makefile 的内容。其中：
```bash
main.o: main.c print.h
```

这一行，通过冒号分割，指定了一个名为 `main.o` 的目标，其依赖为 `main.c` 和 `print.h`。 由于整个文件中没有名为 `main.c` 的目标，所以 Makefile 会认为对应的 `main.c` 文件为一个依赖，`print.h` 同理。

在指定了目标和依赖后，紧接着的下一行如果用 **Tab** 缩进，则可以指定利用依赖获得目标的指令。 例如：

```bash
main.o: main.c print.h
    gcc main.c -c  # 一定要用 Tab 缩进而不是 4 个 / 2 个空格——这是历史遗留问题。
```

以上内容表示如果要获得 `main.o` 这个目标，则会执行 `gcc main.c -c` 这个指令。 *如果没有指定命令，Makefile 会尝试从文件后缀等处获取信息，推测你需要的指令。 例如此处即使不显式写出指令，Makefile 也知道用 gcc 来完成编译*！

最终我们在 shell 中执行 `make main`，正是指定了一个最终目标。 如果不提供这个目标，Makefile 则会选择 Makefile 文件中第一目标。 为了获得最终目标，Makefile 会递归地获取依赖、执行指令。

Makefile 的亮点在于引入了文件间的依赖关系。 在使用它进行构建时，Makefile 可以根据文件间的依赖关系和文件更新时间，找出需要重新编译的文件。 在项目较大时这能明显节省构建所需的时间，同时也能解决一些由于编译链接顺序造成的问题。 相较与输入一大串指令，单个的 `make [target]` 甚至是仅仅 `make`，也更加优雅和方便。

根据上述，很显然makefile还有另外一种写法：
```bash
# 写法2
main.o: main.c print.h
	gcc main.c -c # 明确写出依赖/目标对应的指令
print.o: print.c print.h
	gcc print.c -c #  明确写出依赖/目标对应的指令
main: main.o print.o
	gcc main.o print.o -o main #  明确写出依赖/目标对应的指令
```
```bash
┌─(~/code_projects/ubuntu/muiti_files)────────────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:01:25 on main ✖ ✭)──> make main                                                 ──(三, 228)─┘
make: `main' is up to date.
┌─(~/code_projects/ubuntu/muiti_files)────────────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:01:27 on main ✖ ✭)──> ./main                                                    ──(三, 228)─┘
Hello hbx!
```
##### 1.3.2 cmake和ninja
一个更大的工程可能有上万、上十万份源文件，如果一一写进 Makefile，那依然会异常痛苦，且几乎不可能维护。

为了更好的构建程序，大家想出了“套娃”的办法：用一个程序来生成构建所需的配置，CMake 则在这一想法下诞生。

CMake 在默认情况下，可以通过 `cmake` 命令生成 Makefile，再进一步进行 `make`

详见笔者整理的另一份教程[《CMake_Tutorial》](https://github.com/root-hbx/CMake_Tutorial)

另一个值得一提的是 ninja。ninja 和 Makefile、autoconf 较类似，是构建工具，所属抽象层次低于 CMake。 ninja 的特点的是相较与 Makefile 更快，对于多线程编译的支持更好。 
#### 1.4 C++
C++ 的工具链与 C 的是相似的：

实际上，只需将上面内容中的 `gcc` 指令改为 `g++`，你就能同样地完成 C++ 的开发。 gcc 这一编译器本身即支持多种编程语言，包括了 C、C++、Objective C 等。 

其他编译器如 clang 也会提供 `clang++` 这样的指令完成 C++ 的编译。 

Makefile、CMake 这样的构建工具亦可以用于多种编程语言。
### 2. Python语言开发
#### 2.1 python解释器
一般的 Python（CPython）程序的运行，依靠的是 **Python 解释器（Interpreter）**。 在 Python 解释器中，Python 代码首先被处理成一种*字节码*（Bytecode，与 JVM 运行的字节码不是一个东西，但有相似之处）， 然后再交由 *PVM（Python virtual machine）* 进行执行，从而实现跨平台和动态等特性。

由于使用过于广泛，几乎每一份 Linux 都带有 Python 解释器，以命令 `python2` 或 `python3` 调用，分别对应两个版本的 Python。
#### 2.2 包管理器pip
为使用外部的第三方包，Python 提供了一个包管理器：pip。

pip 和 apt 之类的包管理器有相似之处：完成包的安装和管理，完成依赖的分析，等等。 **不过 pip 管理的是 Python 包，可以在 Python 代码中使用这些包**
#### 2.3 python依赖管理
一个软件一般含有众多依赖，尤其是对于追求易用、外部库众多的 Python 而言，使用外部库作为依赖是常事。

此处给出各种使用较多的 Python 依赖管理方案：
##### 2.3.1 requirements.txt
在一些项目下，你可能会发现一个名为 `requirement.txt` 的文件，里面是一行行的 Python 包名和一些对于软件版本的限制。例如，如果我想要下载django，且指明了pytest和pytest-cov的版本，则应当：
```bash
# requirements.txt
django
pytest>=3.0.0
pytest-cov==1.0.0
```

为了安装这些 Python 包，使用以下指令：
```bash
$ pip3 install -r requirements.txt
```

这将从 `requirements.txt` 文件中逐行读取包名和版本限制，并由 pip 完成安装。

此方案简单明了，易于使用，但对于依赖的处理能力不足！
##### 2.3.2 setuptools：setup.py
在 PyPI，即 pip 获取 Python 包的来源中，使用 setuptools 是主流选择。 setuptools 不是 Python 官方的项目，但它已成为 Python 打包（packaging）的事实标准。

常见状况是目录下会有一个名为 `setup.py` 的文件。 要安装依赖，只需：

```bash
$ ls
setup.py
$ pip3 install .
```

这种方案特点是使用广泛，易于对接，能提供的信息和配置较全，但配置起来也较复杂！
##### 2.3.3 其他的：pip-tools、pipenv
pip-tools 可以看作对 requirements.txt 的增强。 它额外提供了 `requirements.dev` 文件，从而完成了对于依赖进行版本锁定的支持。

pipenv 则是一个更加全面的解决方案，它提供了类似于 npm 的配置文件和 lock 文件，对于依赖有非常强的管理功能。 但其完成度和工业中的稳定性尚有待证明。

Python 有非常多的依赖管理方案，某种意义上讲是自带的 pip 管理功能不足所造成的。 一般而言，只需熟悉常用的 requirements.txt 和 setuptools 方案即可。
#### 2.4 虚拟环境
##### 2.4.1 常规安装的默认位置
- Python 通过包管理器如 apt 安装的包，默认安装在系统目录 `/usr/lib/python[version]` 下；
- 而通过 pip 安装的包，默认安装目录在 `/usr/local/lib/python[version]` 下；
- 当通过 pip 安装时显式传入了一个 `--user` 选项时，则会安装在用户目录 `~/.local/lib/python[version]` 下；
- 另外，在普通用户下直接执行 pip install 而没有传入 `--user` 选项时，也会因为用户没有系统目录的写权限而安装到用户目录。 

当普通地运行 Python 解释器时，上述这几个目录下的包均可见！
##### 2.4.2 问题产生
现在假设用户目录下已有一个包 `a`，版本为 `1.0.0`。 现在我们需要开发一个程序，也需要包 `a`，但要求版本大于 `2.0.0`。

由于 pip 不允许同时安装不同版本的同一个包，当你运行 `pip3 install a>=2.0.0` 时，pip 会更新 `a` 到 `2.0.0`， 那原先依赖于 `a==1.0.0` 的软件就无法正常运行了。

>PS：在一些 Shell（如 zsh）中，`>=` 有特殊含义。 此时上述命令应用引号包裹 `>=` 部分，如 `pip3 install 'a>=2.0.0'`
##### 2.4.3 解决问题
为了解决这一问题，允许不同软件使用不同版本的包，Python 提供了 Virtualenv 这个工具。 其使用方法如下：

一般 Virtualenv 会带在默认安装的 Python 中。 如果没有，可以用 `sudo apt install python3-venv` 来安装。

常见的做法是使用 Python 的模块运行来完成在 Shell 中的执行：
```bash
python3 -m venv .venv  # 这是我的命名习惯 (.venv)
```

以上指令中，`-m` 表示运行一个指定的模块，前一个 `venv` 指运行 venv 这个包的主模块 `__main__`， 后一个 `.venv` 是参数，为生成目录的路径。 这将使 venv(产生“虚拟环境”的包) 在**当前目录下**生成一个名为 `.venv` 的目录。

在一般的 shell 环境下，我们将使用 `source .venv/bin/activate` 来启用这个 venv。

完成以上操作后，你就进入了**当前目录下 .venv 文件夹所对应的 Virtualenv**。 此时，你使用 `pip3 install` 安装的 Python 包*将会被安装在 venv 这个文件夹中*， 这些包也只有在你 `source .venv/bin/activate` 之后才可见，*外部无法找到这些包*。 通过 `deactivate` 可以退出 Virtualenv，回到之前的环境中。

实际上，由于 Python 是借助一些环境变量来完成包搜索的步骤的，`source venv/bin/activate`其实是配置了一些环境变量，从而达到目的。这样，就实现了程序间依赖的隔离。

你可以在下列代码中轻松看出.venv的操作逻辑：（`la` = `ls -a`）
```bash
┌─(~/code_projects/ubuntu/muiti_files)────────────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:30:36 on main ✖ ✭)──> ls                                                        ──(三, 228)─┘
main     main.c   main.o   makefile print.c  print.h  print.o
┌─(~/code_projects/ubuntu/muiti_files)────────────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:30:38 on main ✖ ✭)──> la                                                        ──(三, 228)─┘
.        ..       .venv    main     main.c   main.o   makefile print.c  print.h  print.o
┌─(~/code_projects/ubuntu/muiti_files)─────────────────────
```

```bash
┌─(~/code_projects/ubuntu/muiti_files)────────────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:34:43 on main ✖ ✭)──> cd .venv                                                  ──(三, 228)─┘
┌─(~/code_projects/ubuntu/muiti_files/.venv)──────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:34:49 on main ✖ ✭)──> ls                                                        ──(三, 228)─┘
bin        include    lib        pyvenv.cfg share
┌─(~/code_projects/ubuntu/muiti_files/.venv)──────────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:34:51 on main ✖ ✭)──> cd bin                                                    ──(三, 228)─┘
┌─(~/code_projects/ubuntu/muiti_files/.venv/bin)──────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:34:54 on main ✖ ✭)──> ls                                                        ──(三, 228)─┘
Activate.ps1  activate.fish pip           pyftmerge     python3
activate      f2py          pip3          pyftsubset    python3.11
activate.csh  fonttools     pip3.11       python        ttx
┌─(~/code_projects/ubuntu/muiti_files/.venv/bin)──────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:34:56 on main ✖ ✭)──> cd ../lib                                                 ──(三, 228)─┘
┌─(~/code_projects/ubuntu/muiti_files/.venv/lib)──────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:35:10 on main ✖ ✭)──> ls                                                        ──(三, 228)─┘
python3.11
┌─(~/code_projects/ubuntu/muiti_files/.venv/lib)──────────────(huluobo@huluobodeMacBook-Pro:s000)─┐
└─(11:35:11 on main ✖ ✭)──>
```
#### 2.5 python的补充阅读
##### 2.5.1 python的版本
略
##### 2.5.2 python的其他实现
Python 作为一门编程语言，**官方的实现是 CPython**，我们一般使用的、成为事实标准的就是这个。 CPython 中的 *C 是指此解释器是用 C 实现*。

相应的，Python 还有其他的一些实现：
- JPython：将 Python 编译到 Java 字节码，由 JVM 来运行；
- PyPy：相较于 CPython，实现了 JIT（just in time）编译器，性能有极大地提升；
- Cython：引入了额外的语法和严密的类型系统，性能也有很大提升；
- Numba：将 Python 编译到机器码，从而直接运行，性能也不错。

视情况使用不同的 Python 实现能够很大程度地提升性能。 但如果你不确定自己的意向，且性能需求不大，使用官方的 CPython 也是明智之选。
##### 2.5.3 Anaconda 与 Miniconda
**1. Conda 是一个广泛使用的开源的包管理与环境管理系统：**
Miniconda 与 Anaconda 是两个广为人知的基于 Conda 的 Python 的发行版本。

**2. Miniconda 和 Anaconda 都是开源的 Python 的发行版本：**
Miniconda 是 Anaconda 的免费迷你版本，只包含了 Conda、Python 及其依赖，以及少量其他有用的包，例如 pip 和 zlib。而 Anaconda 则额外包含了 250 多个自动安装的科学软件包，例如 SciPy 和 NumPy，并且测试了这些软件包之间的兼容性。Anaconda 分为个人版、商业版、团队版、企业版，除了个人版以外，其余版本均为付费产品。

**3. Conda 作为包管理器：**
类似于 pip，可以使用 `conda install` 来安装软件包。部分软件包既可以使用 pip 安装，也可以使用 conda 安装。 相比于 pip，conda 会执行更加严格的依赖检查，并且除了 Python 软件包外，还可以安装一些 C/C++ 软件包，例如 cudatoolkit、mkl 等。相对的，conda 支持的 Python 软件包的数量远少于 PyPI。
## Chapter 12 - Docker
这一章详见笔者另一份笔记《Docker学习指南》
此处从略
## Chapter 13 - Shell高级文本处理与正则表达式
### 1. 其他常用的文本处理工具
#### 1.1 sort
sort 用于文本的行排序。默认排序方式是升序，按每行的字典序排序。

一些基本用法：
- `-r` 降序（从大到小）排序
- `-u` 去除重复行
- `-o [file]` 指定输出文件
- `-n` 用于数值排序，否则“15”会排在“2”前

```bash
$ echo -e "snake\nfox\nfish\ncat\nfish\ndog" > animals
$ sort animals
cat
dog
fish
fish
fox
snake
$ sort -r animals
snake
fox
fish
fish
dog
cat
$ sort -u animals
cat
dog
fish
fox
snake
$ sort -u animals -o animals
$ cat animals
cat
dog
fish
fox
snake
$ echo -e "1\n2\n15\n3\n4" > numbers
$ sort numbers
1
15
2
3
4
$ sort -n numbers
1
2
3
4
15
```
#### 1.2 uniq
uniq 也可以用来排除重复的行，但是*仅对连续的重复行生效*！

通常会和 sort 一起使用：`$ sort animals | uniq`

只是去重排序明明可以用 `sort -u` ，uniq 工具是否多余了呢？实际上 uniq 还有其他用途。

`uniq -d` 可以用于仅输出重复行：`$ sort animals | uniq -d`

`uniq -c` 可以用于统计各行重复次数：`$ sort animals | uniq -c`
### 2. 正则表达式拓展
正则表达式（regular expression, RE）描述了一种**字符串匹配的模式**，可以用来检查一个串是否含有某种子串、将匹配的子串做替换或者从某个串中取出符合某个条件的子串等。
#### 2.1 常用特殊字符

| 特殊字符   | 描述                                                                      |
| ------ | ----------------------------------------------------------------------- |
| `[]`   | 方括号表达式，表示匹配的字符集合，例如 `[0-9]`、`[abcde]`                                   |
| `()`   | 标记子表达式起止位置                                                              |
| `*`    | 匹配前面的子表达式*零或多次*                                                         |
| `+`    | 匹配前面的子表达式*一或多次*                                                         |
| `?`    | 匹配前面的子表达式*零或一次*                                                         |
| `\`    | 转义字符，除了常用转义外，还有：`\b` 匹配单词边界；`\B` 匹配非单词边界等！                              |
| `.`    | 匹配除 `\n`（换行）外的*任意单个字符*                                                  |
| `{}`   | 标记*限定符表达式的起止*。例如 `{n}` 表示匹配前一子表达式 n 次；`{n,}` 匹配至少 n 次；`{n,m}`匹配 n 至 m 次 |
| **\|** | 表明*前后两项二选一*                                                             |
| `$`    | 匹配字符串的结尾                                                                |
| `^`    | 常规是匹配*字符串的开头*；特殊：在方括号表达式**内部**表示*不接受该方括号表达式中的字符集合*                      |

以上特殊字符，若是想要匹配特殊字符本身，需要在之前加上转义字符 `\`。
#### 2.2 应用示例
匹配正整数
```bash
[1-9][0-9]*     
# 该数字第一位在1～9中；第二位在0～9中；第三位往后就“重复2的要求”，可延展零/多次
```
匹配仅由 26 个英文字母组成的字符串
```bash
^[A-Za-z]+$ 
# ^字符串开头；[...]表示接受由A-Z/a-z开头的字符串；+$表示到字符串的结尾了
```
匹配以除大写或小写字母之外的任何字符开头的字符串
```bash
[^A-Za-z]+$   
# 同上！
```
匹配 Chapter 1-99 或 Section 1-99
```bash
^(Chapter|Section) [1-9][0-9]{0,1}$
# 以Chapter或Section开头；随后的是数字1-9；下一位是数字0-9，可以匹配0或1次
# [针对[0-9]{0,1}.      eg: 个位数1，就匹配0次“第二位数字”；十位数66，就要匹配1次“第二位数字”]
```
匹配“ter”结尾的单词
```bash
ter\b
```
### 3. 正则文本处理工具
基本正则表达式（Basic Regular Expressions, BRE）和扩展正则表达式（Extended Regular Expressions, ERE）是两种 POSIX 正则表达式风格。

- BRE 可能是如今最老的正则风格了，对于部分特殊字符（如 `+`, `?`, `|`, `{`）需要加上转义符 `\` 才能表达其特殊含义。
- ERE 与如今的现代正则风格较为一致，相比 BRE，上述特殊字符默认发挥特殊作用，加上 `\` 之后表达普通含义。
#### 3.1 grep
grep 全称 Global Regular Expression Print，是一个*强大的文本搜索工具*，可以在一个或多个文件中搜索指定 pattern 并显示相关行。

grep 默认使用 BRE，要使用 ERE 可以使用 `grep -E` 或 egrep

命令格式：`grep [option] pattern file`

一些用法：

- `-n`：显示匹配到内容的行号
- `-v`：显示不被匹配到的行
- `-i`：忽略字符大小写

```bash
$ ls /bin | grep -n "^man$"            # 搜索内容仅含 man 的行，并且显示行号
$ ls /bin | grep -v "[a-z]\|[0-9]"    # 搜索不含小写字母和数字的行
$ ls /bin | grep -iv "[A-Z]\|[0-9]"   # 搜索不含字母和数字的行
```
#### 3.2 sed
==这一部分我个人认为不重要，可以忽略！==

sed 全称 Stream EDitor，即流编辑器，可以方便地*对文件的内容进行逐行处理*。

sed 默认使用 BRE，要使用 ERE 可以 sed -E。

命令格式：
```bash
$ sed [OPTIONS] 'command' file(s)
$ sed [OPTIONS] -f scriptfile file(s)
```

此处的 command 和 scriptfile 中的命令均指的是 sed 命令。

常见 sed 命令：

- s 替换
- d 删除
- c 选定行改成新文本
- a 当前行下插入文本
- i 当前行上插入文本

```bash
$ echo -e "seD\nIS\ngOod" > sed_demo
$ cat sed_demo
seD
IS
gOod
$ sed "2d" sed_demo  # 删除第二行
seD
gOod
$ sed "s/[a-z]/~/g" sed_demo  # 替换所有小写字母为 ~
~~D
IS
~O~~
$ sed "3cpErfeCt" sed_demo  # 选定第三行，改成 pErfeCt
seD
IS
pErfeCt
```
#### 3.3 awk
==这一部分我个人认为不重要，可以忽略！==

awk 是一种用于处理文本的编程语言工具，名字来源于三个作者的首字母。相比 sed，*awk 可以在逐行处理的基础上，针对列进行处理*。默认的列分隔符号是空格，其他分隔符可以自行指定。

awk 使用 ERE。

命令格式：`awk [options] 'pattern {action}' [file]`

awk 逐行处理文本，对符合的 patthern 执行 action。需要注意的是，awk 使用单引号时可以直接用 `$`，使用双引号则要用 `\$`。

```bash
$ cat awk_demo
Beth    4.00    0
Dan     3.75    0
kathy   4.00    10
Mark    5.00    20
Mary    5.50    22
Susie   4.25    18
$ # 选择第三列值大于 0 的行，对每一行输出第一列的值和第二第三列的乘积
$ awk '$3 >0 { print $1, $2 * $3 }' awk_demo
kathy 40
Mark 100
Mary 121
Susie 76.5
```

示例中 `$1`，`$2`，`$3` 分别指代本行的第 1、2、3 列。特别地，$0 指代本行。

awk 语言是「图灵完全」的，这意味着理论上它可以做到和其他语言一样的事情。这里我们不仅可以对每行进行操作，还可以定义变量，将前面处理的状态保存下来，以下是一个求和的例子：

```bash
$ awk 'BEGIN { sum = 0 } { sum += $2 * $3 } END { print sum }' awk_demo
337.5
```
### 4. 正则中的细节
注：以下正则语法如需在 `grep` 中使用，需要加上参数 `-P`，表示使用 Perl 语法！
#### 4.1 懒惰和贪婪
- 使用 `*` `+` 的时候默认是贪婪模式，即尽可能匹配更多的子表达式。
- 在 `*` `+` 之后加上 `?` 变为懒惰模式，即尽可能匹配更少的子表达式。

例如：`123456456`
=> 贪婪：`1.+6` -> `123456456`
=> 懒惰：`1.+?6` -> `123456`
#### 4.2 后向引用
后向引用可以**将之前匹配到的具体内容再次利用**。在正则表达式中，`( )` 以及它们包含的内容为一个分组，每个分组默认拥有一个组号。

组号分配规则：
- 0 代表整个表达式
- 从左至右，按左括号的出现顺序分配，第一个为 1，第二个为 2，以此类推
- 扫描两遍，第一次只分配未命名的组，第二次只分配命名的组。即任意命名组的组号都大于未命名的组号
#### 4.3 零宽断言
零宽断言用于查找某些内容进行定位，但内容并不放入匹配结果，就像 `\b` `^` `$` 的定位一样。`(?=exp)` 用于匹配表达式 `exp` 前面的位置，`(?<=exp)` 用于匹配后面的位置。

`\b\w+(?=ing\b)` 可以匹配 `ing` 结尾的单词前面的部分。例如 `going` 中的 `go` 会被该正则匹配。

`(?<=\bre)\w+\b` 可以匹配 `re` 开开头的单词的后面一部分。例如 `revue` 中的 `vue` 会被该正则匹配。
# 三. 后记
1. 这本书笔者阅读整理约48h，获益良多
2. 很显然我的阅读笔记并不适用于每一位读者
3. 读到这里相信你还会对一些地方有困惑，具体如下：

|        本文忽略的部分        |                                         对应资源                                          |
| :-------------------: | :-----------------------------------------------------------------------------------: |
| 如何在AppleSilicon中使用虚拟机 |                          [orbstack官网](https://orbstack.dev)                           |
|       ssh远程连接指南       | [GitHub-SSH](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh) |
|       tmux入门指南        |                  [tmux的github仓库](https://github.com/tmux/tmux/wiki)                   |
|         CMake         |               [笔者的CMake笔记](https://github.com/root-hbx/CMake_Tutorial)                |
|      Shell脚本的书写       |           [笔者的Shell脚本编程笔记](https://github.com/root-hbx/Shell_Prog_Tutorial)           |
4. 祝愿大家学有所成，早日成为Linux的“老鸟”！
