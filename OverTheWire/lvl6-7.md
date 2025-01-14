# Level 6 --> 7

This time the password file was somewhere in the server and we had to find a file with attributes:-

1. owned by user bandit7
2. owned by group bandit6
3. is of 33 bytes in size

I knew that a `ls -al` will give me all of this information but the problem was this time I didn't know where to look.

So I `cd /` and run `ls -Ral | grep bandit6 | grep bandit7 | grep 33` (after learning a thing or two about chaining `grep` commands ofc)

and got this (after neglecting a sea of permission denied errors):-

```
-rw-r----- 1 bandit7 bandit6      33 Sep 19 07:08 bandit7.password
```

now I ran `find / -name bandit7.password 2> /dev/null | grep -v "find"` (I totally wrote this command correctly in the 1st try and defo didn't forget that the sea of permission denied errors was getting output to stderr and not stdout and I also defo didn't make the mistake of putting the `2>` part after the pipe; along with `grep` instead of putting it before with `find`)

and got: `/var/lib/dpkg/info/bandit7.password`

ran `cat /var/lib/dpkg/info/bandit7.password` and got the password: ``morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

Got the password for the next level, now moving on.
