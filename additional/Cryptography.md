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


## basic mod2

This time it involved modular multiplicative inverses, I looked them up on wikipedia and found modular multiplicative inverse 'a' w.r.t 'm' of 'x' is a number such that ax mod m = 1

This is the script I made:-

```
import sympy

char_list = list('*') + list("ABCDEFGHIJKLMNOPQRSTUVWXYZ") + list("0123456789") + list('_')
enc = [268, 413, 438, 313, 426, 337, 272, 188, 392, 338, 77, 332, 139, 113, 92, 239, 247, 120, 419, 72, 295, 190, 131]
denc = []

for i in enc:
        j = sympy.mod_inverse(i%41, 41)
        denc.append(char_list[j])

print("".join(denc))
```

flag: `picoCTF{1NV3R53LY_H4RD_8A05D939}`


## HideToSee

I ran `binwalk` and `zsteg` but neither worked, the hint said try extracting the file so I used `steghide`, I ran `stehide extract -sf atbash.jpg` and entered nothing when it asked for password, It surpisingly worked, I got the encrypted flag in a `.txt` file and decrypted using an online tool.

flag: `picoCTF{atbash_crack_ac751ec6}`

## Dachshund attacks

Dachshund = Wiener lol

I googled, and there's a type of attack called wiener's attack on the RSA system when d is small. I just had to find it.

I used this repo on github to find it: `https://github.com/orisano/owiener`

and made this python script to get d:-

```
import owiener

e=13638596850950147075002600846582053224326573738288977399226969144539023440068086265602933962139970405438068072735490957136993942650495808773325053819648041598810554878625463257085988671289523433899771349441622200039780844930489738195545249499960009873101038216283802981149383867269735749765307879140667667849
n=106741270125292219633788933100946179048945856330100167199005250257685914218566236105442013142075984660656103207157224973228414077300928913568424907223800224976905187944926620338108100885782381921423399102594430920428675997248620856840620396869725596414772056311391853036985523933816904737728088601790160667917
c=21011228910049049755724405647493722349426636717890474099680666618738757442186131250436061770668488483238034468007506248939214006489337254053336342429882535618470751292709031005009947085126307838317380493428132319395166985486363062913645755670682675337805016506263896824910630988094235894408413998183737872766

d = owiener.attack(e, n)

if d is None:
    print("Failed")
else:
    print("Hacked d={}".format(d))
```

got this as output:-

```
Hacked d=20702173290422837002474475545271829782882304928968346682153637778386821167289
```


and put all of this on dcode.fr's tool for cracking RSA and got the flag.

flag: `picoCTF{proving_wiener_6907362}`

## waves over lambda

Opened it. Looked like monoalphabetic substitution, went over to dcode.fr's tool, decoded it.

Very easy.

flag: `frequency_is_c_over_lambda_agflcgtyue` (flag not in usual format)

## credstuff

Searched for user 'cultiris' in the usernames.txt file, got the line number then went to the corresponding password in passwords.txt

password was encrypted using ROT-13, decrypted it to get the flag.

flag: `picoCTF{C7r1F_54V35_71M3}`
