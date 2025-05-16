One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index **win_eventlogs** for further investigation.  

About the Network Information

The network is divided into three logical segments. It will help in the investigation.  

IT Department  
- James
- Moin
- Katrina

HR department
- Haroon
- Chris
- Diana

Marketing department
- Bell
- Amelia
- Deepak

### Q1: How many logs are ingested from the month of March, 2022?

In "Search & Reporting" section lets filter the results by date - March, 2022. Then let's find every event in **win_eventlogs** index.


![](pics/Pasted%20image%2020250516210430.png)


*Answer: 13959*

### Q2: Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?

To find impostor, let's look into all found user names in logs.

Query:
`index="win_eventlogs"` 
`|  table UserName` 
`|  dedup UserName`


![](pics/Pasted%20image%2020250516210641.png)


We see 2 similar names, looks like adversary wanted to impersonate Amelia

*Answer: Amel1a*

### Q3: Which user from the HR department was observed to be running scheduled tasks?

Let's find all users, who have run scheduled tasks by using the query:

`index="win_eventlogs" schtasks.exe` 
`|  table UserName`
`|  dedup UserName`


![](pics/Pasted%20image%2020250516215201.png)


We have only 4, and only Chris.fort is from HR department

*Answer: Chris.fort*

Q4: Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.

We can't use any EventID's to search for Network Activity, so let's try to find the payload itself by searching for \*.exe in CommandLine:

`index="win_eventlogs" CommandLine="*.exe"`


![](pics/Pasted%20image%2020250516215632.png)

Only one event is presented. We can confirm it's the payload we need, because it was downloaded by certutil - utility from LOLBIN list (https://lolbas-project.github.io/#)

We also can see username - haroon.

*Answer: haroon*

### Q5: To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?

We know it from previous question - certutil.exe

*Answer: certutil.exe*

### Q6: What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)

Looking into the same event:

![](pics/Pasted%20image%2020250516220203.png)

*Answer: 2022-03-04*


### Q7: Which third-party site was accessed to download the malicious payload?

Use same event to answer (see screenshot in Q6)

*Answer: controlc.com*


### Q8: What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?

Use same event to answer (see screenshot in Q6)

*Answer: benign.exe*

### Q9: The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?

First, I thought tha sample activity will be somewhere in logs. I tried searching the logs for "THM" and Base64 encoded - "", but found nothing. Then I tried searching for sample itself, but in logs we have only one event - download of benign.exe.

Other IOC that we have is the URL - hxxps[://]controlc[.]com/e4d11035. Looking into it, it seems like just a copy-pasting site for sharing. I followed the URL and found the answer.

*Answer: THM{KJ&\*H^B0}*

### Q10: What is the URL that the infected host connected to?

Use same event to answer (see screenshot in Q6)

*Answer: hxxps[://]controlc[.]com/e4d11035*

