# Additional Forensics Challenges

## Blast from the past

Wow, this was annoying. I first used `nano` to edit whatever I could see and it also took me a foolishly long time to understand how the file submission worked in this challenge but in the end I figured it out thanks to: `https://stackoverflow.com/questions/78185037/how-to-edit-the-samsung-trailer-tag-timestamp`


I opened my hex editor and went edited the bytes denoting date/time and also those `703` bytes denoting milliseconds (which I figured out by playing with the file's metadata for too long and checking exiftool output), after that the main issue was with the Samsung timestamp:-

```

Checking tag 7/7
Timezones do not have to match, as long as it's the equivalent time.
Looking at Samsung: TimeStamp
Looking for '1970:01:01 00:00:00.001+00:00'
Found: 2023:11:20 20:46:21.420+00:00
Oops! That tag isn't right. Please try again.
```

I figured it out with the help of given link and I just had to replace the time-since-epoch with 1ms to get the flag.

flag: `picoCTF{71m3_7r4v311ng_p1c7ur3_3e336564}`


