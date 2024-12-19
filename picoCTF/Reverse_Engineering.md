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


## Vault-door 3

It was pretty simple, once I understood the Java code written. The program just took in a password, and scrambled the characters as per the following code:-

```
char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }

```

and it checked the scrambled password to: `jU5t_a_sna_3lpm18g947_u_4_m9r54f`

to reverse this I implemented a simple Python script:-


```

scrambled_pass = "jU5t_a_sna_3lpm18g947_u_4_m9r54f"
password = []


for i in range(0, 8):
	password[i] = scrambled_pass[i]

for i in range(8, 16):
	password[23-i] = scrambled_pass[i]

for i in range(16, 32, 2):
    	password[46 - i] = scrambled_pass[i]

for i in range(31, 16, -2):
    	password[i] = scrambled_pass[i]


unscrambled_pass = "".join(scrambled_passw)
print(unscrambled_pass)

```

and after wrapping it up in picoCTF{} I got: `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}`

### Incorrect tangents

None

### learnings

1. learned how to reverse engineer weird substitutions

