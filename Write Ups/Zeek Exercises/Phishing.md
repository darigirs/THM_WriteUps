### Q1: Investigate the logs. What is the suspicious source address? Enter your answer in **defanged format**.

First, lets analyse pcap with Zeek
`zeek -r phishing.pcap` 

Then lets take a look at unique connections in conn.log
`cat conn.log | zeek-cut id.orig_h id.resp_h | sort | uniq -c`

All connections have the same source
![](pics/Pasted%20image%2020250417234015.png)

*Answer: 10[.]6[.]27[.]102*

### Q2: Investigate the **http.log** file. Which domain address were the malicious files downloaded from? Enter your answer in defanged format.

Lets search for connections made by suspicious IP from Q1
`cat http.log | zeek-cut id.orig_h host uri | grep "10[.]6[.]27[.]102"`

We see that .exe was downloaded from smart-fax[.]com
![](pics/Pasted%20image%2020250417234537.png)

*Answer: smart-fax[.]com*

### Q3: Investigate the malicious document in VirusTotal. What kind of file is associated with the malicious document?

Too search file in VT, we need its hash. Lets find it out using hash-demo script
`zeek -C -r phishing.pcap hash-demo.zeek`

Now lets check files.log. We are looking for a document, so lets look through the unique mime types

cat files.log | zeek-cut mime_type | sort | uniq

![](pics/Pasted%20image%2020250417235304.png)

Lets check application/msword

`cat files.log | zeek-cut mime_type md5 sha1 sha256 | grep "application/msword" | sort | uniq`

![](pics/Pasted%20image%2020250417235435.png)

Now we can search hashes on VT. On Relations tab we see, that document is associated with VBA files

![](pics/Pasted%20image%2020250417235638.png)

*Answer: VBA*

### Q4: Investigate the extracted malicious **.exe** file. What is the given file name in Virustotal?

Lets search for exe file in files.log
cat files.log | zeek-cut mime_type md5 sha1 sha256 | grep "exe"

![](pics/Pasted%20image%2020250418000354.png)

Now we can search for hashes on VT. To see names we can use "Details" tab
![](pics/Pasted%20image%2020250418000534.png)

*Answer: PleaseWaitWindow.exe*


### Q5: Investigate the malicious **.exe** file in VirusTotal. What is the contacted domain name? Enter your answer in **defanged format**.

On the relations tab, in Contacted domains list we see one suspicious sub-domain![](pics/Pasted%20image%2020250418000832.png)

*Answer: hopto[.]org*

### Q6: Investigate the http.log file. What is the request name of the downloaded malicious **.exe** file?

We've seen it earlier, in Q2 research:
![](pics/Pasted%20image%2020250417234537.png)

*Answer: knr.exe*


