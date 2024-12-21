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
