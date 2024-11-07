# picoCTF: Forensics

_for picoCTF Forensics challenges_

## m00nwalk

On checking both the hints and searching for a connection between "Scottie" and "Radio Signals", I found out about the SSTV encoding. The given audio file was just a encoded audio signal in Scottie 1 mode.

I tried searching for SSTV decoding software online and came across this project on github to decode SSTV signals: `https://github.com/colaclanth/sstv`

After setting up the tool, I decoded the `message.wav` file by:-

```
sstv -d message.wav -o result.png
```

which gave me `result.png` and the flag which was an image.

flag: ![m00nwalk flag](./images/m00nwalk_flag.png)


### incorrect tangents

Not quite incorrect but I did try to analyse the file using Audacity, on checking the spectrograph I just noticed again that it's a pulsating signal but the approach was a dead-end.

On learning that the signal is SSTV encoded, I installed `qsstv` software however I couldn't decode the file using it since it's probably not made for decoding.

### learnings from m00nwalk

1. got to know about SSTV
2. learned how to decode SSTV audio files to get images
