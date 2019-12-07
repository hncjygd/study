## 什么是多用户多任务分时操作系统

其中多任务比较容易理解，就是我们在浏览网页的同时可以使用音乐播放器、聊天工具等。这就是多任务的使用场景。  
而多用户如果从字面上理解就是可以有多个用户来操作电脑。但是你会有疑惑平时我们使用的时候没看到什么多用户啊，而且电脑只能链接一个鼠标键盘怎么可能多个人操作电脑。其实这里的多用户是操作系统层面的，也就说不仅仅你可以使用操作系统管理的硬件资源，其他用户也可以远程登录(通过ssh协议等)来使用这台电脑。多个用户在同时工作的时候双方互不影响，这就是多用户。  
分时是指将电脑的时间资源适当分配给所有使用者身上，让每一个使用者都感觉在独占这台机器。这个使用者可以看成具体的用户也可以看成具体的任务。所以一个分时操作系统天生就是支持多任务的。  
目前主流的操作系统例如MacOS、Windows、Linux都是支持多用户多任务的。

## 本篇文章要讲述的内容

1. 什么是用户和组
2. 用户和组的配置文件有哪些
3. 具体如何管理用户和组
4. 管理用户和组时的一些技巧

## 管理用户和组

### 什么是用户和组
对于一个多用户多任务操作系统，多个用户可以在同一时间登录同一个系统执行各自不同的任务，而互不影响，并且每个用户被赋予了不同的权限，在不同的权限范围内可以完成不同的任务。在linux下用户等级分为两种：root和非root。root用户被赋予至高无上的权利可以修改系统下的任何文件(包括其他用户的文件)。而非root用户的权限是严格受限的，只能访问由root规定的文件或属于自己的文件。  
而组是具有相同特征用户的逻辑集合，优势我们需要让多个用户具有相同的权限，比如查看、修改某一个文件，其中一个方法就是给每一个需要的用户这个权限。另一个方法就是建立组为该组赋予权限，并将用户放入该组即可。将用户分组是Linux系统中对用户管理及控制访问权限的一种手段，通过这种手段可以很大程度上简化管理工作。

### 用户与组的关系

- 一对一：一个用户可以存在一个组内，并且是改组的唯一成员
- 一对多：一个用户可以被分配到多个组中，该用户就具有这多个组的共同权限
- 多对一：多个用户可以存在于一个组中

### /etc/passwd文件查看用户
该文件是用户配置文件，是用户管理中最重要的一个文件。从文件名上看，应该是跟密码有关。的确是这样，以前这里就保存了用户的密码。但是由于该文件可以被任何用户查看，所以为了安全将密码移到/etc/shadow文件中了。现在我们需要关心的是passwd文件中内容如下：
```
[hncjygd@hncjygd-pc ~]$ cat /etc/passwd
root:x:0:0::/root:/bin/bash
nobody:x:65534:65534:Nobody:/:/usr/bin/nologin
dbus:x:81:81:System Message Bus:/:/usr/bin/nologin
bin:x:1:1::/:/usr/bin/nologin
daemon:x:2:2::/:/usr/bin/nologin
mail:x:8:12::/var/spool/mail:/usr/bin/nologin
ftp:x:14:11::/srv/ftp:/usr/bin/nologin
http:x:33:33::/srv/http:/usr/bin/nologin
systemd-journal-remote:x:982:982:systemd Journal Remote:/:/usr/bin/nologin
systemd-network:x:981:981:systemd Network Management:/:/usr/bin/nologin
systemd-resolve:x:980:980:systemd Resolver:/:/usr/bin/nologin
systemd-timesync:x:979:979:systemd Time Synchronization:/:/usr/bin/nologin
systemd-coredump:x:978:978:systemd Core Dumper:/:/usr/bin/nologin
uuidd:x:68:68::/:/usr/bin/nologin
dnsmasq:x:977:977:dnsmasq daemon:/:/usr/bin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/usr/bin/nologin
avahi:x:975:975:Avahi mDNS/DNS-SD daemon:/:/usr/bin/nologin
colord:x:974:974:Color management daemon:/var/lib/colord:/usr/bin/nologin
cups:x:209:209:cups helper user:/:/usr/bin/nologin
flatpak:x:973:973:Flatpak system helper:/:/usr/bin/nologin
geoclue:x:972:972:Geoinformation service:/var/lib/geoclue:/usr/bin/nologin
git:x:971:971:git daemon user:/:/usr/bin/git-shell
nm-openconnect:x:970:970:NetworkManager OpenConnect:/:/usr/bin/nologin
nm-openvpn:x:969:969:NetworkManager OpenVPN:/:/usr/bin/nologin
ntp:x:87:87:Network Time Protocol:/var/lib/ntp:/bin/false
polkitd:x:102:102:PolicyKit daemon:/:/usr/bin/nologin
rtkit:x:133:133:RealtimeKit:/proc:/usr/bin/nologin
sddm:x:968:968:Simple Desktop Display Manager:/var/lib/sddm:/usr/bin/nologin
usbmux:x:140:140:usbmux user:/:/usr/bin/nologin
hncjygd:x:1000:1000:hncjygd:/home/hncjygd:/bin/bash
nvidia-persistenced:x:143:143:NVIDIA Persistence Daemon:/:/usr/bin/nologin
tss:x:967:967:tss user for tpm2:/:/usr/bin/nologin
```
文件中的每一行代表一个用户，每一行由冒号分割为7个字段，其结构如下：  
> loginname:password:UID:GID:commentfiled:directory:shell 

- loginname：符合Linux命名规则的字符串，表示用户名
- password：都是x，表示密码保存在/etc/shadow文件中
- UID：用户ID，在manjaro中 root用户ID为0,非root用户的默认UID从1000开始，系统用户在1～999中
- GID：用户初始组ID，具体可查看etc/group文件
- commentfiled：实际上就是一个可选注释字段，通常用来记录用户的全名
- directory：用于登录命令设置$HOME环境变量。某些服务的用户主目录设置为/，但是对于普通用户默认为/home/account
- shell:用户默认登录的shell，通常是/bin/bash,在上可看到系统用户的shell大部分都是/usr/bin/nologin，这其实是一个特殊的shell该shell会阻止你登录并且该shell没有任何功能。

### /etc/shadow文件保存用户密码

该文件用于存储linux系统中用户的密码信息。注意该文件只有root用户才拥有读权限。下面列出了部分shadow文件的内容。
```
[hncjygd@hncjygd-pc etc]$ sudo cat /etc/shadow
[sudo] password for hncjygd: 
root:$6$lWgsBRBy3YgxYmc1$iim.PKBNVk4MpRKOijCtJadFwjy9J/wBKEPQ9Mg9CC3/eP1ZV3O2n5ZPvB25Sbko68WSr.3eee7y1rm1XCXoM0:18216::::::
nobody:!!:18150::::::
dbus:!!:18150::::::
bin:!!:18150::::::
daemon:!!:18150::::::
...
nm-openvpn:!!:18150::::::
ntp:!!:18150::::::
polkitd:!!:18150::::::
rtkit:!!:18150::::::
sddm:!!:18150::::::
usbmux:!!:18150::::::
hncjygd:$6$02RYBAi.L18VljwA$wyTVZxXK4jwzGdyUOqLQRgY9.9LCrToC6gqSHyyMsd5AGkDMcLiyrL.CdBmLSg1PEju44eT07Nd2caphkWd5x/:18216:0:99999:7:::
nvidia-persistenced:!!:18216::::::
tss:!!:18216::::::
```
与passwd文件类似一行代表一个用户，并且使用分号隔开共9个字段，其结构如下：
- 用户名： 与/etc/passwd相同
- 加密密码：目前linux使用SHA512加密密码。一些系统用户的密码是!!,这表是没有密码。
- 最后一次修改时间：表示最后一次修改密码的时间，在linux中计算时间的方法是以1970年1月1日作为1不断累计天数，例如上面的root为18216表示其后的18216天。
- 最小修改时间间隔：该字段规定从最后一次修改时间算起，多长时间不能修改密码，如果为空或0表示随时可以修改，如果是10表示10天后才能修改密码。
- 密码有效期：表示在最后一次修改时间算起多长时间内需要再次变更密码，默认99999想当与273年，可以认为永久生效
- 密码需要变更前的警告天数：当帐号密码有效期快要到时，系统会发出警告信息告诉该帐号需要修改密码
- 密码过期后的宽限天数：如果密码过期后还没有修改，允许的宽限天数，如果宽限天数过了将不再能够登录
- 帐号失效时间：该字段时间也是自1970年1月1日算起，表示该帐号在此字段规定的时间之外无论你密码是否过期都将不能使用，通常被用于具有收费服务的系统中
- 保留：这个字段目前没有用处

### /etc/group查看用户组
该文件定义了组，下面显示了group中部分内容：
```
root:x:0:root
adm:x:999:daemon
wheel:x:998:hncjygd
kmem:x:997:
tty:x:5:
utmp:x:996:
audio:x:995:
disk:x:994:
...
rtkit:x:133:
sddm:x:968:
usbmux:x:140:
hncjygd:x:1000:
bumblebee:x:56:
nvidia-persistenced:x:143:
tss:x:967:
vboxusers:x:108:
```
其与passwd一样，每一行字段代表一个组，其中用冒号隔开表示一个字段。其分为4个部分：
> 组名：密码：GID：该用户组中的用户列表

- 组名：用户组的名称
- 组密码：x表示，具体密码位于/etc/gshadow文件中
- GID：组的ID，Linux系统通过GID识别组，而组名只是为了便于记忆
- 组中的用户：列出了这个组的用户，需要注意的是，如果该用户是这个组的初始组，则该用户不会写入这个字段。多个用户是用逗号隔开的。

### /etc/gshadow用户组密码

该文件存储了组用户的密码信息：
```
root:::root
adm:!!::daemon
wheel:!!::hncjygd
kmem:!!::
tty:!!::
utmp:!!::
audio:!!::
disk:!!::
input:!!::
kvm:!!::
lp:!!::cups,hncjygd
optical:!!::
...
```
其每行代表一个组用户的密码信息：
> 组名：加密密码：组管理员：组中的附加用户

- 组名：同/etc/group
- 加密密码：通常不用组密码，该这段通常为空或!!
- 组管理员：从系统管理员的角度来说，该文件最大的功能就是创建群组管理员，群组管理员的作用是分担root任务，当某个用户想要加入某个群组的时候，root来不及回应的时候可以让群组管理员来管理。就目前来说使用sudo会更加方便,所以组管理员以及组密码也就很少使用了
- 组中的附加用户：同/etc/group

### 管理用户和组
前面已经介绍了，在linux中用户保存在/etc/passwd文件中，用户密码保存在/etc/shadow文件中，组保存在/etc/group文件中，组密码保存在/etc/gshadow文件中。所以我们就可以通过修改这些文件来新建、删除用户和组。但是实际使用中如果自己正对于文件的修改会容易出错，并且还有可能误伤其他用户和组的内容。所以linux提供了一系列的命令用于管理用户和组，这些命令说白了最重要的用处就是对这四个文件进行增删改查。   
在manjaro中这些这些管理工具由shadow包提供(而/etc/passwd /etc/shadow等四个文件由filesystem这个系统包提供)。
```
[hncjygd@hncjygd-pc etc]$ pacman -Ql shadow
shadow /etc/
shadow /etc/default/
shadow /etc/default/useradd
shadow /etc/login.defs
shadow /etc/pam.d/
shadow /etc/pam.d/chage
shadow /etc/pam.d/chgpasswd
shadow /etc/pam.d/chpasswd
shadow /etc/pam.d/groupadd
shadow /etc/pam.d/groupdel
shadow /etc/pam.d/groupmems
shadow /etc/pam.d/groupmod
shadow /etc/pam.d/newusers
shadow /etc/pam.d/passwd
shadow /etc/pam.d/shadow
shadow /etc/pam.d/useradd
shadow /etc/pam.d/userdel
shadow /etc/pam.d/usermod
shadow /usr/
shadow /usr/bin/
shadow /usr/bin/chage
shadow /usr/bin/chgpasswd
shadow /usr/bin/chpasswd
shadow /usr/bin/expiry
shadow /usr/bin/faillog
shadow /usr/bin/gpasswd
shadow /usr/bin/groupadd
shadow /usr/bin/groupdel
shadow /usr/bin/groupmems
shadow /usr/bin/groupmod
shadow /usr/bin/groups
shadow /usr/bin/grpck
shadow /usr/bin/grpconv
shadow /usr/bin/grpunconv
shadow /usr/bin/lastlog
shadow /usr/bin/newgidmap
shadow /usr/bin/newuidmap
shadow /usr/bin/newusers
shadow /usr/bin/passwd
shadow /usr/bin/pwck
shadow /usr/bin/pwconv
shadow /usr/bin/pwunconv
shadow /usr/bin/sg
shadow /usr/bin/useradd
shadow /usr/bin/userdel
shadow /usr/bin/usermod
...
```

#### 用户管理

##### useradd 添加用户

> useradd [选项] 用户名

| 选项 |                                   含义                                   |
| ---- | ------------------------------------------------------------------------ |
| -u   | 手动指定用户的UID                                                        |
| -d   | 手动指定用户的家目录                                                     |
| -c   | 手动指定用户的说明信息                                                   |
| -g   | 手动指定用户的初始组                                                     |
| -G   | 手动指定用户的附加组                                                     |
| -s   | 手动指定用户的shell                                                      |
| -e   | 指定用户的失效日期                                                       |
| -m   | 建立用户时强制建立用户的家目录，所以如果想要用户建立家目录需要带上该参数 |
| -r   | 创建系统用户，及UID在1～999之间供系统使用，该参数不会创建家目录          |

示例：
```
[hncjygd-pc hncjygd]# useradd test
[hncjygd-pc hncjygd]# useradd -u 1100 -c "test user" -g wheel test1
[hncjygd-pc hncjygd]# cat /etc/passwd | grep test
test:x:1001:1001::/home/test:/bin/bash
test1:x:1100:998:test user:/home/test1:/bin/bash
```
我们分别不用参数以及使用参数创建了两个用户，此时你查看passwd和shadow其中确实有两个用户被添加。这是你可能有疑惑没有被添加的参数到底是怎么来的。这就需要知道useradd的默认值了。

###### /etc/default/useradd 用户创建默认值

如果在useradd创建用户没有设置属性值的时候，就需要使用这个文件来指定默认值了。

> useradd -D 可以查看useradd的默认值

```
# useradd defaults file for ArchLinux
# original changes by TomK
GROUP=users
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=no
```
我们来逐一介绍这个文件的内容：  
- GROUP=users:这个选项用于建立用户的默认组，也就是说，在添加每个用户时，用户的初始组都是users这个组。(实际使用中并不是使用这个参数，manjaro会创建一个与该用户同名的初始组，这就牵扯到Linux中两种不同的用户组机制：一种私有用户组机制就是manjaro这种，一种公共用户组机制就是将新创建的组默认归属到一个组内)
- HOME=/home：指定用户家目录的默认位置，所有新建的用户的主目录默认都在/home下
- INACTIVE=-1:指的是密码过期后的宽限天数，也就是shadow文件的第七个字段，默认-1代表永不失效
- EXPIRE=:表示密码失效时间，shadow文件的第八个参数，默认空，表示永久有效
- SHELL=/bin/bash:指定用户的默认shell
- SKEL=/etc/skel：在创建用户后，会发现，该用户的主目录并不是空目录，而是有.bashrc .config等文件，这些文件都是从/etc/skel目录中复制过来的，顾名思义该目录就是为了在用户家目录生成一个程序必须的配置文件的
- CREATE_MAIL_SPOOL=no:指的是给新建用户建立邮箱的，默认不创建。如果为yes会在/var/spool/mail/目录下创建和用户名相同的文件夹

而实际上以上的值都是保存在/etc/default/useradd文件中的，-D选项仅仅就是打印了这个文件中的内容。所以如果你想要更改这个默认设置，直接更改这个文件就可以了，当然为了安全，useradd命令也提供了对这个文件修改的选项。  

> useradd -D [选项]

| 选项 |                  含义                   |
| ---- | --------------------------------------- |
| -b   | 设置HOME= 的内容                        |
| -e   | 设置EXPIRE=的内容参数格式为YYYY-MM-DD   |
| -f   | 设置INACTIVE=的内容，参数为整数         |
| -g   | 设置GROUP=的内容，可以是UID也可以是组名 |
| -s   | 设置SHELL=的内容                        |

> 从这里我们可以初步领略一个Linux的一条真谛：一切皆文件。对于Linux的设置基本上都是通过文本文件进行的，你有两条思路通过文本编辑器直接修改这个文件来实现系统甚至，当然这需要你对Linux有非常深入的理解。这就相当于我们需要设置的什么直接设置程序的源代码一样。另一个思路就是通过Linux提供的命令来设置系统，而这些命令就是对这些配置文本的增删改查来实现的，这些命令就相当与程序提供给我们的API。

#### passwd 修改用户密码
当我们通过useradd命令创建了linux用户后，并没有设置用户的密码。此时我们可以查看下shadow文件：
```
[hncjygd-pc hncjygd]# cat /etc/shadow | grep test
test:!:18223:0:99999:7:::
test1:!:18223:0:99999:7:::
```
可以看到密码域为!,这表示还没有设置密码。此时是无法用该用户登录系统的，必须使用passwd来设置密码后才能登录。

> passwd [选项] 用户名

|  选项  |                                意义                                 |
| ------ | ------------------------------------------------------------------- |
| -S     | 查询用户的密码状态，其实就是列出了shadow中部分内容                  |
| -l     | 锁定用户，该选项会在shadow文件中指定用户的加密密码前添加!使密码失效 |
| -u     | 解锁用户,与-l相对应                                                 |
| -stdin | 可以通过管道符输出的数据作为用户的密码，主要在批量添加用户时使用    |
| -n     | 设置该用户修改密码后，多长时间不能再次修改密码                      |
| -x     | 设置该用户的密码有效期                                              |
| -w     | 设置用户密码过期前的警告天数                                        |
| -i     | 设置用户密码失效日期                                                |

我们通过-S来查看下新建的test和test1的密码状态：
```
[hncjygd-pc hncjygd]# passwd -S test
test L 11/23/2019 0 99999 7 -1
[hncjygd-pc hncjygd]# passwd -S test1
test1 L 11/23/2019 0 99999 7 -1
```
其中的包含7个字段分别对应了shadow的相应字段。其中密码字段由L/NP/P分别表示被锁定/空密码/有密码的状态。可见我们再使用useradd创建完用户后是处于锁定状态的(其密码域的!也说明了这个问题)，而且该锁定状态无法使用-u命令解除。接下来我们来设置密码：
```
[hncjygd-pc hncjygd]# passwd test
New password: 
Retype new password: 
passwd: password updated successfully
[hncjygd-pc hncjygd]# passwd -S test
test P 11/23/2019 0 99999 7 -1
[hncjygd-pc hncjygd]# cat /etc/shadow | grep test
test:$6$RX.WYhmgRGyRLoHC$oD7j0nFSC0zqBw8WX7.m46z75l8wZNPzrWWUOw6l96at/r0f0DN0vapi7OYho.6juklqB.ImGePl/cM7SIQAq1:18223:0:99999:7:::
test1:!:18223:0:99999:7:::
```
可以看到此时我们就为test用户设置了密码，并且其可以登录了。(test用户也是惨，连个家目录都没有 ^-^ 这里就是调侃一下，在manjaro发行版中useradd -m并不是默认值，所以在新建用户的时候一定要记着加上-m，而centos ubuntu这都是默认的啊！！)

##### /etc/login.defs 文件

实际上对于密码也有一些默认的配置，这些配置位于/etc/login.defs文件中，其内容如下
```
#
# /etc/login.defs - Configuration control definitions for the login package.
#
# Three items must be defined:  MAIL_DIR, ENV_SUPATH, and ENV_PATH.
# If unspecified, some arbitrary (and possibly incorrect) value will
# be assumed.  All other items are optional - if not specified then
# the described action or option will be inhibited.
#
# Comment lines (lines beginning with "#") and blank lines are ignored.
#
# Modified for Linux.  --marekm

#
# Delay in seconds before being allowed another attempt after a login failure
#
FAIL_DELAY              3

#
# Enable display of unknown usernames when login failures are recorded.
#
LOG_UNKFAIL_ENAB        no

#
# Enable logging of successful logins
#
LOG_OK_LOGINS           no

#
# Enable "syslog" logging of su activity - in addition to sulog file logging.
# SYSLOG_SG_ENAB does the same for newgrp and sg.
#
SYSLOG_SU_ENAB          yes
SYSLOG_SG_ENAB          yes

#
# If defined, either full pathname of a file containing device names or
# a ":" delimited list of device names.  Root logins will be allowed only
# upon these devices.
#
CONSOLE         /etc/securetty
#CONSOLE        console:tty01:tty02:tty03:tty04

#
# If defined, all su activity is logged to this file.
#
#SULOG_FILE     /var/log/sulog

#
# If defined, file which maps tty line to TERM environment parameter.
# Each line of the file is in a format something like "vt100  tty01".
#
#TTYTYPE_FILE   /etc/ttytype

#
# If defined, the command name to display when running "su -".  For
# example, if this is defined as "su" then a "ps" will display the
# command is "-su".  If not defined, then "ps" would display the
# name of the shell actually being run, e.g. something like "-sh".
#
SU_NAME         su

#
# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR      Maildir
MAIL_DIR        /var/spool/mail

#
# If defined, file which inhibits all the usual chatter during the login
# sequence.  If a full pathname, then hushed mode will be enabled if the
# user's name or shell are found in the file.  If not a full pathname, then
# hushed mode will be enabled if the file exists in the user's home directory.
#
HUSHLOGIN_FILE  .hushlogin
#HUSHLOGIN_FILE /etc/hushlogins

#
# *REQUIRED*  The default PATH settings, for superuser and normal users.
#
# (they are minimal, add the rest in the shell startup files)
ENV_SUPATH      PATH=/usr/local/sbin:/usr/local/bin:/usr/bin
ENV_PATH        PATH=/usr/local/sbin:/usr/local/bin:/usr/bin

#
# Terminal permissions
#
#       TTYGROUP        Login tty will be assigned this group ownership.
#       TTYPERM         Login tty will be set to this permission.
#
# If you have a "write" program which is "setgid" to a special group
# which owns the terminals, define TTYGROUP to the group number and
# TTYPERM to 0620.  Otherwise leave TTYGROUP commented out and assign
# TTYPERM to either 622 or 600.
#
TTYGROUP        tty
TTYPERM         0600

#
# Login configuration initializations:
#
#       ERASECHAR       Terminal ERASE character ('\010' = backspace).
#       KILLCHAR        Terminal KILL character ('\025' = CTRL/U).
#       UMASK           Default "umask" value.
#
# The ERASECHAR and KILLCHAR are used only on System V machines.
# The ULIMIT is used only if the system supports it.
# (now it works with setrlimit too; ulimit is in 512-byte units)
#
# Prefix these values with "0" to get octal, "0x" to get hexadecimal.
#
ERASECHAR       0177
KILLCHAR        025
UMASK           077

#
# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                  1000
UID_MAX                 60000
# System accounts
SYS_UID_MIN               500
SYS_UID_MAX               999

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                  1000
GID_MAX                 60000
# System accounts
SYS_GID_MIN               500
SYS_GID_MAX               999

#
# Max number of login retries if password is bad
#
LOGIN_RETRIES           5

#
# Max time in seconds for login
#
LOGIN_TIMEOUT           60

#
# Which fields may be changed by regular users using chfn - use
# any combination of letters "frwh" (full name, room number, work
# phone, home phone).  If not defined, no changes are allowed.
# For backward compatibility, "yes" = "rwh" and "no" = "frwh".
# 
CHFN_RESTRICT           rwh

#
# List of groups to add to the user's supplementary group set
# when logging in on the console (as determined by the CONSOLE
# setting).  Default is none.
#
# Use with caution - it is possible for users to gain permanent
# access to these groups, even when not logged in on the console.
# How to do it is left as an exercise for the reader...
#
#CONSOLE_GROUPS         floppy:audio:cdrom

#
# Should login be allowed if we can't cd to the home directory?
# Default in no.
#
DEFAULT_HOME    yes

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD    /usr/sbin/userdel_local

#
# Enable setting of the umask group bits to be the same as owner bits
# (examples: 022 -> 002, 077 -> 007) for non-root users, if the uid is
# the same as gid, and username is the same as the primary group name.
#
# This also enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

#
# Controls display of the motd file. This is better handled by pam_motd.so
# so the declaration here is empty is suppress display by readers of this
# file.
#
MOTD_FILE

#
# Hash shadow passwords with SHA512.
#
ENCRYPT_METHOD  SHA512
```
其中包含了非常多的设置项目，其中牵扯到我们用户管理的有以下的内容：

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

实际上这个配置文件是shadow软件包的配置文件，目前来说已经很少使用到了，现在大部分发行版都是通过PAM这个系统级用户认证框架来进行认证服务了。但是其中的一些默认参数还是需要了解的。

#### usermod 修改用户信息

当你通过useradd命令添加用户后，如果此时不小心添加错用户信息了，或者对默认的用户信息不满意时，可以通过usermod命令来修改这些信息。  

> usermod [选项] 用户名

| 选项 |             说明              |
| ---- | ----------------------------- |
| -c   | 修改用户说明                  |
| -d   | 修改用户主目录                |
| -e   | 修改用户的失效日期 YYYY-MM-DD |
| -g   | 修改用户的初始组              |
| -u   | 修改用户的UID                 |
| -G   | 修改用户的附加组              |
| -l   | 修改用户的用户名              |
| -L   | 临时锁定用户                  |
| -U   | 解锁用户                      |
| -s   | 修改用户的登录shell           |

注意，修改用户名的时候尤其需要注意，有些程序配置文件中使用到了用户名，尤其是已经用过一段时间的用户修改用户名可能会导致一些程序无法使用。

#### chage 修改用户密码状态
其实就是在passwd后用来修改shadow文件的命令。

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

#### userdel 删除用户

> userdel [-r] 用户名

- 删除用户实际上就是删除其在passwd shadow group gshodow四个文件中存在的痕迹
- -r的意义：删除用户的同时删除其家目录以及邮箱
- 如果你要删除的用户已经使用系统一段时间了，那么用户可能在系统的其他位置留有其他文件，仅仅使用userdel命令是无法彻底删除用户的使用痕迹的，这时可以使用find --user 用户名命令来查出哪些文件属于该用户一并删除即可。

#### 总结
首先让我们来自己手动创建一个用户：
1. 修改passwd文件添加一行：test2:x:1200:998:add user myself:/home/test2:/bin/bash
2. 修改shadow文件添加一行：test2:!:18233:0:99999:7:::
3. 创建/home/test2/文件夹
4. 将/etc/skel/目录下的内容复制到/home/test2/文件夹内
5. 如果想要邮箱可以在/var/spool/mail/文件夹下创建test2文件夹
6. 此时我们就手动创建好了一个用户，删除过程同理

> 本节介绍的每一个命令都有非常多的选项，这并不需要全部记住，之所以全部罗列了出来是想让你在其中深刻领悟到Linux一切皆文件的精髓。对于本节我们只需要知道passwd的字段构成、shadow的字段构成就好了，具体我们需要修改或则新建什么样的用户到时候man一下就行了。

#### 用户组管理

用户组与用户管理非常类似，其也有存储用户组的文件/etc/group 以及存储组密码的文件/etc/gshadow。对用户组的修改也就是修改这两个文件的内容，linux同样提供了一系列的命令来对这两个文件进行增删改查。  
我们这里需要想介绍一个概念：用户初始组和附加组。
- 初始组：就是出现在passwd中的UID，每个用户一定有一个且唯一一个初始组
- 附加组：group文件后面罗列的就是用户的附加组，每个用户可以有多个附加组

- groupadd [选项] 组名  #添加用户组
    - -g 指定组ID
- groupmod [选项] 组名  #修改用户组
    - -g 修改组ID
    - -n 修改组名
- groupdel 组名     #删除组
    - 其本质是删除/etc/group /etc/gshadow中组想对应的行。此命令仅仅适用与删除不是任何用户初始组的群组，否则无法成功。除非使用usermod -g更改用户的初始组，或删除用户。
- gpasswd [选项] 组名   #将用户加入群组或从组中删除
    - -A user1... 将群组的控制权交给user1..等用户管理，即设置他们为用户群组的管理员，仅root用户可用
    - -M user1... 将user1..等用户加入到此群组中，仅root用户可用
    - -r 移除群组的密码，仅root用户可用
    - -R 让群组的密码失效，仅root用户可用
    - -a user 将user用户加入到群组
    - -d user 将user用户从群组中移除
- newgrp 用户名 #临时切换用户的初始组
    - 只能从附加组中选择一个组并切换为初始组
    - 这种切换是临时的，即关闭终端后就不在生效了
    - 如果想要永久切换，使用usermod -g命令修改

### 其他相关命令

#### id 查询用户的UID和GID

```
[hncjygd-pc hncjygd]# id test1
uid=1100(test1) gid=998(wheel) groups=998(wheel)
[hncjygd-pc hncjygd]# id test
uid=1001(test) gid=1001(test) groups=1001(test)
[hncjygd-pc hncjygd]# id hncjygd
uid=1000(hncjygd) gid=1000(hncjygd) groups=1000(hncjygd),998(wheel),991(lp),3(sys),90(network),98(power)
```
可以看到其中列出了UID GID 以及 GROUPS 这个用户组列表。

#### su 临时切换用户命令

su是最简单的用户切换命令，通过该命令可以实现任何身份的切换。  
需要注意的是：普通用户到任何用户切换都需要知晓对方的密码，而root用户到任何用户的切换都不需要对方密码而直接切换成功

> su [选项] 用户名

| 选项 |                                                                           含义                                                                           |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -    | 当前用户不仅切换为指定用户的身份，同时所用的工作环境也切换为此用户的环境（包括 PATH 变量、MAIL 变量等），使用 - 选项可省略用户名，默认会切换为 root 用户 |
| -l   | 同 - 的使用类似，也就是在切换用户身份的同时，完整切换工作环境，但后 面需要添加欲切换的使用者账号                                                          |
| -p   | 表示切换为指定用户的身份，但不改变当前的工作环境（不使用切换用户的配置文件）                                                                             |
| -m   | 和 -p 一样                                                                                                                                               |
| -c   | 仅切换用户执行一次命令，执行后自动切换回来，该选项后通常会带有要执行的命令                                                                               |

#### whoami和who am i命令用于打印用户名时的区别

whoami:用于打印当前执行操作的用户名  
who am i:用于打印登陆当前linux系统的用户名  

加入我们使用test登陆了linux系统，在终端中使用su切换到root命令。此时执行whoami显示的就是root。而使用who am i显示的则是：test

对于：whoami等价于 id -un 命令 而who am i 等价于 who -m 命令。 

### sudo命令详解
linux的用户除了root就是普通用户，而普通用户的权限是非常低的，甚至连安装软件的权利都没有。很多时候系统管理员为了能让普通用户具备一些root用户的特权，就需要赋予他们相应的权限。使用su命令是一个方法，但是该命令需要告诉使用人管理员密码，但是root帐号太过强大，告诉其他人管理员密码太过危险。此时可以通过赋予用户sudo权限来实现。  
但是并非任何普通用户都拥有sudo权限，对于一些面向桌面的发行版，比如manjaro就是，他们的设计理念就是日常应用，比较偏向赋予普通用户相对较为宽泛的权利，所以在系统的配置中，会给 一类基于普通用户的管理员角色，这类角色被赋予了sudo特权。但是对于CentoOS等这些主要面向服务器使用强调是安全可靠，所以在系统的配置中，除非root用户指定，否则不会让普通用户具有sudo特权。

- sudo [选项] 要执行的命令
    - -b 将命令放到后台自行，不对当前shell环境产生影响
    - -l 用于显示当前用户可以用sudo执行哪些命令

#### 如何让用户具有sudo权限

根据linux的尿性，一定是修改某个配置文件，没错这个文件就是/etc/sudoers 文件。(实际上manjaro中sudo还会解析/etc/sudoers.d/目录中的文件，这个目录中的文件按字母顺序被加载.或～开头的文件被跳过)让我们来看看sudoers文件的内容：

```
## sudoers file.
##
## This file MUST be edited with the 'visudo' command as root.
## Failure to use 'visudo' may result in syntax or file permission errors
## that prevent sudo from running.
##
## See the sudoers man page for the details on how to write a sudoers file.
##

##
## Host alias specification
##
## Groups of machines. These may include host names (optionally with wildcards),
## IP addresses, network numbers or netgroups.
# Host_Alias    WEBSERVERS = www1, www2, www3

##
## User alias specification
##
## Groups of users.  These may consist of user names, uids, Unix groups,
## or netgroups.
# User_Alias    ADMINS = millert, dowdy, mikef

##
## Cmnd alias specification
##
## Groups of commands.  Often used to group related commands together.
# Cmnd_Alias    PROCESSES = /usr/bin/nice, /bin/kill, /usr/bin/renice, \
#                           /usr/bin/pkill, /usr/bin/top
# Cmnd_Alias    REBOOT = /sbin/halt, /sbin/reboot, /sbin/poweroff

##
## Defaults specification
##
## You may wish to keep some of the following environment variables
## when running commands via sudo.
##
## Locale settings
# Defaults env_keep += "LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET"
##
## Run X applications through sudo; HOME is used to find the
## .Xauthority file.  Note that other programs use HOME to find   
## configuration files and this may lead to privilege escalation!
# Defaults env_keep += "HOME"
##
## X11 resource path settings
# Defaults env_keep += "XAPPLRESDIR XFILESEARCHPATH XUSERFILESEARCHPATH"
##
## Desktop path settings
# Defaults env_keep += "QTDIR KDEDIR"
##
## Allow sudo-run commands to inherit the callers' ConsoleKit session
# Defaults env_keep += "XDG_SESSION_COOKIE"
##
## Uncomment to enable special input methods.  Care should be taken as
## this may allow users to subvert the command being run via sudo.
# Defaults env_keep += "XMODIFIERS GTK_IM_MODULE QT_IM_MODULE QT_IM_SWITCHER"
##
## Uncomment to use a hard-coded PATH instead of the user's to find commands
# Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
##
## Uncomment to send mail if the user does not enter the correct password.
# Defaults mail_badpass
##
## Uncomment to enable logging of a command's output, except for
## sudoreplay and reboot.  Use sudoreplay to play back logged sessions.
# Defaults log_output
# Defaults!/usr/bin/sudoreplay !log_output
# Defaults!/usr/local/bin/sudoreplay !log_output
# Defaults!REBOOT !log_output

##
## Runas alias specification
##

##
## User privilege specification
##
root ALL=(ALL) ALL

## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL) ALL

## Same thing without a password
# %wheel ALL=(ALL) NOPASSWD: ALL

## Uncomment to allow members of group sudo to execute any command
# %sudo ALL=(ALL) ALL

## Uncomment to allow any user to run sudo if they know the password
## of the user they are running the command as (root by default).
# Defaults targetpw  # Ask for the password of the target user
# ALL ALL=(ALL) ALL  # WARNING: only use this together with 'Defaults targetpw'

## Read drop-in files from /etc/sudoers.d
## (the '#' here does not indicate a comment)
#includedir /etc/sudoers.d
```
在我的manjaro中/etc/sudoers.d/目录下有一个10-installer文件，其内容如下：
```
%wheel ALL=(ALL) ALL
```
整合起来去掉sudoers文件的注释内容如下：
```
root ALL=(ALL) ALL
%wheel ALL=(ALL) ALL
```
要修改/etc/sudoers中的内容，不建议直接使用vim，包括/etc/sudoers中也明确要求使用visudo命令来修改sudoers文件，其实visudo的实际操作于vim一样，但是使用visudo的好处在于修改完毕后在离开时系统会自动检验/etc/sudoers文件的语法正确性。  
下面罗列了一些配置方法，其他配置可以man sudoers来查看：

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

根据上面我们可以知道，在manjaro中root以及wheel群组被赋予sudo权限。所以我们要想在manjaro中让普通用户拥有sudo权限将它加入wheel群组就可以了。当然也可以在创建的时候制定-g参数来实现。而实际上manjaro也是这么做的。我们通过id命令来看hncjygd这个在安装时指定的帐号，就是被加入了wheel组中才具备sudo权限的。