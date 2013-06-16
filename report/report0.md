# 2013 年操作系统课程设计
## 李天阳

***

# palacios

## 取得的成果

能够在最新的 ucore 里面运行 palacios 启动一个 
linux ([http://distro.ibiblio.org/tinycorelinux/](http://distro.ibiblio.org/tinycorelinux/))

## 遇到的问题和解决的思路

### 将虚拟机镜像放入到 ucore 中

之前的做法是把镜像转成 16 进制后放在一个 `.h` 中, 这样会占用编译器大量的内存. 

现在采用 [kitten](https://software.sandia.gov/trac/kitten) 的办法, 
也就是把二进制文件转换成一个 `.o` 文件 
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

***

# ftrace

## 取得的成果

只在 mm 模块中加入 ftrace 的功能后, 我们能够在内核函数进入和内核函数返回的时候调用一个用来跟踪的函数, 
目前的跟踪函数是在被调用时打印一句话, 告诉用户跟踪函数已被调用. 
ftrace 中处理跟踪信息等跟踪功能尚未实现. 

## 遇到的问题和解决的思路

### linux 代码阅读

### 查看为什么增加了 ftrace 功能之后系统崩溃




