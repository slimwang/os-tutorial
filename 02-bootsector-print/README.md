> 这一节是关于在屏幕上打印文本信息的，也是正式开始编写功能性的汇编代码，很多人一看到汇编就感到头疼，事实上，汇编的英文是`assembler`，而`assemble`有动词`装配　组合`的意思，汇编语言的提出是为了解决人类难以阅读二进制机器码的问题的，它的本质是助记符，通过将一大段机器指令标记为MOV ADD等字符，来对底层语言进行一次抽象，这样人们就可以通过将这些标记进行`组合`来编写程序，提高了开发效率和程序的可读性，并降低了Bug出现的几率

*Concepts you may want to Google beforehand: interrupts, CPU
registers*

**Goal: Make our previously silent boot sector print some text**

We will improve a bit on our infinite-loop boot sector and print
something on the screen. We will raise an interrupt for this.

On this example we are going to write each character of the "Hello"
word into the register `al` (lower part of `ax`), the bytes `0x0e`
into `ah` (the higher part of `ax`) and raise interrupt `0x10` which
is a general interrupt for video services.

`0x0e` on `ah` tells the video interrupt that the actual function
we want to run is to 'write the contents of `al` in tty mode'.

We will set tty mode only once though in the real world we
cannot be sure that the contents of `ah` are constant. Some other
process may run on the CPU while we are sleeping, not clean
up properly and leave garbage data on `ah`.

For this example we don't need to take care of that since we are
the only thing running on the CPU.

Our new boot sector looks like this:
```nasm
mov ah, 0x0e ; tty mode
mov al, 'H'
int 0x10
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
int 0x10 ; 'l' is still on al, remember?
mov al, 'o'
int 0x10

jmp $ ; jump to current address = infinite loop

; padding and magic number
times 510 - ($-$$) db 0
dw 0xaa55
```

You can examine the binary data with `xxd file.bin`

Anyway, you know the drill:

`nasm -fbin boot_sect_hello.asm -o boot_sect_hello.bin`

`qemu boot_sect_hello.bin`

Your boot sector will say 'Hello' and hang on an infinite loop
