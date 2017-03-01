* 为什么有这种需求?
    * 由于引导扇区最大只有512字节，如果我们在测试自己的写的操作系统的时候超过512个字节，就无法在引导扇区存放这个程序.
    * 解决这个问题，有两种方法，一种是写一个引导扇区，通过引导扇区来读取我们的操作系统，但是这个方法目前由于我个人的能力不易实现；另一种方法就是借助DOS，把操作系统程序编译成com文件，然后让dos来执行.
* 什么是com文件?
    * com文件是一种可执行程序的内存映像文件，大小限于64k，入口必须是100h.MS-DOS通过直接把该映象从文件拷贝到内存而加载.COM程序，它不作任何改变.
* 尝试使用dos来测试自己写的操作系统
    * 1.在Bochs官网下载FreeDos<font color="blue"><a href="http://bochs.sourceforge.net/guestos/freedos-img.tar.gz ">http://bochs.sourceforge.net/guestos/freedos-img.tar.gz </a></font>, 解压后将启动的a.img复制到我们的工作目录下，重命名为freedos.img.
    * 2.直接执行`bximage`生成一个软盘映像，命名为pm.img
    * 3.在bochsrc中加入(相当于安装a盘和b盘)
        
        ```
        floppya: 1_44=freedos.img, status=inserted
        floppyb: 1_44=pm.img, status=inserted
        boot: a
        ```
    * 4.启动bochs，会自动启动freedos，使用`format B:`格式化B盘，如图![1](https://cloud.githubusercontent.com/assets/16068384/21180804/9a69b71e-c235-11e6-9c59-e7211c77cce6.png)
    * 5.将代码中的07c00h改为0100h，并重新编译`nasm xxx.asm -o xxx.com`
    * 6.将xxx.com复制到虚拟软盘pm.img上(由于是虚拟软盘，无法直接复制，所以需要手动挂载和卸载)
    
        ```
        sudo mount -o loop pm.img /mnt/floppy
        sudo cp pmtest1.com /mnt/floppy/
        sudo umount /mnt/floppy
        ```
    * 7.切换到b盘，执行xxx.com即可看到结果
