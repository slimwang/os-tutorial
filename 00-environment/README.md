> 第一步是搭建环境

>[nasm](https://nasm.us/): 是汇编语言编译器，负责把汇编语言翻译成机器语言

>[qemu](https://www.qemu.org/download/): 模拟器，运行上面生成的代码的

> 注意:
- 如果选择从源码编译，记得在`./configure`时加`--prefix`,不然卸载这两个软件时会很痛苦
- 不同操作系统里的qemu可执行文件可能有不同的名称，你可能需要运行`qemu-system-x86_64 binfile`
（你可以通过`ls /usr/local/bin/qemu*`或`ls /usr/bin/qemu*`来确定，在我的系统上是`qemu-system-i386`）



Concepts you may want to Google beforehand: linux, mac, terminal, compiler, emulator, nasm, qemu

Goal: Install the software required to run this tutorial

I'm working on a Mac, though Linux is better because it will have all the standard tools already available for you.

On a mac, install Homebrew and then brew install qemu nasm

Don't use the Xcode developer tools nasm if you have them installed, they won't work for the most cases. Always use /usr/local/bin/nasm

On some systems qemu is split into multiple binaries. You may want to call qemu-system-x86_64 binfile
