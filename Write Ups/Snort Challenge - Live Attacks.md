# Scenario 1 | Brute-Force

Lets start with sniffing the traffic.


![](pics/Pasted%20image%2020250415002939.png)

With closer look we can see some SSH traffic (using HEX and multiple connections to server SSH port)


![](pics/Pasted%20image%2020250415002833.png)


Lets investigate SSH incoming traffic more. Add rule
`alert tcp any any -> any 22 (msg:"Possible SSH brute"; sid:1000001; rev:1;)`

to local.rules

![](pics/Pasted%20image%2020250415003544.png)
![](pics/Pasted%20image%2020250415003518.png)

When we run it, we have some alerts to investigate

![](pics/Pasted%20image%2020250415004005.png)

Looking into `alert` file, we see, that a lot of traffic is coming from IP *10.10.245.36* with strange port numbers. Possible attacker?


![](pics/Pasted%20image%2020250415004136.png)

Lets try blocking this IP. Change our local.rules file like this:


![](pics/Pasted%20image%2020250415004435.png)

We added blocking rule for particular IP
`reject tcp 10.10.245.36 any -> any any (msg:"Block attacker"; sid:1000001; rev:1;)`

Lets see it in action

![](pics/Pasted%20image%2020250415004713.png)

We see some alerts! 

![](pics/Pasted%20image%2020250415004831.png)

Now lets run in IPS mode...

![](pics/Pasted%20image%2020250415005137.png)

And after a moment we get a flag

![](pics/Pasted%20image%2020250415010551.png)

Flag: *THM{81b7fef657f8aaa6e4e200d616738254}*

# Scenario 2 | Reverse-Shell

Lets start snort sniffer again. 
`sudo snort -X`

We can clearly see suspicious activity:

![](pics/Pasted%20image%2020250415011236.png)

Connection to port 4444: default Meterpreter port
Strings with root-<service_IP>

We can already stop the attack by just blocking any traffic from 4444 port. But lets investigate a bit more, to detect the attacker. Add alert rule to local.rules
`alert tcp any 4444 <> any any (msg:"Meterpreter connection"; sid:1000001; rev:1;)`

Looking into alert file, we can see that possible attacker is IP 10.10.196.55

![](pics/Pasted%20image%2020250415012245.png)

Lets change our rule to block traffic to and from this IP
`drop tcp 10.10.196.55 any <> any any (msg:"Meterpreter blocked"; sid:1000001; rev:1;)`

Lets test the rule


![](pics/Pasted%20image%2020250415012544.png)
![](pics/Pasted%20image%2020250415012615.png)

Now running snort in IPS mode...

 `snort -c /etc/snort/rules/local.rules -q -Q --daq afpacket -i eth0:eth1 -A full -l /var/log/snort/`


And after a minute..


![](pics/Pasted%20image%2020250415022954.png)

Flag: *THM{0ead8c494861079b1b74ec2380d2cd24}*
