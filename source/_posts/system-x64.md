---
title: 《一个64操作系统的设计与实现》阅读笔记（一）
date: 2020-03-03 00:25:53
tags:
---
# 第二章
## 2019/03/02
bochs是真的让人摸不着头脑。。按照书中的
> ./configure --with-x11 --with-wx --enable-debugger --enable-disasm --enable-all-optimizations --enable-readline --enable-long-phy-address --enable-x86-64 --enable-smp --enable-cpu-level=6 --enable-large-ramfile --enable-ltdl-install --enable-idle-hack --enable-plugins --enable-a20-pin --enable-repeat-speedups --enable-fast-function-calls --enable-handlers-chaining --enable-trace-linking --enable-configurable-msrs --enable-show-ips --enable-cpp --enable-debugger-gui --enable-iodebug --enable-logging --enable-assert-checks --enable-fpu --enable-vmx=2 --enable-svm --enable-3dnow --enable-alignment-check --enable-monitor-mwait --enable-avx --enable-evex --enable-x86-debugger --enable-pci --enable-usb --enable-voodoo

进行了配置以后，使用make install 进行编译的时候失败了，报了一个

```bash
make[1]: Entering directory `/home/xuxin/study/bochs/iodev/usb'
make[1]: `libusb.a' is up to date.
make[1]: Leaving directory `/home/xuxin/study/bochs/iodev/usb'
echo done
done
cd bx_debug && \
	make  libdebug.a
make[1]: Entering directory `/home/xuxin/study/bochs/bx_debug'
g++ -c -I.. -I./.. -I../instrument/stubs -I./../instrument/stubs -I. -I./. -g -O2 -D_FILE_OFFSET_BITS=64 -D_LARGE_FILES -pthread   dbg_main.cpp -o dbg_main.o
In file included from ../cpu/cpu.h:648,
                 from dbg_main.cpp:28:
../cpu/icache.h:31: error: ISO C++ forbids initialization of member ‘PHY_MEM_PAGES’
../cpu/icache.h:31: error: making ‘PHY_MEM_PAGES’ static
make[1]: *** [dbg_main.o] Error 1
make[1]: Leaving directory `/home/xuxin/study/bochs/bx_debug'
make: *** [bx_debug/libdebug.a] Error 2
```

对关键字 ‘PHY_MEM_PAGES’ 进行搜索，基本就是毫无信息，一筹莫展，遂记录，然后睡觉

不行，越想越气，起床继续写

查资料根据

>ISO C++ forbids initialization of member

怀疑是gcc/c++版本太低，遂找centos6的升级，因为centos默认最高版本只有4.4.7，由于位的bochs版本是2.6.10的，所以可能不兼容，遂想办法升级gcc版本

根据文章 https://www.vpser.net/manage/centos-6-upgrade-gcc.html 进行更新，然后make，大功告成。。。。我真是惊呆了。。。

如果新学这个东西的人还是别用centos6了，毕竟8都已经出来了，用新的比较好。。。

编译过程中确实出现了缺少文件，此处备用，用于以后以防万一还得再打一遍。。。

```bash
cp misc/bximage.cpp misc/bximage.cc
cp iodev/hdimage/hdimage.cpp iodev/hdimage/hdimage.cc
cp iodev/hdimage/vmware3.cpp iodev/hdimage/vmware3.cc
cp iodev/hdimage/vmware4.cpp iodev/hdimage/vmware4.cc
cp iodev/hdimage/vpc-img.cpp iodev/hdimage/vpc-img.cc
cp iodev/hdimage/vbox.cpp iodev/hdimage/vbox.cc
```

然后编译成功

哈哈哈哈哈，不愧是我（臭屁）

## 2019/03/02(其实是三月三号清晨)
失眠，反反复复从12点床上晃到6点没睡着，遂起来学习

尝试打开虚拟机失败，启动不起来，决定重新安装虚拟机（sad）

有了前车之鉴决定装centos 8 配合最新点软件包，希望一切顺利

然而失败了，报错

> Bochs is not compiled with lowlevel sound support

可能需要再研究一下配置或者bochsrc，睡啦