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

***

# ftrace

## 取得的成果

## 遇到的问题和解决的思路
