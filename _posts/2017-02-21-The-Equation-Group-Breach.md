--- 
layout: post
title: "The Equation Group Breach"
date: 2017-02-21
author: Alexander Oleinik, Brian Roach, Jennifer Tsui, and Karan Varindani
categories: education security
excerpt: Report on the breach of The Equation Group by The Shadow Brokers, and a look at the EXTRABACON exploit.
permalink: /equation
---
Report on the breach of The Equation Group by The Shadow Brokers, and a look at the _EXTRABACON_ exploit. Prepared by `Alexander Oleinik, Brian Roach, Jennifer Tsui, and Karan Varindani` for `Boston University, CS558 Network Security (February 2017)`. This work was done without any outside collaboration.


## Introduction
In August of 2016, a hacking & possible political interest group codenamed The Shadow Brokers breached The Equation Group, likely a subdivision of the NSA’s elite cybersecurity unit, and gained a collection of exploits used to predominantly target network hardware, such as the Cisco ASA series firewalls. The Shadow Brokers attempted to auction off these exploits, and released a small sample of them to prove their authenticity. One such sample was named _EXTRABACON_, which allows an adversary to disable password authentication on Cisco ASA series firewalls through an SNMP request, and then allows the adversary to enter and ‘own’ the firewall. 

We will discuss the parties involved in this breach, the motivations of the Shadow Brokers, the media coverage surrounding the event, and the _EXTRABACON_ exploit (and how to prevent it).

### The Players
The two players involved in this event were The Shadow Brokers and The Equation Group. They are both unknown threat actors, but there are suspicions as to their identities. The Equation Group was discovered by Kaspersky Labs in February 2015.[^1] They claimed that the group was tied to Stuxnet and Flame, which were both associated with cyberattacks launched by the United States.[^2] Some think that the group is associated with the NSA. Nicholas Weaver in particular claimed that “Because of the sheer volume and quality, it is overwhelmingly likely that this data is authentic, and it does not appear to be information taken from compromised targets. Instead, the exploits, binaries with help strings, server configuration scripts, 5 separate versions of one implant framework, and all sort of other features indicate that this is analyst-side code—the kind that probably never leaves the NSA.”[^3] Some think that the Equation Group is a part of the Tailored Access Operations (TAO) division, which is a cyber-warfare intelligence-gathering subunit of the NSA.[^4] [^5] David Sanger of the New York Times said, “According to these experts, the coding resembled a series of ‘products’ developed inside the NSA’s highly classified Tailored Access Operations unit, some of which were described in general terms in documents stolen three years ago by Edward J. Snowden, the former NSA contractor now living in Russia.” While the Snowden documents confirmed that some of the exploits the Equation Group had were indeed once the property of the NSA, many are still reluctant to say that the Equation Group is actually the NSA. 

The Shadow Brokers are an anonymous party who were selling exploits gained from supposedly breaching the Equation Group, and they were being sold for 1 million bitcoins (approximately 500 million USD at time of sale). Some believe that the Shadow Brokers are a foreign political adversary, such as Snowden, who believes that they are the Russian Government.[^6] Others believe that it could be an NSA insider, such as the federal cybersecurity contractor Harold T. Martin III, who was found to have smuggled several terabytes of classified information from the NSA.[^7] 

Additionally, an argument is made that the statements made by the Shadow Brokers were written by a native English speaker attempting to fake non native speech patterns, which would cast doubt on Snowden’s theory.[^8] Ultimately, the identify of both The Equation Group and The Shadow Brokers is not known. In regards to The Shadow Brokers, their true identity likely will not be discovered as they have since ceased operation.

### Motivation of The Shadow Brokers
Many individuals thought the timing with which The Shadow Brokers leaked the exploits to be highly coincidental; around the same time, DNC emails were being leaked by a person named Guccifer 2.0.[^9] Edward Snowden has claimed the following about the Equation Group hack on Twitter (with the assumption that the Russian government is behind the Shadow Brokers):

“This leak is likely a warning that someone can prove US responsibility for any attacks that originated from this malware server. That could have significant foreign policy consequences. Particularly if any of those operations targeted US allies, particularly if any of those operations targeted elections. Accordingly, this may be an effort to influence the calculus of decision-makers wondering how sharply to respond to the DNC hacks. TL;DR: This leak looks like a somebody sending a message that an escalation in the attribution game could get messy fast.”[^10]

To summarize, Snowden believes that the Russian government was sending a warning to the US; If the US were to publicly blame Russia for the DNC, “it will retaliate by leaking potentially damaging information about US cyber-intelligence operations to the world."[^11] However, this may have simply been a ‘hack back’; In the past, the Shadow Brokers could have been hacked by The Equation Group, so they figured they would hack them back.

## Press Coverage
The breach of the Equation Group by The Shadow Brokers caught the attention of the mainstream media because of the likely possibility that the NSA’s exploits had been stolen and partially released to the public. The obvious concern that a federal agency, tasked with upholding national security, had been compromised gave way to a plethora of headlines, from major to smaller news outlets alike. _The New York Times_, _Business Insider_, _The Wall Street Journal_, _Bloomberg_, and more covered the topic, with headlines like “‘Shadow Brokers’ Leak Raises Alarming Question: Was the NSA Hacked?” from _The New York Times_, and “‘Shadow Brokers’ claim to have hacked an NSA-linked elite computer security unit” from _Business Insider_.[^12] [^13] In addition to attention from the mainstream media, the technical media covered how this attack may have transpired, as well as detailing how the leaked exploits operated and the threats they posed to the public. _The Hacker News_ detailed the authenticity of the leak, specifically _EXTRABACON_, and confirmed that Cisco was aware of the holes in its ASA firewall security and was patching the issue.[^14] Additionally, sources like _Ars Techinca_ covered the event from a technical perspective.

## The _EXTRABACON_ Exploit
While The Shadow Brokers were asking for 1 million bitcoins to release all the exploits, they shared an archive containing several Equation Group tools for free in as a proof-of-concept. One of these tools was the _EXTRABACON_ exploit.

### How it works
_EXTRABACON_ (EXBA) leverages a buffer overflow in Cisco’s SNMP implementation on the Adaptive Security Appliance (ASA) software. According to Cisco, “An exploit could allow the attacker to execute arbitrary code and obtain full control of the system or to cause a reload of the affected system”.[^15] Firewalls are commonly positioned at the border of a network and handle traffic flowing in and out of the network. Cisco’s ASA performs functions at every layer in the OSI network model. The ASAs are able to process 40Gbps of traffic and can handle 15000 simultaneous VPN connections.[^16] A compromise can entail a catastrophic compromise of privacy and network integrity affecting many connections. 

The ASA provides many services to facilitate network administration. EXBA targets two of these - Simple Network Management Protocol (SNMP) and Secure Shell (SSH). SNMP is generally run on UDP port 161 and is widely used for collecting and monitoring hardware statistics. An SNMP client sends a request containing an Object Identifier (OID) and a community string to the server and receives a response containing a value corresponding to the OID. The community string acts as a very basic form of security - in SNMP versions prior to 3, it is transmitted as plaintext and susceptible to man-in-the-middle (MITM) attacks.[^17] SNMP security is widely neglected as many administrators wrongfully assume that no harm can be done to the hardware with access to read-only SNMP. 

SSH, on the other hand, is the primary protocol used by network engineers to perform configuration and administration tasks on hardware.[^18] The SSH server on ASAs supports several ciphersuites such as DHE-RSA-AES128-SHA and DES-CBC3-SHA.[^19] Admins generally understand the importance of using strong keys/passwords to prevent unauthorized access via SSH. 

EXBA assumes that the attacker has obtained the SNMP community string by MITM, or guessing a common default such “public”. Additionally the attacker must have access to the ASA on an SNMP-enabled interface. EXBA works by sending an SNMP request with OID 1.3.6.1.2.1.1.1.0, to which the firewall responds with its version string. With this information, EXBA crafts and sends a malicious version-specific OID. Upon processing this OID, a buffer overflow occurs on the ASA. The OID is constructed in such a way that when the overflow occurs, the ASA calls sys\_mprotect to disable protection on the memory address responsible for enabling/disabling password authentication. The contents of the address are changed to disable authentication before returning to a safe address. 

With the attack executed, SSH connections are no longer authenticated and the attacker has access to install persistent malware, tamper with configuration and eavesdrop on the firewall.

![The attack on the ASA's SNMP handler occurs within one request](/assets/extrabacon-attack.jpeg "EXTRABACON Attack"){:class="img-responsive"}

### The targets
The Shadow Brokers uploaded encrypted archives of malware to several file-sharing services including mega.co.nz and Yandex disk. They provided decryption keys for one “sample archive” containing exploits including _BANANAUSURPER_, _BLATSTING_, and _EXTRABACON_. All of these exploits target enterprise firewalls. Most internet traffic passes through one of these devices at the business border, or ISP level. Due to the throughput, routing, and packet inspection capabilities of enterprise firewalls, a single compromise has potential to affect tens of thousands of users.[^20] The exploits target a wide range of hardware manufacturers. For example, _ELIGIBLEBOMBSHELL_, _ELIGIBLECANDIDATE_, and _ELIGIBLECONTESTANT_ all target TOPSEC firewalls. This manufacturer’s products are not used much outside of China where its hardware has supported high-profile clients such as spacecraft, the 2008 Olympic Games, and Shenzhen University.

### Preventing _EXTRABACON_ attacks
There are a few ways to prevent EXTRABACON attacks: 

First, install the latest patch update from CISCO. Previously compromised hardware might still be susceptible if the attacker installed a rootkit while he had access to the system, but fresh installs should be secure. 

Administrators should also limit SSH and SNMP interfaces. Preventing external access to the firewall would make sure attackers would have to have a local hardware connection to compromise the system by blocking remote access to certain ports on the network. 

Additionally, update to SNMP version 3 or later. This version, released in 2005, brings Triple DES Encrypted Authentication.[^21] Triple DES (3-DES) is defined as a symmetric-key block cipher which applies the Data Encryption Standard (DES) cipher algorithm three times to each data block.[^22]

Lastly, using Strong Authentication in general is always helpful and good practice. 

----

## References

[^1]:	[Kaspersky](http://usa.kaspersky.com/about-us/press-center/press-releases/2015/kaspersky-lab-discovers-equation-group-crown-creator-cyber-espi)

[^2]:	[Kaspersky](http://usa.kaspersky.com/about-us/press-center/press-releases/2015/kaspersky-lab-discovers-equation-group-crown-creator-cyber-espi)

[^3]:	[Lawfare](https://www.lawfareblog.com/very-bad-monday-nsa-0)

[^4]:	[Wikipedia](https://en.wikipedia.org/wiki/Tailored_Access_Operations)

[^5]:	[ArsTechnica](https://arstechnica.com/security/2016/08/hints-suggest-an-insider-helped-the-nsa-equation-group-hacking-tools-leak/)

[^6]:	[BusinessInsider](http://www.businessinsider.com/edward-snowden-shadow-brokers-russia-leaked-nsa-equation-group-files-warning-dnc-hacking-2016-8?r=DE&IR=T)

[^7]:	[NYTimes](https://www.nytimes.com/2016/10/20/us/harold-martin-nsa.html)

[^8]:	[taia.global _(deprecated)_](https://taia.global/2016/08/shadowbroker-is-a-native-english-speaker-trying-to-appear-non-native/)

[^9]:	[Bloomberg](https://www.bloomberg.com/view/articles/2017-01-13/cyberwar-has-gone-public-and-that-s-dangerous)

[^10]:	[ArsTechnica](https://arstechnica.com/tech-policy/2016/08/snowden-speculates-leak-of-nsa-spying-tools-is-tied-to-russian-dnc-hack/)

[^11]:	[BusinessInsider](http://www.businessinsider.com/edward-snowden-shadow-brokers-russia-leaked-nsa-equation-group-files-warning-dnc-hacking-2016-8?r=DE&IR=T)

[^12]:	[NYTimes](https://www.nytimes.com/2016/08/17/us/shadow-brokers-leak-raises-alarming-question-was-the-nsa-hacked.html?_r=3)

[^13]:	[BusinessInsider](http://www.businessinsider.com/shadow-brokers-claims-to-hack-equation-group-group-linked-to-nsa-2016-8)

[^14]:	[HackerNews](http://thehackernews.com/2016/08/nsa-hack-exploit.html)

[^15]:	[Cisco](https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20160817-asa-snmp)

[^16]:	[Cisco](https://www.cisco.com/c/en_ca/products/security/asa-5500-series-next-generation-firewalls/models-comparison.html\#tab-c)

[^17]:	[Internet Engineering Task Force (IETF)](https://www.ietf.org/rfc/rfc1157.txt)

[^18]:	[Internet Engineering Task Force (IETF)](https://tools.ietf.org/html/rfc4253)

[^19]:	[Cisco](https://www.cisco.com/c/en/us/td/docs/security/asa/asa-command-reference/S/cmdref3/s16.html)

[^20]:	[Topsec](http://www.topsec.com.cn/english/about_us.html)

[^21]:	[Cisco](http://www.cisco.com/c/en/us/td/docs/ios/12_4t/12_4t2/snmpv3ae.html)

[^22]:	[Wikipedia](https://en.m.wikipedia.org/wiki/Triple_DES)