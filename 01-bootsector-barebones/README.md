>计算机启动时，需要通过`引导扇区`来引导系统的启动，这里编写的就是引导区的代码（写的是一个死循环）．

> 官方文档列出的命令在执行的过程中会遇到很多问题，比如：
>- 将 asm 文件编译为 bin 文件，执行的命令应该是`nasm boot_sect_simple.asm -o boot_sect_simple.bin`（大概是因为Mac下的nasm接受参数的方式与其他操作系统下的不一样）．
>- 同样，当你启动系统时，所需要执行的命令应为`qemu-system-i386 -drive format=raw,file=boot_sect_simple.bin`通过 drive 来传入文件格式和文件名参数
>- 文章结尾提到，在完成引导后，会有新窗口弹出，然而并没有，这时需要手动进行 VNC 连接`vncviewer 127.0.0.1`（ qemu 启动的是一个vnc服务器，连接时可以省略端口号 5900）

> 最终会看到个显示`Booting from Hard Disk...`的命令行

*Concepts you may want to Google beforehand: assembler, BIOS*

**Goal: Create a file which the BIOS interprets as a bootable disk**

This is very exciting, we're going to create our own boot sector!

Theory
------

When the computer boots, the BIOS doesn't know how to load the OS, so it
delegates that task to the boot sector. Thus, the boot sector must be
placed in a known, standard location. That location is the first sector
of the disk (cylinder 0, head 0, sector 0) and it takes 512 bytes.

To make sure that the "disk is bootable", the BIOS checks that bytes
511 and 512 of the alleged boot sector are bytes `0xAA55`.

This is the simplest boot sector ever:

```
e9 fd ff 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
[ 29 more lines with sixteen zero-bytes each ]
00 00 00 00 00 00 00 00 00 00 00 00 00 00 55 aa
```

It is basically all zeros, ending with the 16-bit value
`0xAA55` (beware of endianness, x86 is little-endian).
The first three bytes perform an infinite jump

Simplest boot sector ever
-------------------------

You can either write the above 512 bytes
with a binary editor, or just write a very
simple assembler code:

```nasm
; Infinite loop (e9 fd ff)
loop:
    jmp loop

; Fill with 510 zeros minus the size of the previous code
times 510-($-$$) db 0
; Magic number
dw 0xaa55
```

To compile:
`nasm -f bin boot_sect_simple.asm -o boot_sect_simple.bin`

> OSX warning: if this drops an error, read chapter 00 again

I know you're anxious to try it out (I am!), so let's do it:

`qemu boot_sect_simple.bin`

> On some systems, you may have to run `qemu-system-x86_64 boot_sect_simple.bin` If this gives an SDL error, try passing the --nographic and/or --curses flag(s).

You will see a window open which says "Booting from Hard Disk..." and
nothing else. When was the last time you were so excited to see an infinite
loop? ;-)
