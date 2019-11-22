## pacman软件包管理器

pacman软件包管理器是ArchLinux系的一大特色(Manjaro发展自ArchLinux)，Manjaro的所有版本都包含pacman。对于Linux来所软件包管理器的重要性是毋庸置疑的，每个发行版的不同可以说其最大的区别在于软件包管理器的不同。以下列出了常见的发行版及其软件包管理器：  

| 软件包管理器 | 安装包名称 | 发行版系  |  衍生发行版   |
| ------------ | ---------- | --------- | ------------- |
| yum/rpm      | RPM        | Red Hat   | CentOS fedora |
| apt/dpkg     | DEB        | Debian    | Ubuntu deepin |
| pacman       | pkg.tar.xz | ArchLinux | manjaro       |

pacman软件包管理器将一个简单的二进制包格式和易用的构建系统结合起来，不管软件包是来自官方库还是用户自建库，pacman都能方便地管理他们。pacman通过和主服务器同步软件包列表来进行系统更新。这种服务器/客户端模式可以使用一条命令来下载并安装软件包同时解决必需的依赖。

## 用法介绍

> pacman \<operation\> [options] [targets]

### pacman -S,--sync 同步操作

同步软件包，软件包从远程仓库中被下载下来并安装，包括安装所有需要运行该软件包的依赖项目。其可用选项如下：

1. 不添加选项即安装软件
pacman -S操作最主要的的用处是用来安装软件的。

    1. 安装单个或多个包
    > pacman -S package_name1 package_name2...
    2. 如果不同的软件仓库(如extra testing)有一个软件包的多个版本
    > pacman -S extra/package_name
    3. 要安装一个包组(gnome)会让你选择其中几个(默认为all)，除了逐一键入序号外还可以：
    > Enter a selection (default=all): 1-10 15

2. 清除缓存 -c,--clean
pacman将下载的软件包保存在/var/cache/pacman/pkg/下并且不会自动移除他们，因此需要手动清理,使用一个可是删除未安装的软件，如果想要清除所有的软件连续使用两个选项
    1. 清除所有未安装的软件包
    > pacman -Sc
    2. 清楚所有软件包
    > pacman -Scc

3. 显示指定的软件包组成员 -g,--groups
一些软件是一系列的软件包组成的，他们就是软件包组。通常这类软件都是大型的例如gnome这样的图形桌面。通过该选项可以列出该软件包组包含的成员
    1. 列出软件包组包含哪些软件包
    > pacman -Sg gnome

4. 列出指定仓库的所有软件包 -l,--list
如果添加targets参数会列出指定仓库的所有软件包，如果不加会列出所有仓库的所有列表。是一个很长的列表可以通过配合grep命令使用来寻找指定的软件包。注意仅仅列出而并不安装
    1. 列出所有软件包找到包含netease字符串的软件包
    > pacman -Sl | grep netease

5. 搜素软件包名称或描述 -s,--search [regexp]
将在每个仓库中搜索与regexp匹配的名称或描述。其与-l配合grep的不同之处在于会搜索软件描述，并且其打印出来的信息也包含软件描述
    1. 列出包含netease字符串的软件包及软件描述
    > pacman -Ss netease

6. 更新远程服务器软件包数据库 -y，--refresh
pacman对于定义在/etc/pacman.conf中指定的软件仓库，都会下载一个对用的数据库通常位于/var/lib/pacman/sync/目录下而该选项就是更新这里的数据库文件，通常我们在更新系统的时候会先更新下这里。如果接i两个该选项表示强制更新即使比对结果本地的已经是最新的了。

7. 更新所有软件包 -u，--sysupgrade
顾名思义会升级所有过期的软件包。通常情况下在更新前需要更新本地数据库。
    1. 更新所有软件
    > pacman -Syyu

8. 显示有关给定软件包的信息
    1. 显示某个软件的元信息
    > pacman -Si package_name

### pacman -R,--remove 删除操作
从系统中删除软件包，也可以指定要删除的组。使用该选项会删除属于指定软件包的文件，数据可也随之刷新。除非使用--nosave选项否则大多数配置文件将得到保留并以.pacsave扩展名保存。还有由该软件创建的文件并不会被删除。

1. 不添加任何选项
在不添加任何选项的时候删除某些软件包会保留其配置文件并一.pacsave扩展名保存。并且还保留其依赖。
    1. 删除单个软件，并保留其安装的依赖
    > pacman -R package_name

2. 忽略配置文件 -n --nosave
即不再备份配置文件，删除所有属于该软件包的文件。
    1. 删除所选软件包括配置文件
    > pacman -Rn package_name

3. 递归删除所有目标软件包以及他们的依赖 -c,--cascade
这个选项是非常可怕的，会递归删除软件包以及其依赖，以及其依赖的依赖。这可能会删除大量的软件包并且导致某些软件无法使用。通常情况下不要使用该命令，这个命令通常是用来删除gnome这样的图形环境使用的。
    1. 递归删除软件包及其依赖
    > pacman -Rc package_name

4. 删除软件以及不需要的依赖 -u，-unneeded
他是-c的安全删除方式，他对于依赖并不是全部删除而是会检查其他软件包是否需要他们只有当不需要他们的时候才会删除该依赖。这个选项应该是我们最常使用的软件删除选项。
    1. 删除软件包以及其他软件不需要的依赖
    > pacman -Ru package_name

### pacman -Q, --query 查询软件包数据库
所有对远程仓库的操作都是使用的-S选项，查看远程仓库中的软件为-Ss(实际上也是查询的本地数据库，该数据库包含了远程仓库中所有的软件包内容)，而-Q是对本地已经安装的软件包数据库进行操作的。他可以查看已安装的软件包及其文件，以及各个软件包的元信息(依赖性、冲突、安装日期、构建日期、大小等)。如果命令后不添加任何target，则针对的是本机上所有的安装包，此时可以使用grep来过滤以找到相应的内容。

1. 显示属于命名组的所有包 -g, --groups
如果未指定软件包名称，会显示所有安装在系统中的软件包组。(仅仅列出包组即类似gnome fcitx这样的软件包组)
    1. 列出系统所有软件包组
    > pacman -Qg
    2. 列出属于fcitx软件包组的所有软件
    > pacman -Qg fcitx

2. 显示软件包的信息 -i, --info
默认情况下，显示的是本地数据库中提供的信息，与-p选项配合可以查看具体某个pkg.tar.xz软件包文件的信息。
    1. 查看某个软件包元信息
    > pacman -Qi package_name
    2. 查看某个软件包文件的元信息
    > pacman -Qif package_name.pkg.tar.xz

3. 列出给定软件包拥有的所有文件 -l. --list
如果不添加参数，默认列出所有已经安装的软件包拥有的文件。他并不能列出由该软件生成的文件。
    1. 列出某个软件包拥有的所有文件
    > pacman -Ql package_name

4. 搜索本地安装的软件包以及描述 -s, --search [regexp]
    1. 搜索符合要求的软件包或描述文件
    > pacman -Qs string

5. 搜索某个文件属于那一个安装包 -o, --owns [file]
    1. 指定这个文件属于那一个软件包的
    > pacman -Qo file

6. 检查系统中安装的软件包的完整性
实际上就是比对服务器仓库与本地软件数据库的区别。
    1. 检查软件的完整性
    > pacman -Qk package_name

7. 指定软件包文件 -p, -file
需要在其后面跟pkg.tar.xz软件包，这样搜索的目标不再是本地数据库而是软件包本身。
    1. 列出指定软件包信息
    > pacman -Qip file_name
    2. 列出指定软件包中的内容
    > pacman -Qlp file_name

### pacman -U --upgrade 将一个软件包升级或添加到系统
将一个软件包升级或安装到系统中并安装所需要的依赖。用于安装本地包。
    1. 安装一个本地包
    > pamcan -U package_name.pkg.tar.xz

### pacman总结

#### 常用命令

1. 安装软件
> pacman -S package_name

2. 卸载软件
> pacman -Ru package_name

3. 更新系统
> pacman -Syyu 

4. 查询仓库中软件
> pacman -Ss package

5. 查询本地软件信息
> pacman -Qi package_name

6. 查询软件包括的文件信息
> pacman -Ql package_name

7. 查询某个文件属于哪一个软件包
> pacman -Qo file_name

#### pacman中-S与-Q中有关查询的异同点

- 不同点
    - -S是对定义在pacman.conf中远程服务器仓库中所有文件的查询
    - -Q是对位于已经安装在本地系统上的软件进行查询
    - 实际上-S搜索位于/var/lib/pacman/sync/目录下的数据库
    - -Q搜索的是位于/var/lib/pacman/local/目录下的数据库
- 相同点
    - 都具有 -l -i -s -g 选项
    - -i 得到软件包元信息
    - -l 列出所有软件包
    - -s 在数据库中查找软件包及信息
    - -g 查找软件包组

## Pacman软件包使用到的配置文件和目录

### /var/cache/pacman/pkg/目录
该目录保存了所有pacman下载的安装或未安装的软件包。可以通过 pacman -Scc 命令来清空。

```
[hncjygd@hncjygd-pc pkg]$ ls -l /var/cache/pacman/pkg/
total 2347408
-rw-r--r-- 1 root root    135108 11月 13 09:14 acl-2.2.53-2-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root     50332  9月 23 05:34 acpid-2.0.32-1-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root  11186892 11月  9 00:41 adwaita-icon-theme-3.34.3-1-any.pkg.tar.xz
-rw-r--r-- 1 root root   1837928 11月  7 10:03 android-tools-29.0.5-1-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root      7040 11月  7 10:03 android-udev-20191103-1-any.pkg.tar.xz
-rw-r--r-- 1 root root   2022652  9月 26 14:55 appstream-0.12.9-2-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root    400436 10月 23 16:23 appstream-glib-0.7.16-2-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root     62876  9月 26 14:55 appstream-qt-0.12.9-2-x86_64.pkg.tar.xz
-rw-r--r-- 1 root root  16571096 10月 27 18:37 archlinux-appstream-data-20191027-1-any.pkg.tar.xz
-rw-r--r-- 1 root root    249088 10月 29 16:17 archlinuxcn-keyring-20191029-1-any.pkg.tar.xz
-rw-r--r-- 1 root root    252932 11月 21 14:16 archlinuxcn-keyring-20191122-1-any.pkg.tar.xz
-rw-r--r-- 1 root root    902676 10月 21 01:48 archlinux-keyring-20191018-1-any.pkg.tar.xz
-rw-r--r-- 1 root root   1258048 11月  8 02:53 ark-19.08.3-1-x86_64.pkg.tar.xz
...
```

### /var/lib/pacman/sync/目录
用于存放远程仓库的软件包数据库,通过使用pacman -Sy命令可以跟新该目录下的数据库文件。

```
[hncjygd@hncjygd-pc pkg]$ ls -l /var/lib/pacman/sync/
total 8732
-rw-r--r-- 1 root root 1288097 11月 21 14:16 archlinuxcn.db
-rw-r--r-- 1 root root 5483492 11月 18 18:04 community.db
-rw-r--r-- 1 root root  151182 11月 18 18:04 core.db
-rw-r--r-- 1 root root 1799684 11月 18 18:04 extra.db
-rw-r--r-- 1 root root   22010  9月 11 23:20 mhwd.db
-rw-r--r-- 1 root root  185404 11月 18 18:04 multilib.db
[hncjygd@hncjygd-pc pkg]$ file /var/lib/pacman/sync/core.db 
/var/lib/pacman/sync/core.db: gzip compressed data, last modified: Sun Nov 17 16:19:42 2019, from Unix, original size modulo 2^32 587264
```

可以看到实际上的数据库文件就是一个gzip压缩包。  
改名并解压缩该文件：
> mv core.db core.gz  
> gunzip core.gz  

此时得到一个core文件，file得知是一个tar包文件：
> tar -xf core  

解包后得到如下列表：
```
drwxr-xr-x 2 hncjygd hncjygd     60 11月 13 09:15 acl-2.2.53-2
drwxr-xr-x 2 hncjygd hncjygd     60 11月 18 00:19 amd-ucode-20191022.2b016af-1
drwxr-xr-x 2 hncjygd hncjygd     60 10月 21 01:48 archlinux-keyring-20191018-1
drwxr-xr-x 2 hncjygd hncjygd     60  7月  4 23:49 argon2-20190702-1
drwxr-xr-x 2 hncjygd hncjygd     60 11月 13 09:15 attr-2.4.48-2
drwxr-xr-x 2 hncjygd hncjygd     60  6月 24 16:34 audit-2.8.5-3
drwxr-xr-x 2 hncjygd hncjygd     60 11月  9  2018 autoconf-2.69-5
drwxr-xr-x 2 hncjygd hncjygd     60  9月 25  2018 automake-1.16.1-1
drwxr-xr-x 2 hncjygd hncjygd     60  6月  2  2018 b43-fwcutter-019-2
drwxr-xr-x 2 hncjygd hncjygd     60 10月  7 01:52 base-2-1
drwxr-xr-x 2 hncjygd hncjygd     60 11月 18 00:19 bash-5.0.011-1
...
```
可以看到是一系列文件夹每个文件夹对应一个软件包。我们单独拿出来一个文件夹来分析。可以看到每个文件价下面都是一个desc文件，该文件定义了该软件的一系列信息：
```
%FILENAME%
filesystem-2019.10-3-x86_64.pkg.tar.xz

%NAME%
filesystem

%BASE%
filesystem

%VERSION%
2019.10-3

%DESC%
Base Manjaro Linux files

%GROUPS%
base

%CSIZE%
8632

%ISIZE%
274432

%MD5SUM%
5d468c7756a6105453284122ac7cedff

%SHA256SUM%
263c573b87d4b4c8227b9464ab011ba5cff0f307e48506b081c2541a36c9a5ca

%PGPSIG%
iQEzBAABCAAdFiEEOfDsGuULN+XzGW8J2tOyEWY8omgFAl2uHk8ACgkQ2tOyEWY8omi+DAgA3puG7jsVdCVBmk+sy1eoNsQjZd5r07tbK5K/tAl+gJWZJbUDci7W2K1bxn1hYl5+ZJMQtOLYfjBU7q5Jr0MJr9R2gq1A1XXcAaLv38W0NIzsWzlDjtqWlFqzi3DNLpPzylMg2CFnQyQvwNNGeVYCdZwH3/OImRCmPWN3eVw6ajg1N2kU9JxAeQYljiW594U+YHcrxzZ+hwcgtV+ZzP+tGKK2fRGaTjGeP1ENZCF/bqBQUShE9E9JjuL9cexdxhRZHAreOAPBXy0pJ1jiDFJATUwhUMjm+W6GSQ3HHjR9xdCvMkhS4QO9EQcSiKCn49WsCD3T5/02w/QtxzAOXTzYbA==

%URL%
https://manjaro.org

%LICENSE%
GPL

%ARCH%
x86_64

%BUILDDATE%
1571692108

%PACKAGER%
Bernhard Landauer <oberon@manjaro.org>

%DEPENDS%
iana-etc
```
其中描述了该软件的名称、大小、版本、依赖、MD5值等等信息。

### /var/lib/pacman/local/目录
用于存放本地已经安装的软件包相关数据，打开可以发现其中是一个个文件夹，每个文件夹对应一个个已经安装的软件。其中每个软件对应的文件夹下面基本上都会有三个文件(对于GUI程序会有四个)，分别是desc,files,mtree。其中desc与上面相同，而files表示该软件所有文件的安装目录类似与pacman -Ql package_name的结果。而mtree是一个gzip压缩包文件。解压出来观察：
```
#mtree
/set type=file uid=0 gid=0 mode=644
./.BUILDINFO time=1567549722.0 size=5615 md5digest=f78c30718394df0f3fb5fa3e912c3c50 sha256digest=f379ae68073403558afff1e96d4e4b8589e82034499036f238b3556a89c0357b
./.PKGINFO time=1567549722.0 size=426 md5digest=28cea1b09c06bf295c742548aef47303 sha256digest=1194070d266b824b97d0d91015d3bc1fdce6447332e1b14a0f67ef4443ca352a
/set mode=755
./usr time=1567549722.0 type=dir
./usr/bin time=1567549722.0 type=dir
./usr/bin/scrot time=1567549722.0 size=43480 md5digest=2f0a53b45390dae090b84dd2a45d9f17 sha256digest=2944f1c489eb1037d43c7f677f07c97291dbee5cea849bb3c222359c1c846735
./usr/share time=1567549722.0 type=dir
./usr/share/doc time=1567549722.0 type=dir
/set mode=644
./usr/share/doc/scrot time=1567549722.0 mode=755 type=dir
./usr/share/doc/scrot/ChangeLog time=1567549722.0 size=7842 md5digest=588bec9d4b3d83888b2e8aa1f39837d9 sha256digest=5692c0bd5e8670966fe2da61d50d3ef06ab4ff10fddc482e1acf8d01f84f262d
./usr/share/doc/scrot/README time=1567549722.0 size=1722 md5digest=865767b346ef99d88b72b9e41825e5b4 sha256digest=8820bd15bf839151f1b0c75a54a985ffc41dff7ee715d99956c335891850d26d
./usr/share/licenses time=1567549722.0 mode=755 type=dir
./usr/share/licenses/scrot time=1567549722.0 mode=755 type=dir
./usr/share/licenses/scrot/COPYING time=1567549722.0 size=1144 md5digest=dd3cb8d7a69f3d0b2a52a46c92389011 sha256digest=8601e2dacede853fe325f7fd3a11f93b1753d576db60426f1b0ff4e68f41ff07
./usr/share/man time=1567549722.0 mode=755 type=dir
./usr/share/man/man1 time=1567549722.0 mode=755 type=dir
./usr/share/man/man1/scrot.1.gz time=1567549722.0 size=2011 md5digest=f382eb39a90345c4dd0c3c370bd3df33 sha256digest=3c8ca81a497c3ca45fc0dc021074d11644d2ed80dc8b680d1b1d8bfcc38e232f
```
其中记录了软件安装每个软件时的时间、类型、权限、大小、md5值等等信息。主要用来查看软件以后是否被改动。pacman -Qk 命令就是用该文件中的信息与实际文件比对的结果。  

**特别重要!!!!**  
这个文件目录非常的重要，如果你删除了这个目录中的文件，就相当于阻断了pacman对系统软件的管理，会出现一下问题：
- 通过pacman -Q不会有任何输出
- pacman -Syu会错误的报告系统已经是最新
- pacman -S安装软件包时，很多已经安装过的依赖依然提示未安装。  

当然如果系统出现了以上问题也就可以断定是这个文件夹中的数据出现了错误。这样基本上就面临着需要重装系统了。当然也可以根据ArchWiki中的介绍来重建本地软件数据库。

### /etc/pacman.conf文件
pacman软件的配置文件,其中最重要的就是定义软件仓库：
```
[core]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist

[extra]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist

[community]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist

# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

[multilib]
SigLevel = PackageRequired
Include = /etc/pacman.d/mirrorlist

# An example of a custom package repository.  See the pacman manpage for
# tips on creating your own repositories.
#[custom]
#SigLevel = Optional TrustAll
#Server = file:///home/custompkgs
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

```
其结构为：  
- [存储库] 
    - core 核心库，包括manjaro的启动、网络配置、文件系统、设置等和行内容
    - extra 额外库，包括Xwindow、浏览器、python等
    - community 社区库，社区分享的一些应用程序
    - multilib 其中包含了32软件和库，可用于在64位系统中使用
    - 非官方库 非官方维护的库 例如上面的 archlinuxcn 即由arch中文社区维护的库 
- SigLevel 设置默认签名的级别，由两个字段决定
    - 何时检查
        - Never 即使存在签名也禁止检查
        - Optional 默认，如果存在就检查，没有签名并不是错误而无效签名才是错误
        - Required 需要签名，缺少签名或无效签名都是错误
    - 什么样的签名被允许
        - TrustedOnly 默认，如果检查了签名，这该签名必须位于密码环中并且完全受信任
        - TrustAll  如果检查了签名，该签名必须位于密码环中但是不需要分配信任级别
    - 上述命令前面还可以添加Package或Datebase前缀，将导致它仅针对于该类型生效
    SigLevel实际上就是对上面参数的组装
- Server 定义服务器地址
    - Include 参数可以理解为将文件/etc/pacman.d/mirrorlist引入，本机电脑其中定义了Server = https://mirrors.ustc.edu.cn/manjaro/stable/$repo/$arch
        - $repo变量定义为当前节的名称
        - $arch变量值为Architecture即架构，本机为x86_64
        - 所以对于[core]库其server的实际地址就是：https://mirrors.ustc.edu.cn/manjaro/stable/core/x86_64 我们可以打开这个网址发现其下是一大堆的pkg.tar.xz软件包

## 什么是镜像(镜像服务器)
在GUN/Linux发行版的世界里，镜像服务器就是一台服务器，其中存储了各个发行版软件包的最新副本。该副本存储在版本库[repos]中。版本库分为官方版本库以及非官方的。镜像中通常存储有多个版本库，他们按照类别保存软件，例如manjaro就分为core、extra、community、multilib等。虽然官方提供了一台服务器用于存储版本库，但是为了更快的升级manjaro需要更多的镜像服务器，而其他的镜像服务器都是sync自其他镜像服务器中的。[manjaro镜像服务器列表](https://repo.manjaro.org/) 中罗列了manjaro的镜像服务器列表。其中在中国由中科大、上海交大以及清华提供了manjaro的版本库。我们可以任意选择一个使用。

### 设置国内镜像源

#### 自己定义源地址

既然知道了pacman读取服务器地址的配置文件是pacman.conf只需要修改该文件内容即可。例如添加：
```
[core]
SigLevel = PackageRequired
Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch
```
类似上面的条目来设置每个版本库的源。当然也可以通过Include来引入。此时定义/etc/pacman.d/mirrorlist文件是更加快捷的方式。在其中添加或变更：
> Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch

当然在设置完成后不要忘记执行 pacman -Syy 这个命令会将跟新后的镜像源中的数据库文件下载到/var/lib/pacman/sync文件夹中 

#### 使用pacman-mirrors

pacman-mirrors是manjaro特有的用于生成和维护系统镜像列表的命令。为了可以从repo.manjaro.org上下载最新的镜像源地址，pacman-mirrors会通过打开(wikipedia.org gitub.com bitbuket.org)三个网站来验证网络的可用性(幸好github在中国还能用)。

###### pacman-mirrors中使用到的文件介绍

1. /etc/pacman-mirrors.conf pacman-mirrors的配置文件
2. /etc/pacman.d/mirrorlist 供pacman使用的包含servers的文件，实际上整个pacman-mirrors就是为了使用最优的源配置这个文件
3. /usr/share/pacman-mirrors/mirrors.json 全球的镜像源列表从github中下载保持最新版本
4. /var/lib/pacman-mirrors/status.json 下载自repo.manjaro.org用于存储目前全球镜像的状态
5. /var/lib/pacman-mirrors/custom-mirrors.json 用户自己的镜像源池，通过pacman -i/-c来设置和创建


###### pacman-mirrors命令介绍

- pacman-mirrors设置命令
    - -c.--country [国家] 设置用户镜像池中的国家即设置custom-mirrors.json中的country参数
    - -f,--fasttrack 使用最快的源来更新mirrorlist文件(会用到custom-mirrors.json中内容，使用前最好制定国家，要不会比较慢)
    - -S,--set-branch 设置分支 stable testing ubstable 默认stable，其他分支并不稳定
- pacman-mirrors查询命令
    - -l,--country-list 默认的镜像池中的国家列表
    - -lc,--country-config 用户镜像池的国家列表
    - -G,--get-branch 目前所在的分支
- 直接设置mirrorlist文件命令
    - -R,--re-branch 替换mirrorlist中的分支
    - -U,--url [url] 直接替换mirrorlist中的服务器地址

### 使用非官方仓库

以上说的都是官方仓库，还有一些非官方仓库可以共选择。实际上并没有专门针对于manjaro的非官方仓库。但是manjaro继承了Arch的特点，所以也就相应的继承了Arch的非官方仓库。其中比较重要的有两个AUR和Archlinuxcn。而这些仓库并不能使用pacman-mirrors命令来添加，这个命令是manjaro开发出来专门针对于manjaro散发的官方仓库使用的。但是我们可以直接设置pacman.conf来使用这些非官方仓库。

#### 添加Archlinuxcn仓库

1. 在pacman.conf中添加一下语句
```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
2. 安装 archlinuxcn-keyring钥匙环，否则会出现GPG错误。其本身由TU签名。
> pacman -S archlinuxcn-keyring

3. 这样就安装完成了，其实其他任何库都可以使用类似的方法来完成

#### 使用AUR软件仓库

AUR是一个为Arch Linux用户而生的社区驱动软件仓库，类似与Debian的PPA。AUR包含不直接被Arch Linux官方背书的软件。pacman不直接支持AUR，所以我们通常需要下载一个特殊工具——AUR助手程序。yaourt就是这样的程序他对pacman进行了分装便于用户安装AUR中的软件。基本上采用了与pacman一样的语法。 当然还有很多的AUR助手程序以供选择，实际上yaourt是最流行的但是已经多年没有更新了。通常不建议使用Archlinuxcn以及AUR这类的非官方库，因为这些并不是为manjaro构建的，在Arch中用着没有问题并不一定在manjaro中没有问题。但是毕竟是个人使用谁care呢。

## 总结

### 系统安装的第一步

1. 设置软件源国家(即设置custom-mirrors.json)
> sudo pacman-mirrors -c China
2. 设置该国家最快的软件源地址(结果保存到mirrorlist文件中)
> sudo pacman-mirrors -f
3. 添加archlinuxcn源
    1. 编辑/etc/pacman.conf文件添加
    ```
    [archlinuxcn]
    SigLevel = Optional TrustedOnly
    Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
    ```
    2. 安装archlinuxcn-keyring钥匙环
    > sudo pacman -S archlinuxcn-keyring
4. 更新系统
> sudo pacman  -Syyu

### 安装卸载查询软件
1. 安装软件
> sudo pacman -S package_name
2. 卸载软件
> sudo pacman -Ru package_name
3. 查询某个软件
> sudo pacman -Ss [regexp]

## Pamac软件包管理器

实际上Manjaro有自己的软件包管理器即Pamac，我们在manjaro图形界面下打开的软件管理就是Pamac的图形界面版本。其用法与windows中的软家管家类软件差不多。其中比较有意思的是Pamac提供了AUR的支持，默认是关闭的，需要在设置=>AUR中打开AUR支持选项。

## 参见
[manjaro官方Wiki](https://wiki.manjaro.org/index.php?title=Main_Page)  
[arch官方Wiki](https://wiki.archlinux.org/)