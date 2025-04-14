A multinational technology company has been the target of several cyber attacks in the past few months. The attackers have been successful in stealing sensitive intellectual property and causing disruptions to the company's operations. A threat advisory report about similar attacks has been shared, and as a CTI analyst, your task is to identify the Tactics, Techniques, and Procedures (TTPs) being used by the Threat group and gather as much information as possible about their identity and motive. For this task, you will utilise the OpenCTI platform as well as the MITRE ATT&CK navigator, linked to the details below.
# Answer the questions below

## What kind of phishing campaign does APT X use as part of their TTPs?
Spear-phishing emails (based on attached report)
![[Pasted image 20250405204008.png]]

## What is the name of the malware used by APT X?  
USBferry (based on attached report)
![[Pasted image 20250405204031.png]]

## What is the malware's STIX ID?

malware--5d0ea014-1ce9-5d5c-bcc7-f625a07907d0 (based on OpenCTI search)
![[Pasted image 20250405204116.png]]
![[Pasted image 20250405204133.png]]

## With the use of a USB, what technique did APT X use for initial access?  
Replication through removable media
(based on group info in ATTACK navigator)
![[Pasted image 20250405204247.png]]
## What is the identity of APT X?   
Tropic Trooper (based on USBferry description - https://attack.mitre.org/software/S0452/)
![[Pasted image 20250405204358.png]]

## On OpenCTI, how many Attack Pattern techniques are associated with the APT?  
39 (based on OpenCTI search)
![[Pasted image 20250405204458.png]]
![[Pasted image 20250405204520.png]]


## What is the name of the tool linked to the APT?

BITSAdmin (based on OpenCTI search)
![[Pasted image 20250405204549.png]]
## Load up the Navigator. What is the sub-technique used by the APT under Valid Accounts?
Local Accounts
![[Pasted image 20250405204635.png]]

## Under what Tactics does the technique above fall?
Initial Access, Persistence,  Defense Evasion and Privilege Escalation (in kill chain order, based on mitre sub-technique description)
![[Pasted image 20250405205226.png]]

## What technique is the group known for using under the tactic Collection?
Automated Collection
![[Pasted image 20250405205252.png]]