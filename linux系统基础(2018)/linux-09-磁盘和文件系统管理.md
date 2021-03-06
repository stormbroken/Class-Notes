<!-- TOC -->

- [1. linux磁盘分区](#1-linux磁盘分区)
    - [1.1. 磁盘分区知识](#11-磁盘分区知识)
        - [1.1.1. 什么是硬盘分区](#111-什么是硬盘分区)
        - [1.1.2. 分区类型](#112-分区类型)
        - [1.1.3. 分区和格式化](#113-分区和格式化)
    - [1.2. 使用fdisk进行磁盘分区](#12-使用fdisk进行磁盘分区)
        - [1.2.1. fdisk命令的介绍](#121-fdisk命令的介绍)
        - [1.2.2. linux系统下硬盘分区举例](#122-linux系统下硬盘分区举例)
- [2. linux文件系统简介](#2-linux文件系统简介)
    - [2.1. linux文件系统的工作原理](#21-linux文件系统的工作原理)
    - [2.2. linux主流文件系统](#22-linux主流文件系统)
    - [2.3. 查看文件类型](#23-查看文件类型)
- [3. 创建文件系统](#3-创建文件系统)
    - [3.1. 创建文件系统简介](#31-创建文件系统简介)
    - [3.2. 创建文件系统](#32-创建文件系统)
        - [3.2.1. 使用mkfs命令创建文件系统](#321-使用mkfs命令创建文件系统)
        - [3.2.2. 使用其他命令创建文件系统](#322-使用其他命令创建文件系统)
- [4. 挂载和写在文件系统](#4-挂载和写在文件系统)
    - [4.1. 挂载文件系统](#41-挂载文件系统)
        - [4.1.1. 挂载硬盘](#411-挂载硬盘)
        - [4.1.2. 挂载光盘、软盘、U盘](#412-挂载光盘软盘u盘)
    - [4.2. 卸载文件系统](#42-卸载文件系统)
        - [4.2.1. 卸载硬盘](#421-卸载硬盘)
        - [4.2.2. 卸载光盘、软盘、U盘](#422-卸载光盘软盘u盘)
    - [4.3. 查看分区挂载情况](#43-查看分区挂载情况)
        - [4.3.1. 使用mount -s命令](#431-使用mount--s命令)
        - [4.3.2. 查看/etc/mtab文件](#432-查看etcmtab文件)
- [5. 设置开机自动挂载文件系统](#5-设置开机自动挂载文件系统)
    - [5.1. /etc/fstab文件介绍](#51-etcfstab文件介绍)
    - [5.2. /etc/fstab文件详解](#52-etcfstab文件详解)
        - [5.2.1. 设备和默认挂载目录](#521-设备和默认挂载目录)
        - [5.2.2. 文件系统格式](#522-文件系统格式)
        - [5.2.3. 挂载选项](#523-挂载选项)
        - [5.2.4. 转储和文件系统检查选项](#524-转储和文件系统检查选项)
- [6. 使用交换空间](#6-使用交换空间)
    - [6.1. 添加交换空间](#61-添加交换空间)
        - [6.1.1. 添加交换分区](#611-添加交换分区)
        - [6.1.2. 删除交换空间](#612-删除交换空间)
- [7. 权限设置](#7-权限设置)
    - [7.1. 文件和目录的权限](#71-文件和目录的权限)
    - [7.2. 权限设置](#72-权限设置)
        - [7.2.1. 文件管理器更改权限](#721-文件管理器更改权限)
        - [7.2.2. 文字设定法](#722-文字设定法)
        - [7.2.3. 数字设定法](#723-数字设定法)
    - [7.3. 更改文件和目录的所有权](#73-更改文件和目录的所有权)
        - [7.3.1. chown命令](#731-chown命令)
        - [7.3.2. chgrp命令](#732-chgrp命令)

<!-- /TOC -->
**磁盘和文件系统管理**

# 1. linux磁盘分区

## 1.1. 磁盘分区知识
### 1.1.1. 什么是硬盘分区
1. 分区就是硬盘的“段落”，如果需要在计算机上安装多个操作系统，将需要更多的分区。
2. 为了避免在一个分区下有相同的系统目录，也将他们安装在不同的分区。
3. 在linux系统下，它本身就有更多的分支，比如跟分区"/"和交换分区"SWAP"。

### 1.1.2. 分区类型
1. 磁盘分区类型:
    1. 主分区
    2. 扩展分区:这个只是逻辑分区的"容器"
    3. 逻辑分区
2. 一块硬盘最多只能有4个主分区，可以另外建立一个扩展分区来替代4个主分区的其中一个，然后在扩展分区下可以建立更多的逻辑分区。

### 1.1.3. 分区和格式化
1. linux系统分区的工具是fdisk。
2. 每个主分区和逻辑分区都会被存储为一个识别文件系统的附加信息。
3. 通过分区不能产生任何文件系统，分区只是对硬盘上的磁盘空间进行了保留，还不能直接使用，在此分区后必须进行格式化。linux系统下大多使用mkfs命令来进行格式化。

## 1.2. 使用fdisk进行磁盘分区

### 1.2.1. fdisk命令的介绍
1. 命令介绍:使用fdisk命令可以对磁盘进行分区。
2. 命令语法:
    + `fdisk [-b 磁盘分区][-uv][磁盘设备名]`
    + `fdisk [-l][-b <分区大小>][-uv][磁盘设备名]`
    + `fdisk [-s <分区编号>]`
3. 各参数含义:
    1. -b:指定每个分区的大小。
    2. -l:列出指定磁盘的分区表信息。
    3. -s:将指定的区块大小输出到标准输出上，单位为区块。
    4. -u:用分区书代表柱面数目，表示每个分区的起始地址。
    5. -v:显示版本信息。
4. 子命令:
    1. m:显示所有在fdisk使用的命令
    2. p:显示磁盘分区信息
        1. Device Boot(引导):表示引导分区
        2. Start(开始):表示一个分区从哪个柱面开始
        3. End(结束):表示一个分区到哪个柱面结束
        4. Id和System:确认分区类型
        5. Blocks(容量):表示分区容量，其单位是KB，一个分区容量公式是:Blocks = (相应分区End数值-相应分区Start数值)*单位柱面的容量
    3. a:设置磁盘启动区
    4. n:创建新的分区
    5. e:创建扩展分区
    6. p:创建主分区
    7. t:更改分区文件系统
    8. d:删除磁盘分区
    9. q:退出fdisk,不保存硬盘分区设置
    10. w:保存硬盘分区设置并退出fdisk

### 1.2.2. linux系统下硬盘分区举例
1. 进入fdisk界面:`fdisk /dev/sda`
2. 然后输入子命令即可

# 2. linux文件系统简介
1. 文件系统通过为每个文件分配文件块的方式把数据存储在存储设备上，这就要维护每一个文件夹的文件块的分配信息。

## 2.1. linux文件系统的工作原理
1. 文件系统的分配策略:
    1. 块分配:当文件变大的时候每一次都为这个文件分配磁盘空间。
    2. 扩展分配:某个文件的磁盘空间不够的时候，一次性为它分配一连串字符串。
2. 传统UNIX文件系统使用的块分配的机制提供了文件块的分配策略。
    + 当一个文件慢慢变大的时候，就会造成文件中文件块的不连续，从而导致过多的磁盘寻道时间。
    + 通过优化文件块的分配策略来避免文件夹的随机分配。同样问题是:如果整个文件系统的文件块的分配形成碎片的时候，就无法进行连续分配了。
3. 每一次当文件扩展的时候，块分配算法就要写入关于新分配的块所在位置的信息。

## 2.2. linux主流文件系统
1. 文件系统是指文件在硬盘上的存储方式和排列顺序。
    1. linux系统最重要的特征就是支持多种文件系统。
    2. linux操作系统因此可以和很多的操作系统兼容。
2. 虚拟文件系统:
    1. 系统将linux文件系统的所有细节进行了转换，所以linux内核的其他部分及系统中运行的程序将能看到统一的文件系统。
    2. 这个文件系统是为linux用户提供快速且高效的文件访问服务设计的。
3. linux系统常用的文件系统:
    1. ext(扩展文件系统):第一个专门为linux系统设计的文件系统类型。
    2. ext2(二级扩展文件系统):是linux文件系统中使用最多格式，在速度和CPU利用率上较为突出。
        + 得益于快取层的优良设计。
        + 支持256字节的长文件名。
        + 文件写入的时候没有写入文件的meta-data:先写内容，再写meta-data
    3. ext3(日志式文件系统):
        1. 日志式文件系统:由于数据库操作的出现，通过日志数据结构来记录完整的操作条目(文件系统驱动程序写入)
        2. 优越性:文件系统都有快取层参与动作
        3. 最大的缺点:没有现代文件系统所具有的、能提高文件数据处理速度和解压的高效性。
    4. JFS(提供日志的字节级文件系统):
        1. 目的:为了满足服务器的高吞吐量和可靠性需求而设计开发的。
        2. 突出优点:快速重启能力。
        3. 缺点:系统性能上会有一定损失，系统资源占用率高。
    5. ReiserFS:
        1. 使用了特殊的、优化的平衡树来组织所有的文件系统数据，为自身提供了非常好的性能改进，减轻文件系统设计上的认为约束。
        2. 其在设计上着眼实现未来的插件程序。
        3. 缺点:
            1. 每升级一个版本需要将磁盘重新格式化一次。
            2. 不支持长字节的文件目录，易导致系统挂起。
    6. XFS(日志文件系统):
        1. 全64位、快速、稳固的日志文件系统。
    7. Minix:linux支持的第一个文件系统，性能低下。
    8. Xia:Minix的修正版本，一定程度上解决了文件名和文件系统大小的局限。
    9. ISO9660:不爱这个阅兵CDROM文件系统，是通用的Rock Ridge增强系统，允许长文件名。
    10. NFS:sun公司的网络文件系统，支持多个电脑一个文件系统。
    11. SysV:是System V/Coherent在linux平台上的文件系统。
    12. VFAT:是Windows 9x和Windows NT/2000使用的DOS文件系统，支持长文件名。

## 2.3. 查看文件类型
1. 使用root账号登录后，使用命令`ls -l /lib/modules/2.6.21-2950.fc8xen/kernel/fs/`进行查看。

# 3. 创建文件系统

## 3.1. 创建文件系统简介
如果需要使用某个文件系统存放数据，一般要经过以下操作步骤:
1. 使用fdisk命令在磁盘上创建分区。
2. 使用mkfs命令下分区上创建文件系统。
3. 使用mount命令挂载文件系统，或修改/etc/fstab文件使得开机自动挂载文件系统。
4. 使用umount命令写在文件系统。

## 3.2. 创建文件系统

### 3.2.1. 使用mkfs命令创建文件系统
1. 命令语法:`mkfs -t [文件系统类型][磁盘设备名]`
2. 选项的含义:
    1. -l:指定要建立哪一种文件系统
3. 例子:
    1. 查看当前磁盘上的分区情况，该磁盘设备是sda。
        + `fdisk -l /dev/sda`
    2. 格式化/dev/sda5分区
        + `mkfs -t ext3(不同文件系统) /dev/sda5`

### 3.2.2. 使用其他命令创建文件系统
1. 使用mkfs.ext3,mkfs.ext2,mke2fs,mkdosfs,mkfs.msdos和mkfs.vfat命令,用于格式化成相应的文件系统。
2. 例子:`mkfs.ext3 /dev/sda5`

# 4. 挂载和写在文件系统
1. 使用mount和umount命令可以实现挂载和卸载功能。

## 4.1. 挂载文件系统
1. 使用mount命令将某个分区、光盘、软盘或U盘挂载到linux系统的目录下。
2. 命令语法:`mount [-参数][设备名][挂载点]`
3. 参数含义:
    1. -t:指定设备的文件系统类型。
    2. auto:自动检测文件系统。
    3. -o:指定挂载文件系统时的选项
        1. codepage:代码页
        2. iocharset:字符集
        3. ro:以只读方式挂载
        4. rw:以读写方式挂载
        5. nouser:使一般用户无法挂载
        6. user:可以让一般用户挂载设备
### 4.1.1. 挂载硬盘
1. 挂载分区/dev/sda5到/mut/kk目录中。
    1. `mkdir /mnt/kk`
    2. `mount /dev/sda5 mnt/kk`

### 4.1.2. 挂载光盘、软盘、U盘
1. linux系统使用光盘、软盘、U盘以及移动硬盘时，必须下执行挂载命令。
2. 挂载光盘到/media/cdrom目录下:
    1. `mkdir /media/cdrom`
    2. `mount -t iso9660 /dev/cdrom /media/cdrom`
    3. `ls /media/cdrom|more`查看挂载的光盘中的内容

## 4.2. 卸载文件系统
1. 使用umount命令可以将某一个分区、光盘、软盘或是U盘进行卸载
2. 语法命令:`umount [选项][-t <文件系统类型>][文件系统]`
3. 参数含义:
    1. -a:卸载/etc/mtab中记录的所有文件系统
    2. -n:卸载时不要将信息存入/etc/mtab文件中
    3. -r:若无法成功卸载，则尝试以只读的方式重新挂入文件系统
    4. -t:仅卸载选项中所指定的文件系统。

### 4.2.1. 卸载硬盘
1. 卸载分区/dev/sda5文件系统
2. 例子:
    1. `umount /dev/sda5`(方式一)
    2. `df`
    3. `umount /mut/kk`(方式二)

### 4.2.2. 卸载光盘、软盘、U盘
1. 方式一:`umount /dev/cdrom`
2. 方式二:`umount /media/cdrom`

## 4.3. 查看分区挂载情况

### 4.3.1. 使用mount -s命令
1. 使用mount命令查看挂载情况。
2. 命令语法:`mount -s`

### 4.3.2. 查看/etc/mtab文件
1. 语法:`cat /etc/mtab`

# 5. 设置开机自动挂载文件系统

## 5.1. /etc/fstab文件介绍
1. /etc/mtab文件是一个配置文件，它包含了所有分区以及存储设备的信息。其中包含了磁盘分区和存储设备如何挂载，以及挂载在什么地方的信息。
2. 想要编辑这个文件需要root用户权限。

## 5.2. /etc/fstab文件详解
1. 文件基本内容格式:
    1. 一个行包括一个设备或分区的信息，每一行又有多个列的信息。
    2. 第一列:设备名
    3. 第二列:挂载点
    4. 第三列:文件系统格式
    5. 第四列:挂载参数
    6. 第五列:转存选项
    7. 第六列:文件系统检查选项
2. /etc/fstab文件的具体内容
### 5.2.1. 设备和默认挂载目录
1. 一般选择/mnt目录下创建挂载点

### 5.2.2. 文件系统格式
1. 第三行

### 5.2.3. 挂载选项
1. auto和noauto:
    1. auto:系统启动时自动挂载
    2. noauto:相反
2. user和nouser:
    1. user选项允许一般用户挂载设备
    2. noauto选项只允许root用户挂载设备，并且为默认选项，主要放置新用户的越权行为。
3. exec和noexec:
    1. exec选项允许执行被设为exec分区上的二进制文件，默认选项。
    2. noexec选项不允许如此做。
4. ro:
    1. 只读方式挂载文件系统
5. rw:
    1. 只写方式挂载文件系统
6. sync和async:
    1. sync:指I/O会同时进行
    2. async:指I/O操作会异步进行
7. default:
    1. 使用此选项与使用rw、suid、exec、dev、auto、nouser、async选项是一样的功能。

### 5.2.4. 转储和文件系统检查选项
1. 第五列:dump选项，用一个数字来决定该文件系统是否需要备份。
    + 如果是0，则dump系统会忽略该文件系统的备份
2. 第六列:fsck选项，通过检查第6列的数字来决定什么顺序来检查文件系统
    + 如果是0，则不检查文件系统

# 6. 使用交换空间
1. linux系统的交换空间在物理内存被用完时使用
2. 如果在内存已满的时候，系统需要内存，则内存中不活跃的页被转移到交换空间中。
3. 交换空间总的大小至少为计算机内存的1-2倍左右，但是最好内存不要超过2GB容量。

## 6.1. 添加交换空间

### 6.1.1. 添加交换分区
1. 添加一个交换分区的具体步骤有:
    1. 创建交换分区:`mkswap /dev/sda5`
        + 不可以用free进行查看
    2. 启动交换分区:`swapon /dev/sda5`
        + 可以用free进行查看
    3. 确认已经启动交换分区:`cat /proc/swaps`
    4. 如果要在系统引导时启动交换分区，编辑/etc/fstab添加内容。然后在系统下次引导时，就会启动新建的交换分区。
        + `/dev/sda5 swap swao defaults 0 0`
2. 添加交换文件
    1. 创建文件/swapfile:
        + 将大小乘以1024来判断块的大小
        + 在Shell提示下以root用户身份输入一下命令:`dd if=/dev/zero of=/swapfile bs = 1024 count =65536`
    2. 创建交换文件:
        + `mkswap /swapfile`
    3. 启用交换文件:
        + `swapon /swapfile`
    4. 新添加交换分区并启动文件之后，使用如下命令确保交换文件已被启用了
        + `free`
        + `cat /proc/swaps`
    5. 如果在系统引导时启用交换文件，编辑/etc/fstab文件添加如下内容。然后在系统下次引导时，就会启用新建的交换文件。
        + `/dev/sda5 swap swao defaults 0 0`

### 6.1.2. 删除交换空间
1. 删除交换分区:
    1. 在Shell提示下以root用户身份输入一下命令禁用交换分区
        + `swapoff /dev/sda5`
        + `free`
    2. 如果在系统引导时不启用交换分区，编辑/etc/fstab文件删除以下内容
        + `/dev/sda5 swap swao defaults 0 0`
2. 删除交换文件:
    1. 在Shell提示下，以root用户身份执行一下命令来禁用交换文件
        + `swapoff /swapfile`
        + `free`
    2. 删除/swapfile文件
        + `rm -rf /swapfile`
    3. 如果在系统引导时不启用交换分区，编辑/etc/fstab文件删除以下内容
        + `/dev/sda5 swap swao defaults 0 0`

# 7. 权限设置

## 7.1. 文件和目录的权限
1. 文件权限简介:
    1. 设定权限限制允许一下三种用户访问:
        1. 文件的所有者(文件属主)
        2. 文件所有者所在的同组用户
        3. 系统中的其他用户
    2. linux系统中，对每一个用户对文件或目录的读取、写入和执行权限。
        1. 第一套:(所有者权限):控制访问自己的文件权限。
        2. 第二套:控制用户组访问其他一个用户的文件的权限。
        3. 第三套:赋予用户不同类型的读取、写入及执行权限
    3. 所以有9种类型的权限组
    4. 用户可以控制一个给定文件和目录的访问程度:
        1. 创建一个文件，系统会自动地赋予文件所有者读和写的权限
        2. 文件所有者可以吧权限改为任意权限
        3. 一个文件可以有只读权限，禁止任何修改。也可以有只写权限，允许他像一个程序一样执行
2. 一般权限:
    1. 使用"ls -l"命令可以显示文件的详细信息。
    2. 使用"ls -l"和"ls -al"命令后显示的结果。
    3. 第一个字符用来区分文件的类型，具体参见第五章字母。
    4. 最开始的第2-10个字符用来表示权限的:
        1. 这9个，每3个为一组，左边3个是所有者权限，中间3个字符表示与所有者同一组的用户的权限，右边3个字符是其他用户的权限
        2. 字符代表的意义:
            1. r:读取,文件:读取内容,目录:浏览目录
            2. w:写入,文件:新增修改文件,目录:删除移动目录内文件
            3. x:执行,文件:执行文件,目录:进入目录
            4. -:表示不具有该项权限
    5. 每个文件都拥有自己的目录，通常集中放置在/home目录下，这些主目录的默认权限为"rwx------"。
3. 特殊权限:
    1. 除了一般权限以外，还有特殊权限。用户如果没有特殊要求，不要启用这些权限，避免出现安全漏洞。
    2. 特殊权限类型:
        1. SUID:可执行文件搭配这个权限可以得到特权，任意存取文件的所有者可以使用全部的系统资源。
        2. SGID:设置在文件上，可以访问用户组所使用的资源。
        3. Sticky:/tmp和/var/tmp目录供所有用户暂时存取文件，即每位用户皆拥有完整的权限进入目录，去浏览、删除和移动文件。
    3. 特殊权限占用x的位置来表示，表示上会有大小写区别，如果是小写的，那么是正在执行状态。

## 7.2. 权限设置
1. 只有系统管理员和文件所有者才能更改文件或目录的权限。

### 7.2.1. 文件管理器更改权限
1. 属性->权限->进行修改

### 7.2.2. 文字设定法
1. 使用chmod命令:
    1. 命令格式:`chmod [who] [+|-|=][mode][文件或目录名]`
    2. who参数以及含义:
        1. u:所有者
        2. g:用户组
        3. o:其他用户
        4. a:所有用户，系统默认值
    3. 操作符号:
        1. +:添加某个权限
        2. -:删除某个权限
        3. =:赋予给定权限并取消原先权限
    4. mode权限组合:
        1. r:可读
        2. w:可写
        3. x:可执行
        4. s:执行时把进程的属主或组ID置为该文件的文件属主
            + "g+s":设置文件的SUID权限
            + "u+s":设置文件的GUID权限
        5. t:保存程序的文本到交换设备上

### 7.2.3. 数字设定法
1. 需要三个数字来表示权限。
    1. 0:没有权限，对应-
    2. 1:可执行权限，对应x
    3. 2:写入权限，对应w
    4. 4:读取权限，对应r
2. 第四个数字是特殊权限:
    1. SUID:对应4
    2. SGID:对应2
    3. Sticky:对应1
3. 所有数字属性的格式为3个0-7的八进制数，其顺序是(u)(g)(o)
4. 例子:
    1. `-rwx------`:700
    2. `-rwxr--r--`:744
5. 这种形式的chmod命令:`chmod [n1n2n3] [文件或目录名]`
    1. n1:所有者的权限
    2. n2:同族用户的权限
    3. n3:其他用户的权限
    4. 注:`chmod 7 a`=`chmod 007 a`

## 7.3. 更改文件和目录的所有权
1. 文件和目录的创建者默认具有所有权，他们对文件和目录具有任何权限，可以进行任何操作，也可以将所有权交给其他用户。

### 7.3.1. chown命令
1. 使用chown命令可以更改文件和目录的所有者和用户组
2. 命令语法:`chown [-R][用户.组][文件|目录]`
3. 命令中参数:-R：将子级目录下的所有文件和目录的所有权一起改变。

### 7.3.2. chgrp命令
1. 使用chgrp命令可以更改文件或目录所属的组。
2. 命令语法:`chgrp [选项][用户组][文件|目录]`
3. 参数说明:-R:递归式地改变指定目录及其下的所有子目录和文件所属的组。