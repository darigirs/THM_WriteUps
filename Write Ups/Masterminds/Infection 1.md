### Q1: Provide the victim's IP address.

Logs overview
![](pics/Pasted%20image%2020250415234041.png)

Start by identifying communicated hosts
`_path=="conn" | cut id.orig_h, id.resp_h | sort | uniq`

![](pics/Pasted%20image%2020250415233912.png)

IP 192.168.75.249  catches attention - possible victim

Answer: 192.168.75.249

### Q2: The victim attempted to make HTTP connections to two suspicious domains with the status '404 Not Found'. Provide the hosts/domains requested.

Lets search "http" log. I created this request:

`_path=="http" | id.orig_h == 192.168.75.249 and status_code == 404 | cut id.orig_h, host, status_code`

![](pics/Pasted%20image%2020250415234643.png)

Answer: cambiasuhistoria[.]growlab[.]es, www[.]letscompareonline[.]com

### Q3: The victim made a successful HTTP connection to one of the domains and received the response_body_len of 1,309 (uncompressed content size of the data transferred from the server). Provide the domain and the destination IP address.

Created request:
`_path=="http" | id.orig_h == 192.168.75.249 and response_body_len == 1309 | cut id.orig_h, id.resp_h, host, response_body_len`

![](pics/Pasted%20image%2020250415235057.png)

Answer: ww25.gocphongthe.com, 199.59.242.153

### Q4: How many unique DNS requests were made to cab[.]myfkn[.]com domain (including the capitalized domain)?

Using "Unique DNS queries tab"

![](pics/Pasted%20image%2020250415235341.png)

Answer: 7


### Q5: Provide the URI of the domain bhaktivrind[.]com that the victim reached out over HTTP.

Request
`_path=="http" | id.orig_h == 192.168.75.249 and host == "bhaktivrind[.]com" | cut id.orig_h, host, uri`

Result
![](pics/Pasted%20image%2020250415235659.png)

Answer: /cgi-bin/JBbb8/


### Q6: Provide the IP address of the malicious server and the executable that the victim downloaded from the server.

I tried File Activity section, but nothing there
![](pics/Pasted%20image%2020250416000404.png)

So i decided to research created alerts - probably something malicious was detected.

![](pics/Pasted%20image%2020250416000453.png)

I pivot alert to logs and see our victim's IP in src_ip. And some strange IP as dest_ip.
![](pics/Pasted%20image%2020250416000558.png)

Looking into http requests to this IP. Query:
`_path=="http" | id.resp_h == 185.239.243.112 | cut id.orig_h, id.resp_h, host, uri`

Result
![](pics/Pasted%20image%2020250416000724.png)

Answer: 185.239.243.112, catzx.exe

### Q7: Based on the information gathered from the second question, provide the name of the malware using VirusTotal.

Lets research cambiasuhistoria[.]growlab[.]es from Q2.

![](pics/Pasted%20image%2020250416001135.png)

Community tab has Emotet mentions
![](pics/Pasted%20image%2020250416001211.png)

Answer: Emotet