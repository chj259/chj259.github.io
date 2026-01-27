---
title: "Notes on Security Engineering Chapter 2: Who is the Opponent?"
tags: Other
---

This chapter goes through four different categories of opponents: the state, criminals, geeks, and the swamp. For each opponent the textbook analyzes their motivations and common methods of attack. Analysis of opponents also takes a wider societal view of security, like the representation issue in security engineering and how that fails people suffering from IPV, or viewing online radicalization under the lens of a cyberattack. I will try to look more into the case studies that Anderson brings up to give more context.


NSA please don't get mad at me :) To be honest, the biggest takeaway from this chapter is that the government is pulling some crazy shit.

## Spies

For govts with extensive resources, attacks take advantage of a huge pool of data to carry out surveillance at scale. Other govts use individual viruses, spearphishing, or even outsource work to local hacker groups. Of course, there are also influences to online sentiment like what happened in 2016.

### The Five Eyes

The Five Eyes countries are USA, UK, Canada, Australia, and New Zealand. Their capabilities are mostly known through the Edward Snowden leaks, with the section going through many of the technologies described in the leaks.

The first of these is **Prism**. The NSA had a tap on internal systems of big tech companies like Google, Facebook, and Yahoo as long as the target of the surveillance was a foreigner. 

**Tempora** is a UK-based fire-optic wiretapping program. So much data would be collected daily (21Pb from just Cornwall alone) that the feed undergoes "massive volume reduction" and the remaining material can be filtered with "selectors". Data is kept for 30 days then deleted. Why UK? because fiber optic cables are laid along phone cables are laid along telegraph end stations, and England's 19th ventury global empire literally set the foundations for a world-wide intelligence asset.

**Muscular** is an application running on Tempora. It takes advantage of the fact that big companies don't encrypt communications within their perimeter. Agencies could pick up the data from between the internal data centers that's in the clear. Even though companies learned from this and adopted more zero trust architecture, there might still be issues from how CDNs don't encrypt data flowing in (o.e. backhaul traffic) thus making it vulnerable to parties who can read the traffic directly from the wires. 

![Muscular Slide](/assets/img/muscular.png)

**Special Collection Service** or SCS deals in foreign intelligence collection, such as cell data antennas in embassies, bugs, moles in target organizations, collecting EM emissions (**Tempest** monitoring)
Part of SCS monitoring involves tampering of the supply-chain, such as implants in routers. Bullrun or Edgehill is the program all about supply-chain tampering, from misdirecting academic research to influencing NIST standards so certain cryptographic technologies or key lengths are used, making those systems intentionally vulnerable. The Dual_EC-DRBG random number generator had an NSA backdoor, for example. Though it did not see widespread adoption, it was used in the RSA BSAFE library that many companies used.

To search the data, analysts use *Xkeyscore*. As a federated system, one query scans all sites, including some that might be hacked systems set up to exfiltrate data with a certain query. A notifier called Trafficthief tips off analysts when targets do anything of interest, Mugshot crawls and fingerprints machines for analysts to target, and fingerprints can also be built for a target's online activity. A case study can be made of Gemalto, which was hacked by using Xkeyscore to identify sysadmines to spearphish, compromising billing servers, and harvesting keys in transit to mobile SPs “by email or FTP with simple encryption methods that can be broken … or occasionally with no encryption at all.” To quote this [Forbes article](https://www.forbes.com/sites/thomasbrewster/2015/02/20/telecoms-industry-compromised-by-gchq-and-nsa/), "These keys encrypt calls, texts and internet usage between the mobile user and their telecoms provider. By stealing them, GCHQ could harvest communications data, as the agency is known to do, and unlock the content of the messages any time they wanted."

To defeat VPNs, a decryption service called **Longhaul** breaks the ciphertext using cutting-edge cryptanalytic techniques. For example, some variant of the "Logjam attack" might be possible against 1024-bit Diffie-Hellman key exchanged used by many VPNs and TLS, especially since many people use the same primes. 

**Quantum** attacks communication in real time by injecting a malicious packet somehow exploiting the browser to get in the way of the encrypted tunnel. This and other hacking techniques fall under **Computer and Network Exploitation** (CNE). Thanks to a number of disclosures both by official sources as well as whistleblowers and hackers, there are many tools documented for the public, sometimes even used by law enforcement to catch criminals. 

### The analyst's viewpoint & Offensive Operations

An analyst will basically use Xkeyscore like Google to find their target. Once they have, Xkeyscore gives data on which websites they visit, their locations, install into their devices, look into the people they know through contact historu, etc. On the offensive side, cyberweapons came into the conversation after Stuxnet allegedly caused an explosion in Iran's Natanz Nuclear Facility.

### Attack scaling

Anderson makes some interesting points here. It takes a lot of effort to wiretapp a single person. However, it takes a lot of initial cost to tap everyone, but each additional tap goes way down. "The Five Eyes strategy is essentially to collect everything in the world; it might cost billions to establish and maintain the infrastructure, but once it’s there you have everything." This is true from both a surveillance and offensive standpoint.

### Attribution

"It's often said that cyber is different, because attribution is hard." For the most part this is untrue and attacks can be traced reliably. Nation-states can use the ambiguity to perpetrate false-flag attacks or otherwise cover up their activities. The two examples brought up are the "Climategate" emails that seemed to be intentionally fomenting climate change conspiracy, and the Equifax hack compromising at least 145 million Americans. So intelligence and crime go hand in hand as always.

## Crooks

Anderson claims that the noted decrease in crime is because the crime went online. Around the early 2000's, the cybercrime industry boomed as participants specialized into their own niches of the supply chain. 

### Criminal Infrastructure

**Botnet herders** build and maintain botnets to rent to others in the ecosystem. At first botnets communicated through a central control server, but defenders would just find and take down the control server. Then they used peer-to-peer, but defenders could join the network and harvest all the addresses being shared peer-to-peer. Attackers then used DGAs to randomly locate a control server through a generated domain, but defenders could use blacklists on registrars, and more recently deep learning to predict domains. The most recent iteration combines all of these techniques: a hierarchy of small peer-to-peer networks communicating with a "manager" node that is hard to get at, and these control nodes use DGA to find the overall botmaster. 

**Malware devs** write malware for the criminal market, usually specializing in a certain kind of exploit. This is a small market where arresting one actor can make a big difference, and so devs are usually based in non-extradition nations like Russia. 

**Spam senders** are a highly specialized business requiring many tricks to get past filters. Usually used to get an "in" for malware. 

**Bulk account compromise** sells to botnets by gather up a huge amount of machines to maintain their herds.

**Targeted attackers**, on the other hand, will hack a specific target for a fee. This involves specific research, spear-phishing, and account compromise. 

**Cashout gangs** steal credit cards and attempt to bypass various laundering controls to cash them out. It is a constantly evolving space.

**Ransomware** seems to have switched to demanding payment in gift cards, and to counteract backups, they wait for a long time to encrypt the backups as well as the data,

### CEO crimes

This was an interesting section. "Companies attack each other, and their customers too". An example mentioned is the cryptography used in printers to only allow them to use the proprietary ink cartridges. In Lexmark v SCC, companies can legally hack the chips in the ink cartridges to try and break them. Companies have also been known to try and "hack" around testing standards, for example. So threat models should also consider opponents in the supply chain, such as industrial espionage and more.

## Whistleblowers and Geeks

There is a clear incentive against whistleblowing, which often gets treated as a sort of insider threat. However, it's important that people are able to report wrongdoing safely. There are technical considerations to creating a whistleblowing system, such as preventing the identity from being tracked, encrypting communication channels, etc. The main problem is supporting the whistleblower afterwards, since their identity is usually obvious and the retaliation inevitable.

Geeks similarly reveal vulnerabilities often with the goal of common good and can face the same issues with disclosure. However, Anderson surmises that industries will widely accept responsible disclosure practices because to do otherwise incurs costs. Other geeks may work with the government to store away vulnerabilities instead, operating as cyber-arms dealers. One difference from usual weapons, though, is that using a vulnerability can result in it being reverse-engineered and patched. 

## Other

Some interesting insights: online mobs can be considered a form of hacktivism that are hard to regulate due to the ambiguity of context, and many systems fail when the threat actor is an intimate partner. This oversight seems to be due to security engineering's proclivity towards a certain demographic that overlooks htis kind of abuse, highlighting the importance of diverse perspectives in security.


