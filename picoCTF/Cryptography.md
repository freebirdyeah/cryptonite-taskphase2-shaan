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
 
