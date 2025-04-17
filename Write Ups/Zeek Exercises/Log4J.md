### Q1: Investigate the log4shell.pcapng file with detection-log4j.zeek script. Investigate the signature.log file. What is the number of signature hits?

Lets investigate using detection-log4j.zeek script
`zeek -C -r log4shell.pcapng detection-log4j.zeek` 

And research different signatures

`cat signatures.log | zeek-cut uid | sort | uniq -c`

![](pics/Pasted%20image%2020250418001655.png)

*Answer: 3*

### Q2: Investigate the **http.log** file. Which tool is used for scanning?

Researching http.log, we see some interesting values in user_agent filed

![](pics/Pasted%20image%2020250418002116.png)

Lets see all unique user agents:
`cat http.log | zeek-cut user_agent | sort | uniq`

And here's the tool we are looking for
![](pics/Pasted%20image%2020250418002230.png)

*Answer: nmap*

### Q3: Investigate the **http.log** file. What is the extension of the exploit file?
Let's look through uri values to find exploits
`cat http.log | zeek-cut uri | sort | uniq`

![](pics/Pasted%20image%2020250418002445.png)

*Answer: class*

### Q4: Investigate the log4j.log file. Decode the base64 commands. What is the name of the created file?

Lets see all unique commands
`cat log4j.log | zeek-cut uri | sort | uniq`

![](pics/Pasted%20image%2020250418002629.png)

Now we decode them
`echo <base64_encoded_string> | base64 -d`

![](pics/Pasted%20image%2020250418003018.png)

*Answer: pwned*