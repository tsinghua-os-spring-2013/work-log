As shown in [http://www.v3vee.org/palacios/palacios-1.3-simple.txt](http://www.v3vee.org/palacios/palacios-1.3-simple.txt), we need the Linux kernel source tree.

The usual approach using `linux-headers-2.6.32-5-amd64` package didn't seem to work.

We set up the source tree as shown in [http://kernel-handbook.alioth.debian.org/ch-common-tasks.html](http://kernel-handbook.alioth.debian.org/ch-common-tasks.html). 
But it still didn't seem to work.

There are probably bugs in Palacios's `Makefile`.

__Fixing this is probably too much to do, and unnecessary, too.__
