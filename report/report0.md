# 2013 年操作系统课程设计
## 李天阳

***

# palacios

## 取得的成果

能够在最新的 ucore 里面运行 palacios 启动一个 
linux ([http://distro.ibiblio.org/tinycorelinux/](http://distro.ibiblio.org/tinycorelinux/))

## 遇到的问题和解决的思路

### 将虚拟机镜像放入到 ucore 中

之前的做法是把镜像转成 16 进制后放在一个 .h 中, 这样会占用编译器大量的内存. 

现在采用 [kitten](https://software.sandia.gov/trac/kitten) 的办法, 
也就是把二进制文件转换成一个 .o 文件 
([http://www.linuxjournal.com/content/embedding-file-executable-aka-hello-world-version-5967](http://www.linuxjournal.com/content/embedding-file-executable-aka-hello-world-version-5967)). 
详细做法在[这里](https://github.com/tsinghua-os-spring-2013/work-log/blob/master/link-vm-img-into-ucore-kernel/log1.md)给出.

### ucore 中 palacios 运行速度过慢

ucore 中 palacios 运行速度特别慢, tinycorelinux 的启动需要等大概一天的时间. 

这里采用了 gprof 进行跟踪, 但是由于 palacios 实用了 AMD-V 技术进行虚拟化, 
gprof 会将虚拟机 guest 中运行的函数错误地认为是 host 中的函数. 
例如, `do_halt` 就出现了被 gprof 认为是被调用了, 但是这个函数实际上没有被调用. 

这里的解决方案是像 linux 中一样实现 ftrace, 
对内核中的函数调用进行跟踪. 

### ucore 中 palacios 启动 tinycorelinux 之后在 shell 界面死机

这个需要在 ucore 中 palacios 运行的速度有所提升后才方便进行跟踪并且调试.

## 未来的方向

首先用 ftrace 跟踪 ucore 性能, 进行优化后便于进一步调试 ucore 中的 palacios 运行虚拟机. 

***

# ftrace

## 取得的成果

在 amd64 架构下, 
只在 mm 模块中加入 ftrace 的功能后, 我们能够在内核函数进入和内核函数返回的时候调用一个用来跟踪的函数, 
目前的跟踪函数是在被调用时打印一句话, 告诉用户跟踪函数已被调用. 
ftrace 中处理跟踪信息等跟踪功能尚未实现. 

## 遇到的问题和解决的思路

### linux 代码阅读

[https://www.kernel.org/doc/Documentation/](https://www.kernel.org/doc/Documentation/) 
提供了关于 linux 中的部分文档, 但是大部分内容比较简略不详细. 
linux 源代码解压后, 可以执行 `make tags`, `make cscope` 生成支持 ctags 和 cscope 的文件用来查阅代码. 
同时 [http://lxr.linux.no/](http://lxr.linux.no/) 和 [http://lxr.free-electrons.com/](http://lxr.free-electrons.com/) 
可以查阅代码. [understanding the linux kernel](http://connect.safaribooksonline.com/0596005652) 也是较好的参考. 

同时, 对于 linux 里面的汇编代码, 需要参考对应的架构的 ABI, 
例如对于 amd64 需要参考 [https://refspecs.linuxfoundation.org/elf/x86_64-abi-0.95.pdf](https://refspecs.linuxfoundation.org/elf/x86_64-abi-0.95.pdf).

### 查看为什么增加了 ftrace 功能之后系统崩溃

通过 qemu 和 gdb 可以对 ucore 进行跟踪, 下面列举一些遇到过的问题. 

* 不该添加 `mcount` 的地方添加了 `mcount`

`mcount` 中调用了添加了 `mcount` 的函数后就会形成死递归, 所以要注意. 

需要在 makefile 中对于一些文件不使用 gcc -pg 选项, make 中有功能叫 [`filter-out`](http://www.gnu.org/software/make/manual/make.html#Text-Functions). 
另外, linux 中的部分函数使用了 `notrace`, 也就是 `__attribute__((no_instrument_function))` 防止 gcc 加入 `mcount`. 

下一步的工作需要看 linux 中那些部分编译时是不用 -pg 的, 并且看其他部分里哪些函数是加了 `notrace` 的. 

* 需要等到系统启动完善到一定情况下才能开启 ftrace

目前在 [`kern_init`](https://github.com/eternalNight/ucore_plus-next/blob/lty/ftrace/ucore/src/kern-ucore/arch/amd64/init/init.c#L37) 
里面通过设置 `function_trace_stop` 来开关 ftrace. 
但是这样只能保证 ucore 中只在 mm 加入 `mcount` 后能够启动并且基本功能能够运行. 

下一步的工作需要阅读更多的 linux 源代码, 看 linux 是如何开启和关闭 tracing 的功能的. 

* ucore 链接时出错

在汇编代码中用 C 中的变量在编译时出错 ([这里](https://github.com/eternalNight/ucore_plus-next/blob/lty/ftrace/ucore/src/kern-ucore/arch/amd64/init/no_mcount/entry64.S#L37)出的错)

    relocation truncated to fit: R_X86_64_32S against symbol
    
这样的话可以使用 `movabs` 指令先把变量移动到一个寄存器中 (例如 `%eax`). 

## 未来的方向

了解 linux 里面 tracing 功能是如何开启和关闭的, 保证在整个内核可以加 `mcount` 
的地方加了 `mcount` 之后系统不会崩溃能够正常使用并且测试能够同各国. 


另外, 需要分析 linux 源代码, 看哪些地方是不加 `mcount` 的.

在上述两个实现之后, 可以考虑将 ftrace 跟踪信息分析的功能加入 ucore, 
能够对 ucore 性能有所分析. 








