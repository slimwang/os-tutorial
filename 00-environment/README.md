第一步是搭建环境

*先通过搜索引擎了解一些专业名词: linux, mac, terminal, compiler, emulator, nasm, qemu*

**主要目标: 安装这个教程所需的软件**

[nasm](https://nasm.us/): 是汇编语言编译器，负责把汇编语言翻译成机器语言

[qemu](https://www.qemu.org/download/): 模拟器，运行上面生成的代码的


Mac: [安装 Homebrew](http://brew.sh) 然后 `brew install qemu nasm`

其他操作系统: 自行百度，不作赘述，两者都可以通过命令行直接安装

注意: 
- 不要用Xcode developer tools自带的`nasm`，而是使用`/usr/local/bin/nasm`
- 如果选择从源码编译，记得在`./configure`时加`--prefix`,不然卸载这两个软件时会很痛苦
- 不同操作系统里的qemu可执行文件可能有不同的名称，你可能需要运行`qemu-system-x86_64 binfile`
（你可以通过`ls /usr/local/bin/qemu*`或`ls /usr/bin/qemu*`来确定，在我的系统上是`qemu-system-i386`）
