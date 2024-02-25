# 前置说明
1. 服务器端的运维需要掌握 Linux（或者其他类 Unix 系统）的基本使用以及进程、设备、网络等系统相关的基本概念:
- Reference1: [USTC《Linux101》](https://101.lug.ustc.edu.cn/)
2. 如果需要学习某个具体的概念或工具，比如常见的运维的基础知识和教程，例如 Docker, Kubernetes, Linux, CI-CD, GitHub Actions...
- Reference2: [DevOps-Guide](https://github.com/Tikam02/DevOps-Guide)
3. 这是笔者的学习笔记，由于很多Linux运维知识已进行模块化学习，本教程旨在“将碎片编成网”，因此这份教程是对知识的梳理总结，面向非零基础同学
4. 这份教程主要参考[Linux101_USTC](https://101.lug.ustc.edu.cn/)，但会在此基础上补充一些针对Mac命令的说明
5. 建议学习时长：60h
# 教程内容
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
#### GNU/Linux & Android/Linux
GNU 计划最初是为了对抗 UNIX 而建立的，这可以从 GNU 的全称 “GNU's Not UNIX”（GNU 不是 UNIX）中十分直观地看出来。GNU 计划的创始人理查德·斯托曼希望通过这个计划开发一个自由的操作系统 GNU，他将 GNU 操作系统视作“达成社会目的的技术方法”。

虽然 GNU 计划产生出了许多自由开源的组件和软件，但其核心的 GNU 操作系统却至今没有开发完成。在实际操作中，GNU 计划的组件通常都会依赖 Linux 内核作为系统核心来承载它们，它们在一起合称 GNU/Linux。在这里，Linux 内核是功能核心，而 GNU 组件则是这块核心的外设，也是用户操作和使用 Linux 内核的工具。众多当今通用计算机和服务器上主流的 Linux 发行版都属于 GNU/Linux。

如今智能手机上一个常见的系统是谷歌公司开发的 Android，它其实也是一个基于 Linux 内核开发的操作系统。不过它没有采用 GNU 组件作为工具，而是谷歌自行研发的另一套体系。基于这套体系构成的组合则被称为 Android/Linux，它和我们接下来讨论的 GNU/Linux 大相径庭，感兴趣的读者可以参考本章的拓展阅读：[Android/Linux](https://101.lug.ustc.edu.cn/Ch01/supplement/#android-linux)。
#### GNU自由软件
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
#### 服务器配置
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
#### 服务器IP地址获取
- 可以使用 `ip a` 命令查看服务器的 IP 地址
- 如果无法连接，请检查服务器的防火墙是否放行了 TCP 22 端口
#### 配置密钥登录
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
### 什么是 shell
上面所说的命令行，实际上指的就是 shell

shell 是 Linux 中的一类程序，它可以接受通过键盘输入的命令，然后把命令交给系统执行，并将命令的输出返回给用户。现在几乎所有的 Linux 发行版都提供了一个叫 Bash 的 shell 程序，是传统 shell 的“增强版”。
### 桌面环境的选择
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
#### 使用应用商店安装软件 (software)
在 Ubuntu 下，我们可以使用 Ubuntu Application Store 来进行安装
#### 使用包管理系统安装软件 (software)
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
#### 使用包管理器手动安装软件包 (software package)
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
#### cat 查看文件
```bash
$ # 输出 FILE 文件的全部内容
$ cat [OPTION] FILE
```
1. 输出 file.txt 的全部内容 : `$ cat file.txt`
2. 查看 file1.txt 与 file2.txt 连接后的内容 : `$ cat file1.txt file2.txt`
#### less 查看文件
```bash
$ # 在可交互的窗口内输出 FILE 文件的内容
$ less FILE
```
**less 和 cat 的区别**在于，cat 会一次性打印全部内容到终端中并退出，而 less 一次只显示一页，且支持向前/后滚动、搜索等功能。如果要在一个大文件中（例如 man page）查找一部分内容，less 通常要比 cat 方便得多
#### 编辑文件
1) nano: 启动后，用户可以直接开始输入需要的内容，使用方向键移动光标。在终端最下方是 nano 的快捷键，`^` 代表需要按下 Ctrl 键（例如，`^X` 就是需要同时按下 Ctrl + X）。在编辑完成后，按下 Ctrl + X，确认是否保存后即可
2) vim: 详见笔者的另一份笔记《Vim配置指南》
```bash
nano file.txt
vim file.txt
```
#### 赋值文件和目录
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
#### 移动文件和目录
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
#### 删除文件和目录
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
#### 创建目录与文件
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
#### 搜索目录与文件
```bash
$ # 在 PATH（路径）中根据 EXPRESSION（表达式）搜索文件
$ find [OPTION] PATH [EXPRESSION]
```

|EXPRESSION选项 |对应含义 |
|---|---|
|`-name '*.ext'`|文件名后缀为 ext。其中 `*` 是任意匹配符|
|`-type xxx` |文件类型为目录(xxx=d)，其他的类型例如 xxx=f（普通文件） |
|`-size +1M`|大于 1M 的文件，`+` 代表大于这个大小，对应地，`-` 代表小于之后的大小|
|`-or`|或运算符，代表它前后两个条件满足一个即可|
```bash
1. 在当前目录搜索名为 report.pdf 的文件：
$ find . -name 'report.pdf'

2. 全盘搜索大于 1G 的文件：
$ find / -size +1G

3. 在用户目录搜索所有名为 node_modules 的文件夹：
$ find ~/ -name 'node_modules' -type d
```

#### 模式匹配
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
#### man 命令
```bash
# 得到大部分安装在 Linux 上的软件的用户手册
man (xxx)
```

大部分软件在安装时会将它的软件手册安装在系统的特定目录， `man` 命令就是读取并展示这些手册的命令。在软件手册中，会带有软件的每一个参数的含义、退出值含义、作者等内容，大而全。但一般较少带有使用样例，需要根据自身需要拼接软件参数
#### tldr 命令
```bash
# 精简版文档，通过几个简单的例子让用户可以快速地一窥软件的使用方法
tldr (xxx)
```
### 5. tar 的替代与其他压缩软件
#### unar
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
#### 处理 ZIP 压缩包：`zip` 与 `unzip`[¶](https://101.lug.ustc.edu.cn/Ch03/supplement/#zip "Permanent link")

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
##### htop
Htop 可以简单方便查看==当前运行的所有进程==，以及系统 CPU、内存占用情况与系统负载等信息

使用鼠标与键盘都可以操作 htop。Htop 界面的最下方是一些选项，使用鼠标点击或按键盘的 F1 至 F10 功能键可以选择这些功能，常用的功能例如搜索进程（F3, Search）、过滤进程（F4, Filter，使得界面中只有满足条件的进程）、切换树形结构/列表显示（F5, Tree/List）等等
##### ps
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


#### 2.2 终止进程


#### 2.3 脱离终端


#### 2.4 多终端解决方案—tmux

