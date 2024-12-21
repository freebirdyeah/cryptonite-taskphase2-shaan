# Additional Reverse Engineering picoCTF challenges

## ARMssembly0

After researching I realised that I can't compile ARM-Assembly on my system (for obvious reasons: different arch), I installed `qemu-user-static` and `aarch64-linux-gnu` ARM compiler to run the compiled assembly on my PC.

I ran the following commands on the given file `chall.S`:-

```

~$ aarch64-linux-gnu-as -o chall1.o chall.S
~$ aarch64-linux-gnu-gcc -static -o chall1 chall1.o
~$ ./chall1

```

and got this on putting in the input:-

```

$ ./ch 1765227561 1830628817
Result: 1830628817

```

I converted the number `1830628817` into hex and wrapped the hex in picoCTF{} to get the flag.

flag: `picoCTF{6d1d2dd1}`


### incorrect tangents

wasted a lot of time on figuring out on how to compile and run ARM assembly on my machine but eventually figured it out.

### learnings

1. learned how to compile and run ARM assembly 

## GDB Baby Step 2

We basically had to find out the value of eax register again but this time reading the assembly code didn't give an answer. I ran the program in gdb and after `disassemble main` I saw

```

Dump of assembler code for function main:
   0x0000000000401106 <+0>:     endbr64
   0x000000000040110a <+4>:     push   rbp
   0x000000000040110b <+5>:     mov    rbp,rsp
   0x000000000040110e <+8>:     mov    DWORD PTR [rbp-0x14],edi
   0x0000000000401111 <+11>:    mov    QWORD PTR [rbp-0x20],rsi
   0x0000000000401115 <+15>:    mov    DWORD PTR [rbp-0x4],0x1e0da
   0x000000000040111c <+22>:    mov    DWORD PTR [rbp-0xc],0x25f
   0x0000000000401123 <+29>:    mov    DWORD PTR [rbp-0x8],0x0
   0x000000000040112a <+36>:    jmp    0x401136 <main+48>
   0x000000000040112c <+38>:    mov    eax,DWORD PTR [rbp-0x8]
   0x000000000040112f <+41>:    add    DWORD PTR [rbp-0x4],eax
   0x0000000000401132 <+44>:    add    DWORD PTR [rbp-0x8],0x1
   0x0000000000401136 <+48>:    mov    eax,DWORD PTR [rbp-0x8]
   0x0000000000401139 <+51>:    cmp    eax,DWORD PTR [rbp-0xc]
   0x000000000040113c <+54>:    jl     0x40112c <main+38>
   0x000000000040113e <+56>:    mov    eax,DWORD PTR [rbp-0x4]
   0x0000000000401141 <+59>:    pop    rbp
   0x0000000000401142 <+60>:    ret
```

I set breakpoint `break *0x0000000000401141` and ran the program, after it stopped I simply ran `info registers` and I got:-

```
(gdb) info registers
rax            0x4af4b             307019
```

flag: `picoCTF{307019}`

## vault door 1

I just looked at the code and wrote down the flag by hand.

flag: `picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_ff63b0}`


