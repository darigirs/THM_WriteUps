**An alert has been triggered:** "A user came across a poor file index, and their curiosity led to problems".

The case was assigned to you. Inspect the provided **directory-curiosity.pcap** located in `~/Desktop/exercise-files` and retrieve the artefacts to confirm that this alert is a true positive.

**Your tools:** TShark, VirusTotal.  

Answer the questions below

Investigate the DNS queries.  
Investigate the domains by using VirusTotal.  
According to VirusTotal, there is a domain marked as malicious/suspicious.

### Q1: What is the name of the malicious/suspicious domain?

Enter your answer in a defanged format.

First search for unique DNS queries


![](pics/Pasted%20image%2020250427023948.png)


Checking domains at VT, we finally found malicious one


![](pics/Pasted%20image%2020250427024001.png)


*Answer: jx2-bavuong[.]com*

### Q2: What is the total number of HTTP requests sent to the malicious domain?

Search for every request to malicious domain and count lines


![](pics/Pasted%20image%2020250427024332.png)


*Answer: 14*


### Q3: What is the IP address associated with the malicious domain?

Enter your answer in a defanged format.

We can see IP from previous filter output

*Answer: 141[.]164[.]41[.]174*

### Q4: What is the server info of the suspicious domain?
Let's search for http.server field, filtering packets by malicious domain's IP (source and destination)

![](pics/Pasted%20image%2020250427024845.png)

*Answer: Apache/2.2.11 (Win32) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8i PHP/5.2.9*

### Q5: Follow the "first TCP stream" in "ASCII". Investigate the output carefully. What is the number of listed files?

First, we follow TCP stream. Streams start from "0", so to follow the first one we use "0" option.
`tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q`

We see 3 different files in output:


![](pics/Pasted%20image%2020250427025352.png)


*Answer: 3*

### Q6: What is the filename of the first file?

Enter your answer in a defanged format.

We can see filenames from previous command output.

*Answer: 123[.]php*


### Q7: Export all HTTP traffic objects. What is the name of the downloaded executable file?

Enter your answer in a defanged format.

First we extract all the objects from HTTP traffic, using `--extract-objects` option:

`tshark -r directory-curiosity.pcap --export-objects http,/home/ubuntu/Desktop/exercise-files/extracted -q`

Then we look through extracted objects list:

![](pics/Pasted%20image%2020250427025834.png)

*Answer: vlauto[.]exe*

### Q8: What is the SHA256 value of the malicious file?

Calculate SHA256 using `sha256sum`:


![](pics/Pasted%20image%2020250427025935.png)


*Answer: b4851333efaf399889456f78eac0fd532e9d8791b23a86a19402c1164aed20de*

### Q9: Search the SHA256 value of the file on VirtusTotal. What is the "PEiD packer" value?

We can find it on "Details" tab:
![](pics/Pasted%20image%2020250427030121.png)

*Answer: .NET executable*

### Q10: Search the SHA256 value of the file on VirtusTotal. What does the "Lastline Sandbox" flag this as?

We can find it on "Behavior" tab:

![](pics/Pasted%20image%2020250427030354.png)

*Answer: MALWARE TROJAN*