# 简介
## 本手册的目的
本手册主要用来学习linux的，对于linux的学习开始于我的大学时代，刚开始的目的仅仅是为了不玩游戏而想要抛弃windows阵营，但在office、学校管理系统、CAD尤其是腾讯系软件等在linux系统下的糟糕体验而最终没有坚持下来。  
这一次重新检起linux的学习，希望在学习的同时将其整理成手册以便以后查阅以及督促自己的学习。

## 发行版的选择
首先我们给出一个发行版的时间线：  
![发行版时间线](./img/gldt1210.png)  
[发行版时间线图表网站](https://futurist.se/gldt/2012/10/29/gnulinux-distribution-timeline-12-10/)

## 发行版介绍
从上面的图表中可以看到，发行版实在太多了。你可以在DistroWatch.com网站上查询到几百个不同的发行版。  
我们还可以发现linux发行版可以分为三个主要的分支：RedHat、Slackware和Debian。每一个分支下都拥有一个最具代表性的商业服务器级的发行版。分别对应的是Red Hat Enterprise Linux简称RHEL，SUSE Linux Enterprise简称SUSE，以及Ubuntu。接下来我们来分别介绍下这几个发行版：  
1. RedHat: 其本身是一个收费的发行版，当然也可以免费使用，但是你将得不到任何系统升级服务，也得不到任何技术支持。其包管理方式采用基于RPM包的YUM包管理方式。其重点在稳定性而非持续跟新上，所以其软件仓库中的软件比较少而且都相对比较老。通常被用于服务器端。  
    1. CentOS:该发行版是利用完全免费的RHEL的源代码重新编译而成的，免费提供给大家使用，所以本质上与RHEL没有什么区别。唯一的区别就是更新的频率没有RHEL快。
    2. Fedora:可以说是RHEL的试验田，RedHat将一些新的技术应用到该发行版中，以便观察是否稳定，当该技术稳定后会加入到下一版的RHEL中，所以通常fedora稳定性会比较差，最好只用于桌面系统。
2. Debian: 是社区Linux的典范，是迄今为止最遵循GNU规范的Linux发行版。其包管理采用apt/dpkg的方式，采用DEB包形式。其分为三个版本分支：stable为最稳定最安全版本通常用于服务器，unstable为最新的测试版本，包含最新的软件包，相对的也具有最多的bug。testing版本都经过unstable中的测试，相对比较稳定也支持不少新技术，一般用于桌面。  
    1. Ubuntu: 基于Debian的unstable版本加强而来。继承了Debian的优点，并且拥有自己独特的优势。可以说是拥有最多使用人群的linux发行版本。
        1. Elementary OS: 基于Ubuntu而来，被誉为最美linux发行版。其桌面环境称为Pantheon基于Gnome构建。
        2. Deepin: 基于Ubuntu而来，专为国人打造的Linux发行版。由深度科技开发，其中集成了深度音乐、深度截图、深度视频等程序。并且其应用程序仓库中包含QQ、阿里旺旺等国人必不可少的程序。
        3. Linux Mint: 提供了经典桌面配置的现代发行版，如果是从windows转换过来的新手，可以使用这个发行版本，他提供了普通家庭日常所需的几乎所有的应用。
3. Slackware: 与其他开发版不同，它坚持KISS(Keep It Simple Stupid)原则，也就是说没有任何配置系统的图形界面工具。在一开始使用的时候会相对困难些，但是会更加的灵活和透明。Slackware没有如RPM DEB之类的软件包管理方式。其软件包都是通常的tgz格式文件再加上安装脚本来完成的。
    1. openSUSE: 由德国一家公司开发的发行版。以Slackware为基础，并提供了完整的桌面环境和图形程序支持。
4. Arch Linux: 该发行版本采用滚动升级的策略，其尽力保持软件处于最新的稳定版本，只要不出现系统软件包破损都尽量使用最新的版本。所以他的软件仓库也包含最全的软件。其仓库中即提供开源、自由的软件，也包含闭源软件。其宗旨是实用性大于意识形态。Arch采用Pacman包管理系统。而且其官方Wiki以及社区我感觉是最好的。  
    1. manjaro: 该发行版基于Arch创建，其提供了一个更简单的方法来安装和使用基于Arch的发行版。Arch虽然是一个前瞻性很强的Linxu发行版，但对于新手并不太友好，而Manjaro就是为了解决这个问题而提供了。

## 本手册使用的发行版
看似很难抉择，但实际上没有什么，不管是谁家的发行版，其本质都是一样的。因为Linux本身就不是一个完整的系统，它实际上只是一个内核。所谓的发行版只不过是给这个内核加上一堆应用程序而组成一个操作系统而已。而这些应用程序很大一部分都是来源于GNU社区，代码都是一样的，所以其内在并没有多大的差别。  
其主要的差别体现在管理工具的选用上。不同的发行版可能对某种特性有不同的偏好。尤其是包管理的不同。目前比较主流的包管理有YUM红帽系，APT debian系以及pacman Arch系。  
所以针对于桌面系统：yum系使用fedora，apt系使用ubuntu、elementary OS、Deepin等，pacman系使用manjaro。而针对与服务器可以使用centos ubuntu server等。  
本手册采用centos发行版作为学习机，具体为centos7.7版本。之所以采用该系统纯粹是因为市面上的教程多数是以该发行版讲述的。

# 安装系统

省略

# 图形界面
linux本身是没有图形界面的。实际上linux只是一个内核。而我们之所以能通过图形用户界面来使用linux，是因为一个软件提供了该功能。  
其中图形界面有几个概念经常混淆： X/X11 XFree86/Xorg  KDE/GNOME。我们通过对这三个概念的解释来说明linux的图形界面是什么。  

## X不是程序而是协议
X是一个协议，就像HTTP协议、IP协议一样。一个基于X协议的应用程序需要运行并显示内容时他就连接到X服务器，开始用X协议与服务器交谈。比如一个X应用程序要在屏幕上输出一个圆，他就用X协议告诉X服务器。  
而X11 代表的是X协议的第十一个版本。类似于HTTP 1.1  

## XFree86是一个实现了X协议的服务器软件
XFree86是一个软件，他具体实现了X协议。就相当于Apache实现了HTTP协议一样。起初XFree86是以GPL许可发行的，后来开发商更改了许可协议，这就引起了GNU社区的不满，于是从XFree86 4.4 RC2衍生出了xorg。目前几乎所有开源的类Unix系统都使用xorg。

##  有了服务器还需要有X客户端程序
类似于Apache实现了HTTP协议的服务器软件，而浏览器就是一个客户端程序。同理xorg是一个实现了X协议的服务端软件，也就是需要一个客户端软件通常我们称之为窗口管理器(window manager WM)。其显见的作用就是绘制鼠标位置、应用程序的移动、最小化、最大化等。

## 图形界面操作环境是一系列软件的集和
KDE/GNOME是linux的图形界面操作环境。他们不仅是一个窗口管理器(KDE的WM是KWin，GNOME的WM是Metacity)，还有很多配套的应用软件和方便使用的桌面环境，比如任务栏、开始菜单、桌面图标等。

## DM又是什么鬼
对于那些默认使用图形用户界面的Linux系统，还有一个十分重要的X客户端需要启动，就是显示管理器(Display Manager DM)。这个是专门负责图形界面的用户登陆问题的。也就是说，系统启动之后第一个要启动的X客户端程序就应该是DM，而且没有人可以关闭它。

# linux用户和用户组管理

## 多用户多任务分时操作系统

- 多用户：可以让多个人使用同一台电脑而且不能互相窥探对方的秘密。  
- 多任务：当你使用电脑的时候可以一边听音乐一边玩游戏。
- 分时：将电脑的时间资源适当分配给所有使用者身上，让所有使用者有独占机器的感觉。
通常我们说一个操作系统会说他支持多用户多任务。这是因为一个分时系统，支持多任务是其与生俱来的本质。

## 用户的身份

Linux从诞生之日起就是多用户的，而对多用户的管理简单来说就是管理用户的等级以及用户对文件的访问权限。  
Linux下用户等级分为两个等级：root和非root。root用户在linux下拥有至高无上的权力，所以一个系统只能有一个root用户其用户名就是root。而非root用户的权力是受限的，只能访问由root规定的文件或者自己的文件。

### 澡堂子模型

Linux的用户管理方式可以比喻成一个澡堂子模型。非root用户就是澡堂的顾客，而root用户就是澡堂的管理员。linux系统就着这个澡堂本身。  
在去澡堂洗澡的时候，管理员会让你登记并发给你一个带有号码牌的钥匙。在linux中对应的就是root用户给其他用户分配帐号：登记相当于在系统添加用户，钥匙就是登陆系统的密码，号码牌就是用户名。  
获得钥匙后，就可以进入更衣室了。更衣室中有许多柜子，而你的钥匙就对应其中的一个。在linux中对应的就是：更衣室相当于home目录，而你的柜子就是自己的家目录。即/home/[username]  
当你一切准备就绪，就可以开始淋浴了。一旦离开了你的home目录，就进入公共区间了。一般情况下公共区间都是只读的，但是也有少数位置可以写入数据，比如/tmp目录。  
而在澡堂中有很多职业例如搓澡的、扫地的等，而客户也有许多职业但并不被澡堂关心所以通称客户。在linux中对应的就是用户组。在linux中一个用户可以属于多个用户组，并且一个用户至少应该属于一个用户组。
Linux的用户管理跟这个例子基本上是差不多的，只是粒度更加细腻一些。  

### 理解用户角色

linux是一个多用户操作系统，所以可以在linux中添加n多的用户，具体n的数字为2的32次方个。  
在linux系统中还有一些用户是用来完成特定任务的，比如nobody、admin、ftp等。需要注意的是在Linux系统中无论这些用户多么厉害多么特殊，主要不是root，他就一定是普通用户，权力大小都是相同的。  
虽然用户角色不能更权限靠上关系，但是不同的角色还是有待遇区别的，所谓的待遇就是是否拥有密码、home目录以及shell这些资源。例如nobody用户可以用于Nginx的工作进程。对于这类用户一般是不分配密码和shell的。甚至连home目录都没有。究其原因就是很多服务程序默认使用这些用户，如果设置了密码，程序就无法自动运行了。其次因为不会有人使用这个用户登陆系统也就没有必要分配shell以及设置home目录了。  


### /etc/passwd文件查看用户

该文件是系统用户配置文件，存储了系统中所有用户的基本信息。并且所有用户都可以对此文件执行读操作。  

``` bash
[hncjygd@bogon ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
...
```

可以看到，passwd文件中的内容非常规律，每一行对应一个用户。默认情况下Linux系统有非常多的用户，这些用户中的绝大多数是系统或服务正常运行所必须的用户通常被称为系统用户或伪用户。这些用户无法登陆也不能删除，一旦删除依赖于这些用户的服务或程序就不能正常运行了。  
每一行表示一个用户其具体含义如下：  
> 用户名:密码:UID:GID:描述性信息:家目录:默认shell  

- 用户名：一串代表用户身份的字符串
- 密码：全部都是x表示，并不是真正的密码，真正的密码保存在/etc/shadow文件中。虽然x并不代表真正的密码，但是也不能删除，如果删除了x那么系统会认为这个用户没有密码，从而导致只输入用户名而不用输入密码就可以登陆。
- UID：这是用户ID，每个用户都有唯一的UID，Linux系统通过UID来识别不同的用户。实际上UID就是一个0～65535之间的数。其中有一些约定俗成的规定：
    - 0：超级用户即root的UID
    - 1～999：系统用户，此范围的UID保留给系统使用。每个发行版可能不一样
    - 1000～65535：普通用户，例如你新建的第一个用户的UID就是1000.
- GID：用户组ID。其也是一个0～65535之间的数。其规定与UID差不多。但是还有一些不同的概念：
    - 初始组：在建立一个新用户的时候通常用该用户的用户名创建一个以该用户名相同的组名作为该用户的初始组。每个用户的初始组只能有一个
    - 附加组：指用户可以加入多个其他的用户组，附加组可以有多个
    - passwd文件中的GID是用户初始组的GID
- 描述性信息：这个字段没有什么重要的用途，默认与用户名相同。
- 家目录：默认情况下为/home/[userName] 
- 默认的Shell：shell就是Linux的命令解释器，是用户和Linux内核之间沟通的桥梁。通常你新建的用户使用linux系统默认使用的命令解释器bash(/bin/bash)，当然也可以使用sh、csh等。我们也可以将这个字段更改为其他程序： 
    - /bin/bash 表示该用户可以使用普通用户的所有权限
    - /sbin/nologin  改为此表示用户不能登陆
    - /usr/bin/passwd 表示用户可以登陆，但登陆后只能修改自己的密码  
    - 这里并不能随便写入和登陆没有关系的命令例如写成 /bin/ls ，系统并不能识别这个命令，同时也就意味着这个用户不能登陆

### /etc/shadow文件

/etc/shawdow文件，用于存储linux系统中用户的密码信息，又称为影子文件。  
在以前密码是存放在/etc/passwd文件，由于该文件允许所有用户读取，容易导致用户密码泄露。因此Linux系统将用户的密码信息从passwd文件中分离出来，并单独放到了此文件中。而/etc/shadow文件只有root用户才拥有读权限。  

``` bash
[root@bogon hncjygd]# cat /etc/shadow
root:$6$vFgSj9Ymd2fGksg5$ocC7VhtSejKyPrlFtULO6QRh7H8OVjBNlHObXKqmIOPS1AsEIeNXJgUqR9wzdq6k1gS.XfDN7.1BbzjI6QJrX/::0:99999:7:::
bin:*:17834:0:99999:7:::
daemon:*:17834:0:99999:7:::
adm:*:17834:0:99999:7:::
lp:*:17834:0:99999:7:::
sync:*:17834:0:99999:7:::
shutdown:*:17834:0:99999:7:::
halt:*:17834:0:99999:7:::
mail:*:17834:0:99999:7:::
```

其每行也代表一个用户，其具体信息为：  
> 用户名:加密密码:最后一次修改时间:最小修改时间间隔:密码有效期:密码需要变更前的警告天数:密码过期后的宽限时间:帐号失效时间:保留字段  

- 用户名：与/etc/passwd文件相同
- 加密密码：目前linux使用SHA512加密密码。一些系统用户的密码是!!或*，这代表没有密码。如果你在船舰用户时不设置密码的情况下该字段也为!!
- 最后一次修改时间：表示最后一次修改密码的时间。在linux中计算日期的时间是以1970年1月1日作为1不断累计得到的时间。到了1971年1月1日则为366。
- 最小修改时间间隔：该字段规定从最后一次修改时间算起，多长时间不能修改密码，如果是0就表示密码随时可以修改，如果是10则表示10天后才能修改密码。
- 密码有效期： 表示在最后一次修改时间算起多长时间内需要再次变更密码。默认为999999也就是273年，可以认为永久生效。管理服务器时，通过这个字段强制用户定期修改密码。
- 密码需要变更前的警告天数：当帐号密码有效期快到时，系统会发出警告信息告诉该账户需要修改密码
- 密码过期后的宽限天数：如果密码过期后没有被修改，允许宽限的天数。如果还没有修改系统将不再让此账户登陆。
- 帐号失效时间：同第三个字段相同，使用自1970年1月1日以来的总天数作为帐号的失效时间，该字段表示，帐号在此字段规定的时间之外，不论你的密码是否过期，都将无法使用。该字段通常被使用在具有收费服务的系统中。  
- 保留：这个字段目前没有使用  

### /etc/group  

该文件时用户组配置文件。对应于/etc/passwd文件中每行用户的GID。

``` bash
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:
cdrom:x:11:
mail:x:12:postfix
man:x:15:
hncjygd:x:1000:hncjygd
...
```

可以看到，每一行代表一个用户组。其含义为：  
> 组名:密码:GID:该用户组中的用户列表

- 组名：用户组的名称 
- 组密码：x仅仅是作为表示，具体密码位于/etc/gshadow文件中。这个密码主要用来指定组管理员的，由于系统中的帐号非常多，root用户可能没有时间进行用户的组调整，这时可以给用户组指定管理员，如果有用户需要加入或退出某个用户组，可以由该组管理员替代root进行管理。这个功能目前很少使用了，也就很少在设置组密码了。如果需要赋予某个用户调整某个用户组的权限，可以使用sudo命令替代。  
- GID：组的ID.Linux系统是通过GID识别组的，而组名只是为了便于管理员记忆。
- 组中的用户：此字段列出了每个组中的用户。需要注意的是如果该用户是这个用户的初始组，则该用户不会写入这个字段。多个用户是用逗号隔开的。

### /etc/gshadow文件

该文件存储了组用户的密码信息。

``` bash
root:::
bin:::
daemon:::
sys:::
adm:::
tty:::
disk:::
...
hncjygd:!!::hncjygd
```

其每行代表一个组用户的密码信息：  
> 组名:加密密码:组管理员:组附加用户列表  

- 组名：同/etc/group
- 加密密码：通常不是组密码，该这段通常为空。有时也为!指该群组没有组密码也不设置管理员。
- 组管理员：从系统管理员的角度来说，该文件最大的功能就是创建群组管理员。群组管理员的作用就是分担root任务，当某个用户想要加入某群组的时候，root来不及回应的时候可以让群组管理员来管理。在目前由于sudo的使用，该功能已经很少被提及了。
- 组中的附加用户：与/etc/group相同

## 管理用户和组

linux系统为用户和组的增删改查提供了一些基本的命令。这些命令的作用机制就是对/etc/passwd /etc/group则两个文件进行增删改查来完成的。外加一个/etc/shadow /etc/gshadow两个文件来专门保存相关的密码。

### 管理用户

#### useradd添加用户

> adduser/useradd  这两个命令都可以。通常我们使用useradd  
> 基本格式： useradd [选项] 用户名  

| 选项 |                                     含义                                      |
| ---- | ----------------------------------------------------------------------------- |
| -u   | 手动指定用户的UID                                                             |
| -d   | 手动指定用户的主目录                                                          |
| -c   | 手动指定用户说明信息                                                          |
| -g   | 手动指定初始组                                                                |
| -G   | 指定用户的附加组                                                              |
| -s   | 手动指定用户的shell，默认为/bin/bash                                          |
| -e   | 指定用户的失效日期，格式为YYYY-MM-DD                                          |
| -m   | 建立用户时强制建立用户的家目录，在建立系统用户时，该选项时默认的              |
| -r   | 创建系统用户，也就是UID在1～999之间，公系统程序使用，该参数默认不会创建家目录 |

> useradd usertest  #以默认设置创建用户  
> useradd -u 1100 -g usertest -c "test user" usertest1 #设置相应的参数来创建一个用户

默认参数由useradd的两个配置文件中定义：/etc/default/useradd文件以及/etc/login.defs文件

##### /etc/default/useradd文件  

其中

``` bash
# useradd defaults file
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

我们来逐一介绍：

- GROUP=100:这个选项用于建立用户的默认组。也就是说，在添加每个用户时，用户的初始组就是GID为100的这个组。(在CentOS并不是这样的，而是在添加用户时自动创建和用户名相同的组作为此用户的初始组。Linux中默认用户组有两种机制：一种私有用户组机制，即centOS这种，另一种就是公共用户组机制即使用useradd文件中定义的100)
- HOME=/home:指定用户主目录的默认位置，所有新建用户的主目录默认都在/home下
- INACTIVE=-1:指的是密码过期后的宽限天数，也就是/etc/shadow文件的第七个字段，默认-1，代表所有新建的用户密码永远不会失效
- EXPIRE= :表示密码失效时间，也就是/etc/shadow文件的第八个字段，默认为空，代表永久有效
- SHELL=/bin/bash:表示新建的用户默认的Shell
- SKEL=/etc/skel:在创建一个新用户后，你会发现，该用户主目录并不是空目录，而是有.bash_profile .bashrc等文件，这些文件都是从/etc/skel目录中自动复制过来的
- CREATE_MAIL_SPOOL=yes:指的是给新建用户建立邮箱，默认是创建。对于所有新建用户系统都会默认在/var/spool/mail/目录下创建和用户名相同的邮箱。

要想更改这个文件的内容，除了直接使用vim来改写这个文件，linux也为用户提供了useradd -D参数来修改这个文件中的内容。  

| 选项 |                  含义                   |
| ---- | --------------------------------------- |
| -b   | 设置HOME=的内容                         |
| -e   | 设置EXPIRE=的内容，参数格式为YYYY-MM-DD |
| -f   | 设置INACTIVE=的内容，参数为整数         |
| -g   | 设置GROUP=可以是UID也可以是组名         |
| -s   | 设置SHELL= 的内容                       |

##### /etc/login.defs文件

该文件是shadow密码套件配置。也是创建用户时对一些基本属性作默认设置。  

``` bash
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#

# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR	Maildir
MAIL_DIR	/var/spool/mail
#MAIL_FILE	.mail

# Password aging controls:
#
#	PASS_MAX_DAYS	Maximum number of days a password may be used.
#	PASS_MIN_DAYS	Minimum number of days allowed between password changes.
#	PASS_MIN_LEN	Minimum acceptable password length.
#	PASS_WARN_AGE	Number of days warning given before a password expires.
#
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                  1000
UID_MAX                 60000
# System accounts
SYS_UID_MIN               201
SYS_UID_MAX               999

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                  1000
GID_MAX                 60000
# System accounts
SYS_GID_MIN               201
SYS_GID_MAX               999

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD	/usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is overridden with the -m flag on
# useradd command line.
#
CREATE_HOME	yes

# The permission mask is initialized to this value. If not specified, 
# the permission mask will be initialized to 022.
UMASK           077

# This enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

# Use SHA512 to encrypt password.
ENCRYPT_METHOD SHA512
```

|          设置项          |                                                                            含义                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MAIL_DIR /var/spool/mail | 创建用户邮箱的位置                                                                                                                                         |
| PASS_MAX_DAYS 99999      | 密码有效期，99999默认273年即永久有效                                                                                                                       |
| PASS_MIN_DAYS 0          | 自上次修改密码以来最少相隔多少天才能再次修改密码                                                                                                           |
| PASS_MIN_LEN 5           | 指定密码的最小长度，默认为5位，但是现在用户登陆时验证已经被PAM模块取代，所以该选项并不生效                                                                 |
| PASS_WARN_AGE 7          | 指定密码到期前多少天，系统开始通知用户密码将到期                                                                                                           |
| UID_MIN 1000             | 指定最小UID为1000，也就是说添加用户时默认UID从1000开始。注意：如果手动指定一个用户UID时1100，那么下一个创建的用户的UID会从1101开始，哪怕1000～1099是空的。 |
| UID_MAX 60000            | 指定用户最大的UID为60000                                                                                                                                   |
| GID_MIN 1000             | 指定GID最小值为500                                                                                                                                         |
| GID_MAX 60000            | 指定GID最大值为60000                                                                                                                                       |
| CREATE_HOME yes          | 指定在创建用户时，是否同时创建用户的主目录，yes表示创建，no表示不创建                                                                                      |
| USERGROUPS_ENAB yes      | 指定删除用户的时候是否同时删除由该用户初始化的用户组                                                                                                       |
| ENCRYPT_METHOD SHA512    | 指定用户密码的加密规则，默认采用SHA512                                                                                                                     |

> 当我们执行了useradd命令后系统到底执行了什么？  
> useradd usertest  
> 1.在/etc/passwd 文件中创建一行数据(这也是为什么非root用户无法使用useradd的原因其对passwd文件没有写权限):  
> usertest:x:1001:1001::/home/usertest:/bin/bash  
> 2.在/etc/shadow文件中创建一行有关usertest的数据：  
> usertest:!!:18202:0:99999:7:::  
> 3.在/etc/group文件中创建一行关于usertest的用户组数据：  
> usertest:x:1001:  
> 4.在/etc/gshadow文件增加一行关于group密码的数据：  
> usertest:!::  
> 5.默认创建用户的主目录和邮箱：  
> /home/usertest/  
> /var/spool/mail/usertest  
> 6.将/etc/skel目录中的配置文件复制到新用户的主目录中  
> 以上就时useradd命令的全过程，如果你不想使用useradd命令，完全可以根据这个过程手动创建用户。

#### passwd修改用户密码

通过useradd命令创建了linux用户后。并没有设置用户的密码，表现就在/etc/shadow中密码列为!!,此时无法用该用户登陆系统，需要使用passwd来设置密码后才可以登陆。  

 > passwd [选项] 用户名  
 >从下面的选项可以看出，每个选项对应的都是对/etc/shadow中各个字段的修改，其实直接使用vim修改shadow文件可能会跟简单写但也相对危险一些  
 > 普通用户只能使用passwd修改自己的密码，而不能修改其他用户的密码  
 > 还要注意如果使用普通用户修改自己的密码的时候时需要被验证的即123这样的密码时无法被认可的。但是如果使用root用户修改密码，虽然依然无法通过验证但是可以强制修改为123这样的弱密码。

|  选项   |                                   意义                                   |
| ------- | ------------------------------------------------------------------------ |
| -S      | 查询用户的密码状态，其实就是查询shadow文件中此用户的密码内容             |
| -l      | 暂时锁定用户，该选项会在shadow文件中指定用户的加密密码前添加!,使密码失效 |
| -u      | 解锁用户，与-l选项相对应                                                 |
| --stdin | 可以通过管道符输出的数据作为用户的密码，主要在批量添加用户时使用         |
| -n      | 设置该用户修改密码后，多长时间不能再次修改密码                           |
| -x      | 设置该用户的密码有效期                                                   |
| -w      | 设置用户密码过期前的警告天数                                             |
| -i      | 设置用户密码失效日期                                                     |

> 如果忘记密码了怎么办？  
> 对于非root用户，如果忘记密码了，直接可以让root用户通过passwd命令来更改该用户密码。  
> 而对于root用户丢失了密码就需要一些特殊的方法了。  

#### usermod修改用户信息

通过useradd命令添加用户后，如果不小心添加错用户信息了，或者对默认的用户信息不满意时，后期可以通过usermod命令来修改这些信息。  

> usermod [选项] 用户名  
> 根据下面的选项可以看到基本上与useradd的参数设定差不多，但是一个是创建时设置，一个是创建后修改，其本质都是对/etc/passwd /etc/shadow文件的修改

| 选项 |             说明              |
| ---- | ----------------------------- |
| -c   | 修改用户说明信息              |
| -d   | 修改用户主目录                |
| -e   | 修改用户的失效日期 YYYY-MM-DD |
| -g   | 修改用户的初始组              |
| -u   | 修改用户的UID                 |
| -G   | 修改用户的附加组              |
| -l   | 修改用户的用户名              |
| -L   | 临时锁定用户                  |
| -U   | 解锁用户与-L相对              |
| -s   | 修改用户的登陆shell           |

#### chage修改用户密码状态

> chage [选项] 用户名  

| 选项 |                                                 含义                                                  |
| ---- | ----------------------------------------------------------------------------------------------------- |
| -l   | 列出用户的详细密码状态                                                                                |
| -d   | 修改 /etc/shadow 文件中指定用户密码信息的第 3 个字段，也就是最后一次修改密码的日期，格式为 YYYY-MM-DD |
| -m   | 修改密码最短保留的天数，也就是 /etc/shadow 文件中的第 4 个字段                                        |
| -M   | 修改密码的有效期，也就是 /etc/shadow 文件中的第 5 个字段                                              |
| -W   | 修改密码到期前的警告天数，也就是 /etc/shadow 文件中的第 6 个字段                                      |
| -i   | 修改密码过期后的宽限天数，也就是 /etc/shadow 文件中的第 7 个字段                                      |
| -E   | 修改账号失效日期，格式为 YYYY-MM-DD，也就是 /etc/shadow 文件中的第 8 个字段                           |

在实际应用中，如果你需要修改用户的密码信息，直接修改/etc/shadow文件可能更加方便。但是chage也是有其使用环境的。尤其是通过修改最后一次修改密码的日期即-d选项来强制让用户在首次登陆帐号时先修改密码。例如：chage -d 0 usertest #通过chage命令设置此帐号的密码创建的日期为1970年1月1日，这样用户在登陆后就必须先修改密码才能干别的。  
实际使用：如果一些学校要给学生发放一批用户，以学号作为用户名以及密码交给学生，并且设置change -d 0 学号 来让每个学生在第一次登陆时更改密码。

#### userdel删除用户

通过上面的学习我们知道，用户的相关数据包含在如下文件中：

- 用户基本信息：/etc/passwd
- 用户密码信息：/etc/shadow
- 用户组基本信息：/etc/group
- 用户组密码信息：/etc/gshadow
- 用户家目录：/home/[username]
- 用户邮箱：/var/spool/mail/[username]

而对于userdel命令的作用就是从以上文件中删除指定用户的数据信息。  

> userdel -r 用户名  
> -r选项表示在删除用户的同时删除用户的家目录以及邮箱  
> 你也可以手动删除指定用户，即通过对上面文件的修改来删除用户，但是这样作没什么意义错非你为了加深对userdel命令的理解。  
> 如果要删除的用户已经使用系统一段时间了，那么用户可能在系统的其他位置留有其他文件，仅仅使用userdel命令时无法删除该用户的使用痕迹的，这是你可以在使用userdel命令前，通过find -user 用户名 命令来查出那些文件属于该用户一并删除后在使用userdel命令删除用户。(find 默认搜索当前目录下，所以要搜索整个电脑可以切换到根目录搜索)

### 管理组

管理组与管理用户差不多，只是其中的user改为group即可。其本质也是对/etc/group /etc/gshadow文件的修改。  

- groupadd [选项] 组名 添加用户组
    - -g 指定组ID
- groupmod [选项] 组名 修改用户组
    - -g 修改组ID
    - -n 修改组名
- groupdel 组名 删除组
    - 其本质就是删除/etc/group /etc/gshadow中组相应的行。此命令仅仅适用于删除那些不是任何用户初始组的群组，否则无法成功。除非你使用usermod -g更改用户的初始组，或者删除用户。

#### gpasswd将用户加入群组或从组中删除

使用gpasswd命令可以为群组添加密码，设置群组管理员以及将用户加入群组或从组内删除。  

> gpasswd [选项] 组名  

|     选项     |                                                    功能                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------- |
| 空           | 选项为空时，表示给群组设置密码，仅 root 用户可用。                                                         |
| -A user1,... | 将群组的控制权交给 user1,... 等用户管理，也就是说，设置 user1,... 等用户为群组的管理员，仅 root 用户可用。 |
| -M user1,... | 将 user1,... 加入到此群组中，仅 root 用户可用。                                                            |
| -r           | 移除群组的密码，仅 root 用户可用。                                                                         |
| -R           | 让群组的密码失效，仅 root 用户可用。                                                                       |
| -a user      | 将 user 用户加入到群组中。                                                                                 |
| -d user      | 将 user 用户从群组中移除。                                                                                 |

> 注意：我们可以使用usermod -G命令将用户加入一个群组，但是同时会产生一个副作用，即使用该命令后会将用户以前加入的群组都清空(初始组除外)  
> 而使用gpasswd命令则不会出现问题，所以将用户加入某个群组最好使用gpasswd命令

#### newgrp切换用户的有效组

用户在创建文件的时候，该文件默认会有组权限，而这个文件所属的组由用户的有效组决定。而用户的有效组默认情况下就是用户的初始组。而newgrp命令可以从用户的附加组中选择一个群组作为用户新的有效组。这种切换是临时的，在关闭终端后重新开启终端其有效组还是初始组。  

``` bash
su root #切换到root用户
groupadd grouptest #新建组
gpasswd -a usertest grouptest #将usertest添加到grouptest组中
exit #退出root
touch test1 #新建一个test1文件
ls -al
-rw-rw-r--. 1 usertest usertest   0 11月  2 22:09 test1
newgrp grouptest  #切换有效组
touch test2 
ls -al
-rw-rw-r--. 1 usertest usertest   0 11月  2 22:09 test1
-rw-rw-r--. 1 usertest grouptest   0 11月  2 22:10 test2
exit #关闭终端
touch test3
-rw-rw-r--. 1 usertest usertest   0 11月  2 22:09 test1
-rw-rw-r--. 1 usertest grouptest   0 11月  2 22:10 test2
-rw-rw-r--. 1 usertest usertest   0 11月  2 22:13 test3
# 可见切换是临时的
```

总结：

- 默认情况下有效组就是初始组
- 通过newgrp可以临时切换有效组，前提是只能选择附加组中的组
- 要想永久切换有效组，可是使用 usermod -g 修改用户的初始组

## 其他有关命令

### id查询用户的UID和GID

> id 用户名  
> uid=1001(usertest) gid=1001(usertest) groups=1001(usertest),1002(grouptest)  
> uid表示用户id，gid是初始组id，groups是附加组列表

### su临时切换用户命令  

su是最简单的用户切换命令，通过该命令可以实现任何身份的切换。  
注意：普通用户到任何用户切换都需要知晓对方的密码，而root用户到任何用户的切换不需要对方密码直接切换成功。

> su [选项] 用户名  

| 选项 |                                                                           含义                                                                           |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -    | 当前用户不仅切换为指定用户的身份，同时所用的工作环境也切换为此用户的环境（包括 PATH 变量、MAIL 变量等），使用 - 选项可省略用户名，默认会切换为 root 用户 |
| -l   | 同 - 的使用类似，也就是在切换用户身份的同时，完整切换工作环境，但后面需要添加欲切换的使用者账号                                                          |
| -p   | 表示切换为指定用户的身份，但不改变当前的工作环境（不使用切换用户的配置文件）                                                                             |
| -m   | 和 -p 一样                                                                                                                                               |
| -c   | 仅切换用户执行一次命令，执行后自动切换回来，该选项后通常会带有要执行的命令                                                                               |
除了使用-c选项外，其他切换用户的方式不会自动切换回来，需要exit命令来退出。  

> 注意：  
> su和su - 区别是非常大的  
> 如果不切换用户环境例如使用su切换，可能导致很多命令无法执行。例如虽然普通用户可以使用su切换到root，但是使用的配置文件依然是普通用户的，其环境变量中不包含/sbin /usr/sbin等root用户命令的保存路径，这就导致你无法使用这里的命令除非全路径调用。  
> 简单的理解就是su切换的不彻底，而su - 切换的彻底就好了。

### whoami和who am i命令用于打印用户名时的区别

whoami:用于打印当前执行操作的用户名  
who am i:用于打印登陆当前linux系统的用户名  

加入我们使用usertest登陆了linux系统，在终端中使用su切换到root命令。此时执行whoami显示的就是root。而使用who am i显示的则是：usertest  pts/0        2019-11-02 22:17 (:0)  

对于：whoami等价于 id -un 命令 而who am i 等价于 who -m 命令。 

## sudo命令详解

lnux的用户除了root就是普通用户，而普通用户的权限非常的低，甚至连安装软件的权力都没有。很多时候系统管理员为了能让普通用户具备一些root用户的特权，就需要赋予他们相应的权限。使用su命令是一个方法，但是该命令需要告诉使用人管理员密码，并且root账户太过强大。因此赋予权限的就交给了一个叫做sudo的命令了。  
但是并非任何普通用户都拥有sudo特权。对于一些面向桌面的发行版，比如Ubuntu Fedora等，他们的设计理念就是日常应用，比较偏向赋予普通用户相对较为宽泛的权力，所以在系统的配置中，会给一类基于普通用户的管理员角色，这类角色被赋予了sudo特权。但是对于CentOS来说他主要面对服务器使用，强调的是安全可靠，所以在系统的配置中，除非root用户指定，否则不会让普通用户具有sudo特权。  
而给某个用户赋予sudo特权，实际上就是更改/etc/sudoers文件中的内容。  

> sudo [-b] [-u 新使用者帐号] 要执行的命令  
> -b: 将后续的命令放到背景中让系统自动运行，不对但前的shell环境产生影响  
> -u: 后面可以接需要切换的用户名，若无此项则代表切换身份为root  
> -l:用于显示当前用户可以用sudo执行那些命令


### /etc/sudoers文件  

首先让我们看看该文件中包含那些内容：  

``` bash
## Sudoers allows particular users to run various commands as
## the root user, without needing the root password.
##
## Examples are provided at the bottom of the file for collections
## of related commands, which can then be delegated out to particular
## users or groups.
## 
## This file must be edited with the 'visudo' command.

## Host Aliases
## Groups of machines. You may prefer to use hostnames (perhaps using 
## wildcards for entire domains) or IP addresses instead.
# Host_Alias     FILESERVERS = fs1, fs2
# Host_Alias     MAILSERVERS = smtp, smtp2

## User Aliases
## These aren't often necessary, as you can use regular groups
## (ie, from files, LDAP, NIS, etc) in this file - just use %groupname 
## rather than USERALIAS
# User_Alias ADMINS = jsmith, mikem


## Command Aliases
## These are groups of related commands...

## Networking
# Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, /sbin/dhclient, /usr/bin/net, /sbin/iptables, /usr/bin/rfcomm, /usr/bin/wvdial, /sbin/iwconfig, /sbin/mii-tool

## Installation and management of software
# Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum

## Services
# Cmnd_Alias SERVICES = /sbin/service, /sbin/chkconfig, /usr/bin/systemctl start, /usr/bin/systemctl stop, /usr/bin/systemctl reload, /usr/bin/systemctl restart, /usr/bin/systemctl status, /usr/bin/systemctl enable, /usr/bin/systemctl disable

## Updating the locate database
# Cmnd_Alias LOCATE = /usr/bin/updatedb

## Storage
# Cmnd_Alias STORAGE = /sbin/fdisk, /sbin/sfdisk, /sbin/parted, /sbin/partprobe, /bin/mount, /bin/umount

## Delegating permissions
# Cmnd_Alias DELEGATING = /usr/sbin/visudo, /bin/chown, /bin/chmod, /bin/chgrp 

## Processes
# Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall

## Drivers
# Cmnd_Alias DRIVERS = /sbin/modprobe

# Defaults specification

#
# Refuse to run if unable to disable echo on the tty.
#
Defaults   !visiblepw

#
# Preserving HOME has security implications since many programs
# use it when searching for configuration files. Note that HOME
# is already set when the the env_reset option is enabled, so
# this option is only effective for configurations where either
# env_reset is disabled or HOME is present in the env_keep list.
#
Defaults    always_set_home
Defaults    match_group_by_gid

# Prior to version 1.8.15, groups listed in sudoers that were not
# found in the system group database were passed to the group
# plugin, if any. Starting with 1.8.15, only groups of the form
# %:group are resolved via the group plugin by default.
# We enable always_query_group_plugin to restore old behavior.
# Disable this option for new behavior.
Defaults    always_query_group_plugin

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"

#
# Adding HOME to env_keep may enable a user to run unrestricted
# commands via sudo.
#
# Defaults   env_keep += "HOME"

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

## Next comes the main part: which users can run what software on 
## which machines (the sudoers file can be shared between multiple
## systems).
## Syntax:
##
## 	user	MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere 
root	ALL=(ALL) 	ALL

## Allows members of the 'sys' group to run networking, software, 
## service management apps and more.
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL

## Same thing without a password
# %wheel	ALL=(ALL)	NOPASSWD: ALL

## Allows members of the users group to mount and unmount the 
## cdrom as root
# %users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom

## Allows members of the users group to shutdown this system
# %users  localhost=/sbin/shutdown -h now

## Read drop-in files from /etc/sudoers.d (the # here does not mean a comment)
#includedir /etc/sudoers.d
```

要修改/etc/sudoers中的内容，不建议直接使用vim，包括在/etc/sudoers中也有明确要求使用visudo命令来修改sudoers文件，其实visudo的实际操作与vim一样，使用visudo的好处在于当修改完毕后在离开修改后的页面时系统会自动检验/etc/sudoers文件的语法正确性。  

其中有两条没有被注释，我们来分析：  

> root ALL=(ALL) ALL 和 %wheel ALL=(ALL) ALL  
> 用户名/群组名 被管理主机的地址(可使用的身份) 授权的命令  
> 用户名/群组名：表示系统中的那个用户或群组可以使用sudo，群组前要加百分号以与用户名区别  
> 被管理主机的地址：用户可以指定IP地址的服务器。ALL表示可以管理任何主机。如果写固定IP表示用户可以管理指定的服务器。如果写本机的IP代表指定用户可以从任何IP地址来管理当前服务器。  
> 可使用的身份：就是来源用户可以切换成什么身份也就是sudo -u后面可以接谁，ALL指代任意  
> 授权命令：表示将什么命令授权给用户ALL表示所有命令,要写命令的全路径  

我们使用几个具体的例子来解释如阿大夫编写sudoers文件：

``` bash
# 赋予usertest sudo的所有权限
usertest ALL=(ALL) ALL

# 赋予usertest 组 sudo的所有权限
%usertest ALL=(ALL) ALL

# 赋予usertest1 sudo的所有权限并且在执行sudo时不需要密码
usertest1 ALL=(ALL)  NOPASSWD:ALL

# 赋予usertest1 组 sudo的部分权限
# 即usertest1群组成员可以执行
#sudo mount /mnt/cdrom
#sudo unmount /mnt/cdrom
#其余命令不可以执行
%usertest1 ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom 

# 赋予usertest2组sudo除了adduser useradd权限外的所有权限
%usertest2 ALL=(ALL) ALL,!/user/sbin/adduser, !/user/sbin/useradd
```

需要注意的问题：  
CentOS系统默认提供了一个wheel用户组给与sudo权限，如果想要给与某个用户sudo权限只要在创建该用户的时候将其加入到wheel用户组即可。  
如果用户已经存在，也可以手动将用户加入到wheel群组中。  
此时会有一个问题：  
使用gpasswd -a usertest wheel 将用户加入wheel。此时我们在切换到usertest用户的时候发现还是无法使用sudo命令，这是因为此时我们的有效组依然是我们的初始组usertest，而wheel作为附加组无法被激活。此时我们可以执行newgrp wheel命令来临时将wheel作为当前的有效组，此时我们发现sudo命令就可以执行了。  
但是我们不可能每次新开一个终端的时候都需要执行newgrp wheel命令来提升有效组，这样就太没有效率了，其最终的解决办法就是通过 usermod -g wheel usertest将用户的初始组改变为wheel即可。