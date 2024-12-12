# picoCTF: Reverse Engineering

_for picoCTF RE challenges_


## GDB Baby step 1

The challenge requires us to find out the contents of `eax` register, I download the file and run `gdb debugger0_a`.

After `set disasssembly-flavor intel` and `disassemble main` I see:-


```
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   rbp
   0x000000000000112e <+5>:     mov    rbp,rsp
   0x0000000000001131 <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x0000000000001134 <+11>:    mov    QWORD PTR [rbp-0x10],rsi
   0x0000000000001138 <+15>:    mov    eax,0x86342
   0x000000000000113d <+20>:    pop    rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
```

I can see from `0x1138` address, value being moved into `eax` is `0x86342` which is `549698`

`flag: picoCTF{549698}`

### incorrect tangents

None

### learnings from this challenge

1. learning how to work with gdb
2. learning how to read basic asm instructions like `mov`
