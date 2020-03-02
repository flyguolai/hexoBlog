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