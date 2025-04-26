An alert has been triggered: "The threat research team discovered a suspicious domain that could be a potential threat to the organisation."

The case was assigned to you. Inspect the provided **teamwork.pcap** located in `~/Desktop/exercise-files` and create artefacts for detection tooling.

**Your tools:** TShark, VirusTotal.

Answer the questions below

Investigate the contacted domains.  
Investigate the domains by using VirusTotal.  
According to VirusTotal, there is a domain marked as malicious/suspicious.  
  
### Q1: What is the full URL of the malicious/suspicious domain address?

Enter your answer in defanged format.

First, investigate what protocols were used


![](pics/Pasted%20image%2020250427015709.png)


We see some http frames, lets investigate http hosts


![](pics/Pasted%20image%2020250427015913.png)


We found 2 domains, next we need to investigate it in VT. 
First one is marked as malicious. Defanged domain name is the answer.


![](pics/Pasted%20image%2020250427020750.png)


*Answer: www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com*


### Q2: When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?

Investigate URL for found domain


![](pics/Pasted%20image%2020250427020718.png)


*Answer: 2017-04-17 22:52:53 UTC*


### Q3: Which known service was the domain trying to impersonate?

Looking at subdomains, we can see service name:
www[.]**paypal**[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com

*Answer: paypal*


### Q4: What is the IP address of the malicious domain?

Enter your answer in defanged format.
We can find answer in VT domain report (from Q1)


![](pics/Pasted%20image%2020250427020956.png)


*Answer: 184[.]154[.]127[.]226*


### Q5: What is the email address that was used?

Enter your answer in defanged format. (**format:** aaa[at]bbb[.]ccc)

Looking through the frames, where interactions with malicious domain appear, we can see frame 202. 


![](pics/Pasted%20image%2020250427023015.png)


Probably we can find e-mail here, when POST method for /inc/login.php uri was used.
`tshark -r teamwork.pcap -x -Y 'frame.number == 202'`


![](pics/Pasted%20image%2020250427022239.png)


We have an e-mail, let's decode it...


![](pics/Pasted%20image%2020250427022424.png)


Then defang as mentioned in task, and we have the answer.

*Answer: johnny5alive[at]gmail[.]com*