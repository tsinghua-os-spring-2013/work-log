As shown in [http://www.linuxjournal.com/content/embedding-file-executable-aka-hello-world-version-5967](http://www.linuxjournal.com/content/embedding-file-executable-aka-hello-world-version-5967), 
we first rename a `.img` file created by Palacios's `build_vm` to `palacios.img`, 
and then we use 

    objcopy --input binary --output elf64-x86-64 --binary-architecture i386 palacios.img palacios.o

to createan `palacios.o` from `palacios.img`.

Now copy `palacios.o` to the same directory as `libv3vee.a` and build.
