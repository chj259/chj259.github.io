---
title: Some Notes on AD Security
tags: Other
---

AD has the same security tricks as usual, like password rotation using Local Administrator Password Solution (LAPS), setting up logging and monitoring, RBAC with Group Policy Objects. Special care should be given for **group managed service accounts (gMSA)**, which manage services that require credentials, **Domain Admins**, which have full control over the entire domain, and the rights for **Local Admin** as well as **RDP**.

## Group Policy

GPOS are applied in the following order from first to last, with overlapping policies getting overwritten.

1. **Local Group Policy:** Defined by host locally
2. **Site Policy:** Policies specifically for the Enterprise Site of the host
3. **Domain-wide Policy:** Defined by domain
4. **Organizational Unit:** Defined by organizational unit

Policies in OUs are applied from parent to child, so policies in leafs like users and computers overwrite higher level OUs. Within an object, policies with higher link orders are applied first. However, by specifying the **Enforced** option, a GPO cannot be overwritten. The option **Block inheritance** will prevent policies from higher levels from being applied to the OU. 

## GPO Vulnerabilities

Make sure users only have the rights and privileges they need. Otherwise they can do all sorts of malicious things to your AD, like adding additional rights, scheduling malicious tasks, adding admins, etc. 

![Bloodhound Example](/assets/img/bh_gpo.png)

We can identify possible attack paths using [Bloodhound](https://bloodhound.specterops.io/get-started/introduction) as shown below. Here, **Domain Users** can modify **Disconnect Idle RDP** GPO because they are part of **Authenticated Users**.
