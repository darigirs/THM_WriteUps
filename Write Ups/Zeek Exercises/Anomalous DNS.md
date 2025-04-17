### Q1: Investigate the **dns-tunneling.pcap** file. Investigate the **dns.log** file. What is the number of DNS records linked to the IPv6 address?

First, lets analyse pcap with Zeek

`zeek -r dns-tunneling.pcap`

What is the number of DNS records linked to the IPv6 address?

First i tried searching for IPv6 inside PCAP. Found only 1 address. Then I tried searching for domain types, that can be linked to IPv6 addresses (searching for AAAA domains)

`cat dns.log | zeek-cut qtype_name | grep "AAAA" | uniq -c`

![](pics/Pasted%20image%2020250417232451.png)

*Answer: 320*

### Q2: Investigate the **dns-tunneling.pcap** file. Investigate the **dns.log** file. What is the number of DNS records linked to the IPv6 address?

Sort from highest to lowest
`cat conn.log | zeek-cut duration | sort -nr | head`

![](pics/Pasted%20image%2020250417232759.png)

*Answer: 9.420791*
### Q3: Investigate the **dns.log** file. Filter all unique DNS queries. What is the number of unique domain queries?

I tried searching unique queries, but got a lot of subdomains:

`cat dns.log | zeek-cut query | sort | uniq`

![](pics/Pasted%20image%2020250417232954.png)

Using tip from THM, i filtered it out and got unique domains:

`cat dns.log | zeek-cut query |rev | cut -d '.' -f 1-2 | rev | sort | uniq`

![](pics/Pasted%20image%2020250417233042.png)

*Answer: 6*

### Q4: There are a massive amount of DNS queries sent to the same domain. This is abnormal. Let's find out which hosts are involved in this activity. Investigate the **conn.log** file. What is the IP address of the source host?

Let's look at all IP's, which were using DNS
`cat conn.log | zeek-cut id.orig_h service | grep "dns" | sort | uniq`

![](pics/Pasted%20image%2020250417233357.png)

*Answer: 10[.]20[.]57[.]3*


