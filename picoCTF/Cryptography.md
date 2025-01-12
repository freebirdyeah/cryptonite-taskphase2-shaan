# picoCTF: Cryptography

_for picoCTF cryptography challenges_

## c3

first we look at the given script:-

```
import sys
chars = ""
from fileinput import input
for line in input():
  chars += line

lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""

prev = 0
for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur

sys.stdout.write(out)
```

For every character in the given input, the algorithm looks up the index of that character in `lookup1` (saves the value to `cur`) and then takes that index and operates on it using: `(cur-prev)%40` to get a new index value which is used to get a new character from `lookup2` and the `prev` value is set to `cur` for the succeeding character

We know that the value of `prev` for the first character is 0 and the `cur` value lies between 0-39 (since both `lookup1` and `lookup2` have len = 40) so `(cur - prev) %40` will always yield values between 0-39 (even in the case when `cur - prev < 0`), since `prev = 0` for the first character and `cur` lies in 0-39 the index won't change when operated on and hence we can decrypt the first character just by taking it's index in `lookup2` and finding `lookup1[index]`, then we set `prev = index` and begin the decryption process for subsequent characters.

For the next character and onwards we know the value of `prev` and we can also find out the value of `(cur - prev)%40` simply by looking up the encrypted character in `lookup2` now we've something like this:-

```
lhs = (X - prev) % 40
```

Now although `X` can have multiple solutions, we only need that solution which lies in 0-39 which will be:-

```
lhs + 40p = X - prev; 	p is some integer
lhs + prev = X + 40q;	q is some integer	

(lhs + prev) % 40 = X 
```

Using modular arithmetic, we can find X which is `cur` value for the encrypted character and now we can just lookup `lookup1` using the `cur` value to find the decrypted character and set `prev = cur` for the succeeding character.

On this logic I wrote this script:-

```
lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

def find_cur(lhs, prev):
	return (lhs + prev) % 40


ciphertext = list("DLSeGAGDgBNJDQJDCFSFnRBIDjgHoDFCFtHDgJpiHtGDmMAQFnRBJKkBAsTMrsPSDDnEFCFtIbEDtDCIbFCFtHTJDKerFldbFObFCFtLBFkBAAAPFnRBJGEkerFlcPgKkImHnIlATJDKbTbFOkdNnsgbnJRMFnRBNAFkBAAAbrcbTKAkOgFpOgFpOpkBAAAAAAAiClFGIPFnRBaKliCgClFGtIBAAAAAAAOgGEkImHnIl")

decrypted =  ""
prev = 0

for char in ciphertext:
	
	lookup2_index = lookup2.index(char)
	cur = find_cur(lookup2_index, prev)
	decrypted += lookup1[cur]
	prev = cur


print(decrypted)

```


On running this I got:-

```
#asciiorder
#fortychars
#selfinput
#pythontwo

chars = ""
from fileinput import input
for line in input():
    chars += line
b = 1 / 1

for i in range(len(chars)):
    if i == b * b * b:
        print chars[i] #prints
        b += 1 / 1


```

judging by "#selfinput" and "#pythontwo" I realised this was a python2 script and asked for itself as input

so on running `$ python2 c3_2.py < c3_2.py`

I got:-

```
a
d
l
i
b
s
```

flag: `picoCTF{adlibs}`

### incorrect tangents

Initially, I thought of decrypting the ciphertext starting from the end character since it's cyclical and keeps using the index of the preceeding character. However I quickly realised this was the wrong approach since I wouldn't know `prev` value (which is needed) as well as `(cur - prev) % 40` (a.k.a `lhs`) value.

After decrypting the ciphertext, I modified the script a little bit to take input from the user instead of the file and tried to input the orginal deciphered script using paste as one line. This messed up the flag as I saw `adl...` followed by garbled letters. Which made me go on further incorrect tangents where I tried different inputs.

I got the flag only after I ran the script and self-inputted as the program intended which was by `$ python2 c3_2.py < c3_2.py`

### Learnings from c3

1. always run the program as it's intended
2. got to learn a little more about algebra involving modulo function
 

## custom encryption

solved this by tracing my steps back one by one:-

```
from random import randint
import sys


def generator(g, x, p):
    return pow(g, x) % p


def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher


def is_prime(p):
    v = 0
    for i in range(2, p + 1):
        if p % i == 0:
            v = v + 1
    if v > 1:
        return False
    else:
        return True


def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text


def test(plain_text, text_key):
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
    cipher = encrypt(semi_cipher, shared_key)
    print(f'cipher is: {cipher}')


if __name__ == "__main__":
    message = sys.argv[1]
    test(message, "trudeau")
```

now looking at the last `cipher = encrypt(semi_cipher, shared_key)` part, we know `a`, `b`, `p` and `g` and we can use this to calculate the `shared_key` and we already have the cipher.

Now on researching I found out the script involves Diffie-Hellman key exchange, which uses two private keys of both parties `a` and `b` to generate a public key with two numbers `p` and `g` so I used this script to calculate the public key i.e. `shared_key`


```
p = 97  
g = 31  
a = 89  
b = 27  

u = pow(g, a, p)  
v = pow(g, b, p)  

shared_key = pow(v, a, p)  # or equivalently pow(u, b, p)
print(shared_key)

```

And I get `shared_key` to be `12`, Now we need to find out the `semi-cipher` which we can do by reversing the `encrypt` function.


```

def decrypt(cipher, key):
    plaintext = ""
    for i in cipher:
        
        original_char_code = i // (key * 311)
        
        if 32 <= original_char_code <= 126:  
            plaintext += chr(original_char_code)
        else:
            plaintext += f'\\x{original_char_code:02x}'    # f-string used to find out the non-printable ascii letters
    
    return plaintext

print(decrypt([33588, 276168, 261240, 302292, 343344, 328416, 242580, 85836, 82104, 156744, 0, 309756, 78372, 18660, 253776, 0, 82104, 320952, 3732, 231384, 89568, 100764, 22392, 22392, 63444, 22392, 97032, 190332, 119424, 182868, 97032, 26124, 44784, 63444], 12)
)

```

which gives me: `\x09JFQ\XA\x17\x16*\x00S\x15\x05D\x00\x16V\x01>\x18\x1b\x06\x06\x11\x06\x1a3 1\x1a\x07\x0c\x11`, Now we need to reverse the `dynamic_xor_encrypt` function, we already have the `text_key` which is `trudeau` and we also have the `semi_cipher`:-


```

semi_cipher = "\x09JFQ\XA\x17\x16*\x00S\x15\x05D\x00\x16V\x01>\x18\x1b\x06\x06\x11\x06\x1a3 1\x1a\x07\x0c\x11"

def dynamic_xor_decrypt(ciphertext, text_key):
    decrypted_text = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))  # XOR the character back
        decrypted_text += decrypted_char
    return decrypted_text

plain_text = dynamic_xor_decrypt(semi_cipher, "trudeau")
print(plain_text)

```


then we get: `}835994cd_d6tp0rc2d_motsuc{FTCocip`

reversing this we get the flag: `picoCTF{custom_d2cr0pt6d_dc499538}`


### incorrect tangents

got stuck for a while trying to convert the `semi_cipher` to the flag, the problem was in non-printable ascii characters which I couldn't append to a string, had to put them in string using escape sequences.

### learnings from custom encryption

1. Diffie-Hellman key exchange
2. reversing encryption functions which use XOR (XOR it again to get the original thing)



## miniRSA

After going through RSA's wikipedia page, I noticed a section mentioning the "Coppersmith's attack" which said RSA can be decrypted in cases the value of 'e' is small.

I immediately searched for tools, which can help me decrypt RSA using Copppersmith's Attack and I found one through which I got the flag.

flag: `picoCTF{n33d_a_lArg3r_e_ccaa7776}`

### incorrect tangents

None, I just needed to read the wikipedia page.

### learnings from miniRSA

1. learned how to decrypt using low-exponent attack for small values of 'e'
