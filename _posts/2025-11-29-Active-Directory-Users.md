---
title: All About Users
tags: Other
---

Gives "identity" to a person or computer, allowing for more efficent access management through use of groups. 

Some users may have multiple accounts for different roles, and there are often also disabled users from former employees, temps, interns, etc. that are being saved for audit purposes and put in the "former employees" OU.

## Local Accounts

Scope is limited to a single server/workstation, so permissions only apply to that single host and not to the wider domain.

- **Administrator**: the first account created on the system, it has full access to all resources and cannot be deleted or locked. However, it can be disabled/renamed.
- **Guest**: Disabled by default, a bad idea to re-enable because it allows unauthenticated access to the host
- **SYSTEM**: Default account used by Windows OS to perform certain processes. Has highest level permission. Can't be altered in User Manager/put in a group. 
- **Network Service**: Account used for remote services.
- **Local Service**: Account used for running local services, the name doesn't lie.

## Domain Users

Unlike local users, their scope is the entire domain. 

Important Account: **KRBTGT** account distributes TGTs for various domain resources, if an attacker has this account they can basically grant themselves access

## User Naming Attributes

Remember what these are: UPN, ObjectGUID, SAMAccountName, objectSID, sIDHistory

## Domain-joined and Non-Domain-joined

### Domain-joined

The classic AD model using domain controller as central "brain" and storage, access is determined by DC, devices don't communicate directly with each other. 

### Non-domain joined

Also known as a **workgroup**, devices don't usually have a DC and are P2P connected to each other. Each host determines access itself and there's not really any concept of a "group". 


