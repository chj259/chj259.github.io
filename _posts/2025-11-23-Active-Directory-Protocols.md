---
title: Active Directory Protocols
tags: Other
---
## Kerberos

The default EAP for Windows since 2000. Distributes one-time tickets to authenticate users, and is stateless.

Remember that domain controllers handle authentication. Each DC has a **Key Distribution Center (KDC)** that recieves requests from users when they login. The request is encrypted by user's password, and if successfully decrypted, grants **Ticket Granting Ticket** to user. 

User shows TGT to a DC in order to gain access to services (TGS ticket). They present this ticket to the service or application to gain entry.

System ensures that  *passwords are never sent over the network.*

Kerberos is TCP/UDP port 88.

## LDAP

Directory lookup protocol (latest specification [here](https://datatracker.ietf.org/doc/html/rfc4511)). How applications communicate with serves, similarly to how HTML allows web to communicate wtih a web server--Active Directory is a directory server that uses LDAP protocol.

Connect to a **Directory System Agent**, which is the LDAP server and converts requests to LDAP queries. LDAP is 389 and LDAPS is 636.

## LDAP Authentication

1. **Simple Authentication**: provide a username and password to authenticate. Includes anonymous auth, unauthenticated auth, and username/password auth.
2. **SASL Authentication**: Uses Simple Authentication and Security Layer framework for authentication. 

Authentication credentials are set per each session using BIND operation. All of these messages are unencrypted by default, so it's recommended to turn on TLS encryption.

## MSRPC

Window's remote protocol, allows one host to execute programs on another host. There are 4 different RPC interfaces:

1. **lsaprc**: Handles calls to Local Security Authority(LSA), which manages local security policy, audit policy, and auth services. Usually for changing domain security policy.
2. **netlogon**: Authenticate users and services in domain.
3. **samr**: Remote SAM manages domain account database, e.g. users, groups, computers and other security principals. This info is usually available to all authenticated domain users, which allows tools like Bloodhound to map out domain trees. This can be mitigated by only allowing admins to make samr queries. 
4. **drsuapi**: Directory Replication Service Remote Protocol, handles replication between remote DCs. Be careful that it doesn't replicate an important file!

## NTLM

This is a hashing protocol used to store sensitive info, used in common AD auth protocols like **NTLMv1** and **NTLMv2**. 

### LAN Manager (LM)

Oldest and worst hash type, uses MAXIMUM of 14 characters split into two seven-character chunks, NOT CASE SENSITIVE. If password is shorter than 14 chars, it's padded with NULLs. 

1. Split password into seven-character chunks
2. On each chunk, use DES to encrypt chunk with string KGS!@#$% 
3. Concatenate the chunks together, crating LM hash

### NT LAN Manager (NTLM) 

Challenge-response protocol that builds on previous LM hash as well as a new NT hash. 
1. NEGOTIATE_MESSAGE: the "SYN"
2. CHALLENGE_MESSAGE: Checking authentication
3. AUTHENTICATE_MESSAGE: the "SYN-ACK". Store the NT and LM password hash in SAM database or NTDS.DIT database

### NTLMv2
New and improved NTLM that has a more detailed challenge response mechanism. Uses MD5 and sense two responses instead of just one. 

### MSCache2

Auth protocol that can be used when DCs are unavailable. Also known as Domain Cached Credentials (DCC). 

Hosts save the last ten hashes for users that logged in at **HKEY_LOCAL_MACHINE\SECURITY\Cache** and use these for authentication. Users logged on this way can't access any services that require DC auth. Also, hashes acquired from MSCache2 can't be used for pass-the-hash attacks and are hard to crack.



