# Level 5 --> 6

To get the password for the next level, we have to search for a file in `inhere` directory which is:-

1. Human Readable
2. 1033 bytes in size
3. not executable


I `cd` into `inhere` and see a bunch of directories. `cd`ing to each directory then finding would be slow but luckily I remember the days I learnt about `man` and looked up `man ls`.

`-R` flag can be used alongside to list files recursively.

I run `~$ ls -Ral | grep 1033` and see:-

```
-rw-r-----  1 root bandit5 1033 Sep 19 07:08 .file2
```

the problem now was I didn't know which directory `.file2` would be in, so I just lazily ran `ls -Ral` again (without `grep`ing this time) and just scrolled until I saw `1033` (only one file of this size exists)

found it, `cat`ed it. Got: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

Got the password for the next level, now moving on.
