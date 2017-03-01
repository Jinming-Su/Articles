# Linux 系统管理员
***
### Unit 20 启动流程、模块管理与Loader
* 启动流程
    * 加载BIOS的硬件信息与进行自我测试，并一依据设置取得第一个可启动的设备(如硬盘)
    * 读取并执行第一个启动设备内的MBR的boot Loader(即是grub, spfdisk等程序)
    * 依据boot loader的设置加载Kernel, Kernel会开始检测硬件与加载驱动程序
    * 在硬件驱动成功后，Kernel会主动调用init进程，而init会取得run-level信息
    * init执行/etc/rc.d/sysinit文件来准备软件执行的操作环境(如网络、时区等)
    * init执行run-level的各个服务的启动(script方式)
    * init执行/etc/rc.d/rc.local文件
    * init执行终端机模拟程序mingetty来启动login进程，最后就等待用户登录
* 启动流程分析
    * 加载内核检测硬件与initrd的功能
        * 当通过boot loader的管理而开始读取内核文件后，Linux就会将内核解压缩到内存当中，并且利用内核的功能，开始测试与驱动各个周边设备，包括存储设备、CPU、网卡，声卡等.此时Linux内核会以自己的功能重新检测一次硬件，而不一定会使用BIOS检测到的硬件信息，也就是说，<b>内核此时才可是接管BIOS后的工作了</b>.
        * /boot内文件功能
            * config-3.19.0-43-generic //此版本内核被编译时选择的功能与模块配置文件
            * grub/  //引导装载程序grub相关数据目录
            * initrd.img-3.19.0-43-generic //虚拟文件系统文件
            * System.map-3.19.0-43-generic //内核功能放置到内存地址的对应表
            * vmlinuz-3.19.0-43-generic  //内核文件
        * 为了方便硬件开发商与其他内核功能开发者的便利，因此Linux内核是可以通过动态加载内核模块的(可以简单理解为驱动程序)，这些内核模块就放置在/lib/modules中.
        * 虚拟文件系统(initialRAM Disk)一般使用的文件名为/boot/initrd.这个文件能通过boot-loader来加载到内存中，然后这个文件会被解压缩并且在内存中仿真成一个根目录，且此仿真在内存当中的文件系统能够提供一个可执行的程序，通过该程序来加载启动过程中所需要的内核模块，通常这些模块就是USB，RAID，LVM，SCSI等文件系统与磁盘接口的驱动程序.等载入完成后会帮助内核重新调用/sbin/init来开始后续的正常启动流程.![1](https://cloud.githubusercontent.com/assets/16068384/21078506/48a47fca-bfad-11e6-80c6-12ee37313692.png)
        <font color="red">注意: 并不是没有initrd就无法顺利启动.当更目录是在特殊的磁盘接口(USB, SATA，SCSI)中，或者文件系统较为特殊(LVM，RAID等)，才需要initrd.如果Linux是安装在IDE接口的磁盘上，并且使用默认的ext2/ext3文件系统，那么就不需要initrd就能够顺利启动进入linux.</font>
    * 第一个进程init及配置文件/etc/inittab与runlevel
        * /sbin/init最主要的功能就是准备软件执行的环境，包括系统的主机名、网络配置、语系处理、文件系统格式及其他服务的启动等.所有的操作都会通过init的配置文件/etc/inittab来规划.而inittab内还有一个很重要的设置选项，就是默认的run level(启动执行等级).
        * Linux是通过设置run level来规定系统使用不同的服务来启动，让Linux的使用环境不同.基本上，根据有无网络与有无X window而将run level分为7个等级
            * 0-halt(系统直接关机)
            * 1-single user mode(单用户维护模式，用在系统出问题时的维护，类似windows的安全模式)
            * 2-Multi-user without NFS(类似runlevel3，但无NFS服务)
            * 3-Full multi-user mode(完整含有网络功能的纯文本模式)
            * 4-unused(系统保留功能)
            * 5-X11(与runlevel3类似，但加载使用X window)
            * 6-reboot(重新启动)
    * init处理系统初始化流程/etc/rc.d/rc.sysinit
        * 开始加载各项服务之前，需要先设置好整个系统环境，主要是利用/etc/rc.d/rc.sysinit(ubuntu下是/etc/init/rc.sysinit)
        * sysinit主要工作
            * 1.取得网络环境与主机类型: 读取网络配置文件/etc/sysconfig/network，取得主机名与默认网关等网络环境.
            * 2.测试与挂载内存设备/proc及USB设备/sys
            * 3.决定是否启动SELinux
            * 4.启动系统的随机数生成器: 可以帮系统进行一些密码加密盐酸功能，在此需要启动两次随机数生成器.
            * 5.设置终端机字体
            * 6.设置显示于启动过程中的欢迎界面textbanner
            * 7.设置系统时间与时区: 需读入/etc/sysconfig/clock设置值
            * 8.接口设备的检测与Plug and Play(PnP)参数的测试: 根据内核在启动时检测的结果(/proc/sys/kernel/modprobe)开始进行ide/scsi/网络/音效等接口设备的检测，以及利用已加载的模块进行PnP设备的参数检测
            * 9.用户自定义模块的加载: 用户可在/etc/sysconfig/modules/*.modules中加入自定义模块
            * 10.加载内核的相关设置: 系统自动读取/etc/syscel.conf中的设置值，使内核功能成为我们想要的样子
            * 11.设置主机名与初始化电源管理模块(ACPI)
            * 12.初始化软件磁盘阵列: 主要通过/etc/mdadm.conf来设置好
            * 13.初始化LVM的文件系统功能
            * 14.以fsck检测磁盘文件系统: 会进行filesystem check
            * 15.进行磁盘配额quota的转换(非必要)
            * 16.重新以可读写模式挂载系统磁盘
            * 17.启动quota功能(可以看出不需要自定义quotaon的操作)
            * 18.启动系统伪随机数生成器(pseudo-random)
            * 19.清楚启动过程中的临时文件
            * 20.将启动相关信息加入/var/log/dmesg文件中
    * 启动系统服务与相关启动配置文件(/etc/rc.d/rc N & /etc/sysconfig)
    * 用户自定义开机启动程序/etc/rc.d/rc.local
    * 根据/etc/inittab的设置加载终端机或X Window界面
    * run level的切换
        * 临时切换命令
            * `runlevel`: 查看当前run level
            * `init 3`: 切换到runlevel3
* 内核与内核模块
    * 内核与内核模块存放位置
        * 内核: /boot/vmlinuz或/boot/vmlinuz-version
        * 内核解压缩所需RAMDisk: /boot/initrd
        * 内核模块: /lib/modules/version/kernel或/lib/modules/$(uname-r)/kernel
        * 内核源码: /usr/src/linux或/usr/src/kernels
        * 内核版本: /proc/version
        * 系统内核功能: /proc/sys/kernel
    * 内核模块依赖性
        * modules.dep文件记录了内核支持的模块的各项依赖性
        * `depmod [-Ane]`进行modules.dep文件的创建和更新
    * 内核模块查看
        * `lsmode`: 查看目前内核加载模块量
        * `modinfo [-adln] [module_name|filename]`: 查看每个模块的信息
    * 内核模块的加载与删除
        * `modprobe [-lcfs] module_name`: 分析依赖性之后决定加载哪些模块
        * `insmod`: 用户自行加载
        * `rmmod [-fw] module_name`
    * 内核模块的额外参数设置: /etc/modprobe.conf
* Boot Loader: Grub
    * boot loader的两个阶段
        * 1.执行boot loader主程序: 通常MBR或boot sector仅安装boot loader的最小主程序，并没有安装loader的相关配置文件.
        * 2.主程序加载配置文件: 通过boot loader加载所有配置文件与相关的环境参数文件(包括文件系统定于与主要配置文件menu.lst)，一般来说，配置文件都在/boot下(例如grub的都在/boot/grub).
    * grub的配置文件/boot/grub/menu.lst与菜单类型
        * grub堆硬盘的识别使用代号: (hd0, 0)
    * 测试与安装grub
        * 安装grub相关的文件到设备上(对应bootloader的阶段2): `grub-install [--root-directory=DIR] INSTALL_DEVICE`
        * 之后需要设置配置文件menu.lst
        * 以grub shell安装grub主程序到MBR或者boot sector(对应阶段1)
            * `grub`: 进入grub shell
            * `root (hd0, 0)`: 设置含有grub目录的分区
            * `setup (hd0)`(MBR)或`setup (hd0, 0)`(boot sector)

### Unit 21 硬件检测: 硬件数据收集与驱动及lm_sensors
* 硬件信息的收集与分析
    * 内核检测到的各项硬件设备后来会被记录在/proc与/sys中，包括/proc/cpuinfo, /proc/partitions, /proc/interrupts等.
    * 常见命令
        * fdisk: 使用fdisk -l将分区表列出.
        * hdparm: 可查看磁盘的信息与测试读写速度.
        * dmesg: 查看内核运行过程当中所显示的各项信息记录.
        * vmstat: 可分析系统(CPU/RAM/IO)目前的状态.
        * lspci: 列出整个PC系统的PCI接口设备.
        * lsusb: 了出目前系统上面哥哥usb端口的状态与连接的usb设备.
        * lostat: 与vmstat类似，可实时列出整个cpu与接口设备的I/O状态.
* 使用lm_sensors取得温度、电压等信息
    * `sensors-dectect`: 从检测到主板芯片组后，将该信息写入配置文件当中
    * `sensors`: 检测温度、电压等硬件参数
