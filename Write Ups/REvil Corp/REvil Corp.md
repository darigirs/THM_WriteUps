**Scenario**: One of the employees at Lockman Group gave an IT department the call; the user is frustrated and mentioned that all of his files are renamed to a weird file extension that he has never seen before. After looking at the user's workstation, the IT guy already knew what was going on and transferred the case to the Incident Response team for further investigation.

### Q1: What is the compromised employee's full name?


![](pics/Pasted%20image%2020250529015815.png)



*Answer: John Coleman*
### Q2: What is the operating system of the compromised host?


![](pics/Pasted%20image%2020250529015926.png)


*Answer: Windows 7 Home Premium 7601 Service Pack 1*
### Q3: What is the name of the malicious executable that the user opened?


![](pics/Pasted%20image%2020250529020727.png)

*Answer: WinRAR2021.exe*
### Q4: What is the full URL that the user visited to download the malicious binary? (include the binary as well)


![](pics/Pasted%20image%2020250529020809.png)

*Answer: hxxp[://]192[.]168[.]75[.]129:4748/Documents/WinRAR2021[.]exe*
### Q5: What is the MD5 hash of the binary?


![](pics/Pasted%20image%2020250529020950.png)

*Answer: 890a58f200dfff23165df9e1b088e58f*
### Q6: What is the size of the binary in kilobytes?


![](pics/Pasted%20image%2020250529021043.png)

*Answer: 164 *
### Q7: What is the extension to which the user's files got renamed?


![](pics/Pasted%20image%2020250529021317.png)

*Answer: .t48s39la*
### Q8: What is the number of files that got renamed and changed to that extension?

![](pics/Pasted%20image%2020250530003350.png)

I searched for every file with .t48s39la extension presented in user system (c: drive) and i got 24 matches. But this answer was wrong. I was curious and used provided Tip, which led me to Timeline section, where I could search for extension in Summary of changes. The answer 48 seems right for THM, but for me it counts number of changes, not files (looking closely you can see duplicating MD5 in matched Summary).

![](pics/Pasted%20image%2020250530003137.png)

*Answer: ~~24~~ 48*
### Q9: What is the full path to the wallpaper that got changed by an attacker, including the image name?

REvil Ransomware saves wallpaper as .bmp files in Temp folder. (https://www.secureworks.com/research/revil-sodinokibi-ransomware)

We can find 2 .bmp files in Temp folder. And one of them created right after files' extensions were changed

![](pics/Pasted%20image%2020250530010823.png)


*Answer: C:\Users\John Coleman\AppData\Local\Temp\hk8.bmp*
### Q10: The attacker left a note for the user on the Desktop; provide the name of the note with the extension.


![](pics/Pasted%20image%2020250530010952.png)

*Answer: t48s39la-readme.txt*
### Q11: The attacker created a folder "Links for United States" under C:\Users\John Coleman\Favorites\ and left a file there. Provide the name of the file.


![](pics/Pasted%20image%2020250530011334.png)

*Answer: GobiernoUSA.gov.url.t48s39la*
### Q12: There is a hidden file that was created on the user's Desktop that has 0 bytes. Provide the name of the hidden file.

I filtered out only 0 bytes files on user's desktop

![](pics/Pasted%20image%2020250530011446.png)

*Answer: d60dff40.lock*
### Q13: The user downloaded a decryptor hoping to decrypt all the files, but he failed. Provide the MD5 hash of the decryptor file.


![](pics/Pasted%20image%2020250530011643.png)

![](pics/Pasted%20image%2020250530011928.png)

*Answer: f617af8c0d276682fdf528bb3e72560b*
### Q14: In the ransomware note, the attacker provided a URL that is accessible through the normal browser in order to decrypt one of the encrypted files for free. The user attempted to visit it. Provide the full URL path.

Let's check browser activity. Sort events by Last Visit date, so the most recent ones are on top (obviously we need to look at events, that happened after files were encrypted). At first we see a lot of bing/torproject requests, but one domain visited after encryption looks different.

![](pics/Pasted%20image%2020250530012441.png)

*Answer: hxxp[://]decryptor[.]top/644E7C8EFA02FBB7*
### Q15: What are some three names associated with the malware which infected this host? (enter the names in alphabetical order)

I looked up the hash of decryptor on Virus Total and found all names in tags


![](pics/Pasted%20image%2020250530012908.png)

*Answer: REvil,Sodin,Sodinokibi*