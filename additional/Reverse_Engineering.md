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

