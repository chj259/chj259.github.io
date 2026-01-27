---
title: "HTB AD Administration: Guided Lab"
tags: Other
---

Happy New Year! First post of 2026 will be going over the commands used in this guided lab.

Firstly, **xfreerdp** command is used to RDP to the Windows computer. It's an open-source command line tool for this, there are other apps out there for different OS, including an official one by Windows. Command usage is: 
```
xfreerdp /v:hostname /u:username /p:password
```
Be careful not to include space after the colon since the command is sensitive towards that. Special characters like \ should also be escaped. Other flags include /sec: (specifies which TLS), /d: (domain name), /f: (launch in full screen)

## Task 1

Task 1 of the lab is to add and remove users to "inlanefreight.local". We do this by opening "Active Directory Users and Computers". Right click on the folder the user should be added on and select New > User. Fill in information as needed. The next screen will have options about the user's password, e.g. change password at next login.

![Add New User](/assets/img/new_user.png)

We also need to remove users, who we are given names for. To search for a certain employee, right click on Employees > Find and enter the name. From here, we can right click on the user to remove and select Delete. 

![Remove User](/assets/img/search.png)

Lastly, we need to unlock an account and force password change at the next login. After finding the guy's account, select Reset Password and configure accordingly.

![Reset Password](/assets/img/reset_pass.png)

## Task 2

We will now add the new users to a security group and nest that security group in an OU. Remember that groups are for permissions while OUs are for rights. Since OUs are not security principals, you can't add one to a group. You instead would have to add the security group to the OU. Creating a new OU and new security group is very similar to creating a user: New > Organizational Unit and New > Group respectively. After the containers have been created, drag the users into the folder icon to add them to the OU or security group.

## Task 3

Our very last task relates to Group Policy Objects (GPO). We want to create a new GPO by duplicating the Logon Banner GPO. Then, we have to right click and Edit for setting some custom configurations. 
* Access to Powershell and CMD is located in User Configuration > Policies > Administrative Templates > System. 
* Removable media policy is located in User Configuration > Policies > Administrative Templates > System > Removable Storage Access. 
* To check Logon Banner settings, we go to Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options. 

So far we've learned how to and and remove new users, security groups, and OUs, as well as configure access for users. We've also looked at GPOs and how to create/configure them. We move on to the second part of the guided lab, which only has one part. 

## Task 4

We are asked to add one new computer to the domain. Using the credentials given to us, we RDP into that computer. Navigate to Control Panel > System and Security > System. Click "Change settings" under the domain settings header, which will allow us to input our new domain. 
