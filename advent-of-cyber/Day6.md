# Day 6: If I can't find a nice malware to use, I'm not going.

Before trying anything here's what I understand.

1. We check the presence of the directory `C:\Program Files` by querying the path `HKLM\\Software\\Microsoft\\Windows\\CurrentVersion`. This directory is absent on sandboxes, and by querying this path we can find out if we are in a sandbox environment or not.

2. We can use something called YARA rules to identify suspicious stuff which will look for variables and values to look out for and determine whether to log something as true positive or not. In the case of intro given to us, The ran YARA rule logs true positives in `C:\Tools\YaraMatches.txt`

3. There's an EDR (Endpoint Detection and Response) Tool mentioned in the instructions `.\JingleBells.ps1` which will run and monitor event logs and will throw an alert in case our YARA rule find something.

4. If we run malware which tries to detect the presence of the mentioned directory the EDR will notify us.

5. Encoding a command in base64 (obsfucation) can help us get around the EDR with the YARA rule however, there are tools such as floss to detect this.

6. YARA rules can also be used to check for any activity by a program in the event logs. 

7. We can use Event Record ID value to filter for certain events in the Event Viewer



After understanding all of this I launch the VM and do as Instructed, I navigate to `C:\Tools\Malware\` and launch the `MerryChristmas.exe` file after I run The EDR in the back.

After running the malware, I get a popup with the first flag.

Then I just run `floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt` (in the right directory of course) and run `cat '.\malstrings.txt' | grep "THM"` to get the second flag (bash commands work in PS!)


