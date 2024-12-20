#  Day 4: I’m all atomic inside!


The problem statement was:-


```
Your task is to identify the correct atomic test to run that will take advantage of a command and scripting interpreter, conduct the test, and extract valuable artefacts that would be used to craft a detection rule.
```

for the first question I cleared the event logs, and ran `Invoke-AtomicTest T1566.001 -TestNumbers 1` and went to `C:\Users\Administrator\AppData\Local\Temp\` to look at the flag stored next to the Phishing file.

After that I googled, `MITRE ATT&CK Framework command and scripting interpreter` and got the relevant ID: `T1059`, also googled the framework ID which focuses on Windows Command Shell and got `T1059.003` as the relevant subtechnique ID.

On fetching the details of this particular technique using `-ShowDetails`, I looked at the hint asking me to 
find the testcase with regards to Malware and I found `Simulate BlackByte Ransomware Print Bombing` and the name 
of the file being used in the test was `Wareville_Ransomware.txt`

I ran this particular atomic test and saved the resulting .pdf file in Desktop directory, I opened the PDF file and got the flag: `THM{R2xpdGNoIGlzIG5vdCB0aGUgZW5lbXk=}`

