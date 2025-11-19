**
---
title: Introduction to Active Directory
tags: Other 
---
## Active Directory Structure

AD is the directory service for Windows. It follows a sort of graph-like **forest** structure, each tree is a **domain**, subgraphs are subdomains. Vertices are **objects** (GPOs, OUs, DCs, Users, Groups, Computers, etc.) 

## Active Directory Vocab

1. **Objects**: basic "unit" of resource in AD
2. **Attributes**: characteristics of object e.g. hostname, DNS; think metadata
   * Each attribute has associated LDAP name for querying.
3. **Schema**: blueprint of environment, defines objects & attributes in forest
   * This is where classes are defined (very object oriented)
4. **Domain**: tree, not necessarily maximal
5. **Tree**: the spanning tree
6. **Leaf**: objects with no children
7. **Container**: objects whose goal is to store others
8. **GUID**: unique ID for every user & group, stored in objectGUID attribute; think MAC address
9. **Security Principals**: Either objects that should be authenticated, or the objects that do the authenticatine
   * DIFFERENT from local auth, e.g. SAM, this is domain auth
10. **SID**: unique ID for security principals and groups 
   * there are some default SIDs, e.g. Guest, Everyone
   * login process uses SID in auth token that contains it, as well as associated access levels
11. **Distinguished Name (DN)**: full path from root to object
12. **RDN**: subset of DN, two objects in same container can't have the same RDN
13. **First Single Master Operation (FSMO)**: system for assigning abilities to DCs, designed to improve consistency between DCs while preventing everything from being assigned to a single DC. first DC in root of forest gets all 5 permissions, subsequent DCs only get 3. this can be reconfigured by admins
14. **Global Catalog**: DC that archives all objects in forest, even those not in its domain
   * for auth & object searching
15. **Read-Only DC**: reduces replication traffic and contains modifications by SYSVOL
16. **Replication**: objects copied between DCs, KCC makes sure changes are synchronized between DCs
17. **GPO**: the rules and settings, they also have GUID
18. **SACL**: Sometimes access will be logged, these permissions are controlled by SACL
19. **Tombstone**: contains deleted object, unlike recycle bin, removes all attributes
20. **Recycle Bin**: Mostly preserves attributes
21. [**SYSVOL**](https://learn.microsoft.com/en-us/archive/technet-wiki/24160.active-directory-back-to-basics-sysvol): Folder on every DC, stores public files that outside DCs should have access to, particularly all GPOs, some task scripts
22. **Admin SD Holder**: Security principal for admins 
   * SDPropagator restores ACL by comparing permissions with AdminSDHolder, check happens regularly to prevent persistence
   * side effect is that accounts with adminCount=1 attribute (SDProp protected) usually means they're higher value and become targets
23. **dsHeuristics**: attribute describing config settings forest-wide
   * interaction with SDProp - removes built-in groups from being touched by AdminSDHolder
24. **sIDHistory**: attribute recording all previous SIDs, could be dangerous if misconfigured
25. **NTDS.DIT**: the heart of AD, stores all password hashes in domain, or even just cleartext

## Common Objects

### Users

Leaf objects, considered security principals. Even low privileged users can access much of the environment and enumerate the entire domain/forest

### Contacts

Leaf objects, but NOT security principals. They contain contact info for external users

### Printers

Leaf objects and not security principals

### Computers

Leaf objects, security principals, very important targets similar to users. Use NT AUTHORITY\SYSTEM account to gain access and perform enumerations

### Shared Folders

Points to the folder on the specific computer where it resides. Follows certain access control rules.

### Groups

Container object that is a security principal. Helps sort objects by roles, departments, etc. However, nested group membership usually leads to vulnerabilities. Manages *permissions*.

### Organizational Units

Container that stores similar objects. It's sort of like a mix between a class and a container, if you set certain attributes on the OU object it gives those permissions to all objects within the OU. Manages *settings*.

### Domain

A tree of objects, basically. All domains have policies, some are manual and some are automatically configured

### Domain Controllers

The brains of AD, they handle authentication, authorization, and access, also enforces security policies(e.g. SDProp) and stores information about domain

### Sites

Set of computers connected across subnets, they are used to help replication across DCs

### Built-in

Container that holds default groups

### Foreign Security Principals

Security Principals from trusted external forests, added whenever such an object is added to the current domain. It's more like a pointer to the actual object in the external forest. They're all put into a specific container called ForeignSecurityPrincipals
