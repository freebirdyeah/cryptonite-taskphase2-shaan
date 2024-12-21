# Additional Cryptography Challenges

## ReadMyCert

A .csr file was given, I `cat`ed it and saw that it's contents were encrypted in base64. I decoded it 
and found the flag within the decrypted output.

flag: `picoCTF{read_mycert_57f58832}`

## substituion0

I just used dcode.fr 's mono-alphabetic substitution decryptor.

flag: `picoCTF{5UB5717U710N_3V0LU710N_357BF9FF}`

## substitution1

Did the same thing as the previous challenge, however this time the flag didn't submit. This confused me for a bit and I noticed in the flag there should be a 'Q' instead of an 'X', I made the correction and submitted the flag.

flag: `picoCTF{FR3QU3NCY_4774CK5_4R3_C001_7AA384BC}`

## substitution2

This time I had the 'spaces can be ignored or are missing' option on dcode.fr 's mono-alphabetic substitution cipher.

So I used it.

flag: `picoCTF{N6R4M_4N41Y515_15_73D10U5_8E1BF808}`

## basic mod1

I used this script to decrypt:-

```

char_list = list("ABCDEFGHIJKLMNOPQRSTUVWXYZ") + list("0123456789") + list('_')
enc = [350, 63, 353, 198, 114, 369, 346, 184, 202, 322, 94, 235, 114, 110, 185, 188, 225, 212, 366, 374, 261, 213]
denc = []

for i in enc:
        denc.append(char_list[i%37])

print("".join(denc))
```

flag:`picoCTF{R0UND_N_R0UND_ADD17EC2}`
