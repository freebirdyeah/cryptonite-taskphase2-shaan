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


## Mob psycho

I was given a `.apk` I file, I downloaded it and looked at the first hint which said: `you can unzip .apk files`, on running `unzip` I got a bunch of files and I ran `ls -Ral | grep flag` and I got output saying some `flag.txt` file existed in those files.


I ran `find ~ -name flag.txt` and I got the path to the `flag.txt` file. However on `cat`ing it I realised it was encrypted.

I went over to my trusty `dcoder cipher identifer tool` and decoded it (was in hex) and got the flag.

flag: `picoCTF{ax8mC0RU6ve_NX85l4ax8mCl_a3eb5ac2}`


## PcapPoisoning

I checked the Export Objects section but there was nothing to be extracted from there, On going through the 
packets, I noticed the 3rd packet had the data 'flag is close' I initially thought `gc2VjcmV0OiBwaWNvQ1RGe` to 
be the encoded flag as it was in every packet when I filtered using 'FTP-DATA' but on going through each and 
every packet quickly I noticed one in the end which had the flag.

flag: `picoCTF{P64P_4N4L7S1S_SU55355FUL_4624a8b6}`


## hideme

I couldn't upload the image into Aperisolver but no matter, I researched on basic use of `zsteg` and `binwalk` and ran `zsetg -a flag.png` and got:-

```

[?] 3266 bytes of extra data after image end (IEND), offset = 0x9b3b
extradata:0         .. file: Zip archive data, at least v1.0 to extract, compression method=store
    00000000: 50 4b 03 04 0a 00 00 00  00 00 3b 10 70 56 00 00  |PK........;.pV..|
    00000010: 00 00 00 00 00 00 00 00  00 00 07 00 1c 00 73 65  |..............se|
    00000020: 63 72 65 74 2f 55 54 09  00 03 91 78 12 64 91 78  |cret/UT....x.d.x|
    00000030: 12 64 75 78 0b 00 01 04  00 00 00 00 04 00 00 00  |.dux............|
    00000040: 00 50 4b 03 04 14 00 00  00 08 00 3b 10 70 56 9e  |.PK........;.pV.|
    00000050: b1 e9 3a 80 0b 00 00 17  0c 00 00 0f 00 1c 00 73  |..:............s|
    00000060: 65 63 72 65 74 2f 66 6c  61 67 2e 70 6e 67 55 54  |ecret/flag.pngUT|
    00000070: 09 00 03 91 78 12 64 91  78 12 64 75 78 0b 00 01  |....x.d.x.dux...|
    00000080: 04 00 00 00 00 04 00 00  00 00 cd 56 69 34 d4 7f  |...........Vi4..|
    00000090: 17 ff 69 15 61 0a 33 a5  b2 0c 26 bb 59 50 cd d8  |..i.a.3...&.YP..|
    000000a0: b3 45 b6 19 86 30 0c 63  2d 83 18 7b 7f 45 24 19  |.E...0.c-..{.E$.|
    000000b0: fc 25 24 6b 4a 1a b2 65  f9 47 06 d5 10 29 14 c6  |.%$kJ..e.G...)..|
    000000c0: 56 23 64 9b 21 5b 93 2d  3c f3 bc 7a ce f3 e2 79  |V#d.![.-<..z...y|
    000000d0: ff dc 17 f7 73 d7 73 be  e7 7b ef b9 e7 93 64 63  |....s.s..{....dc|
    000000e0: 65 2a 24 20 21 00 00 80  d0 25 33 23 2c 00 ec 73  |e*$ !....%3#,..s|
    000000f0: e4 d9 48 10 4f 01 5c 32  0c cf 83 03 3e 06 96 06  |..H.O.\2....>...|

```

I saw `secret/flag.png` leading me to suspect there's a file inside the file. I then ran `binwalk -e flag.png` and got:-

```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 512 x 504, 8-bit/color RGBA, non-interlaced
41            0x29            Zlib compressed data, compressed
39739         0x9B3B          Zip archive data, at least v1.0 to extract, name: secret/
39804         0x9B7C          Zip archive data, at least v2.0 to extract, compressed size: 2944, uncompressed size: 3095, name: secret/flag.png
42983         0xA7E7          End of Zip archive, footer length: 22
```

I was then very suprised to find that simple `unzip flag.png` worked, it produced a folder `secret` I `cd`ed 
into it and viewed the flag image.


flag: `picoCTF{Hiddinng_An_imag3_within_@n_imag9_ad9f6587}`

## st3g0

A simple check using `zsteg -a pico.flag.png` was sufficient:-

```
b1,rgb,lsb,xy       .. text: "picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}$t3g0"

```

flag: `picoCTF{7h3r3_15_n0_5p00n_96ae0ac1}`


## Sleuthkit Intro

I download the `disk.img.gz` file and unzipped it using, `gzip -d disk.img.gz` and then as per the instructions ran `mmls disk.img` and got:-


```
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)
```

and the submitted the length of the Linux Partition, to `nc saturn.picoctf.net 58931` and got the flag.

flag: `picoCTF{mm15_f7w!}`


## Lookey here

Very easy: `cat anthem.flag.txt | grep pico`

flag: `picoCTF{gr3p_15_@w3s0m3_58f5c024}`


## m00nwalk 2

Pretty easy as well but this time however there was the annoyance of figuring the password from the image. I did what I did in the OG m00nwalk challenge and used the `sstv` program I downloaded from GitHub to decode the file, it said "there is a hidden message" so I tried using `binwalk` and `zsteg` on the resultant image but found nothing.

I installed the clues audio files and decoded them and the first clue was sufficient, I then used the password given along with `steghide` on the `message.wav` file to get the flag.

```
~$ steghide extract -sf message.wav --passphrase "hidden_stegosaurus"
wrote extracted data to "steganopayload12154.txt".
```

got the flag on `cat`ing it. flag: `picoCTF{the_answer_lies_hidden_in_plain_sight}`

## So Meta

just run exiftool on the image.

flag: `picoCTF{s0_m3ta_fec06741}`

## Matryoshka doll

Just by the name I knew what to do, I ran `binwalk -e dolls.jpg` and it created a directory called `base_images` when I unzipped the image.

I kept `cd`ing into every folder and unzipped every image I got and at last I got `flag.txt`

flag: `picoCTF{96fac089316e094d41ea046900197662}`


## Wireshark doo dooo do doo...

Opened the `.pcapng` file and went to `Export Objects > HTTP` and saw a bunch of files there and saved them all.

I ran `file *` to see the filetype of every file I saved and saw all of them as data except 2 were ascii text one of them being `%2f`, I `cat`ed the file and saw the flag encrypted in Caesar Cipher, decrypted it and got the flag.

flag: `picoCTF{p33kab00_1_s33_u_deadbeef}`


## Enhance!

Weird challenge, file didn't open with `feh` and `zsteg` and `binwalk` revealed nothing. I just `cat`ed it and saw this:-


```

id="tspan3748">p </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.08942"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3754">i </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.09383"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3756">c </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.09824"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3758">o </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.10265"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3760">C </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.10706"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3762">T </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.11147"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3764">F { 3 n h 4 n </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.11588"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3752">c 3 d _ a a b 7 2 9 d d }</tspan></text>
```


flag: `picoCTF{3nh4nc3d_aab729dd}`
