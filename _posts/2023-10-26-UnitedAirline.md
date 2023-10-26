---
layout: post
title: United Airlines and AWS Backup to Protect Data from Ransomware Events    
---

Ransomware is continuing as the number one threat to the data across all Industries. There have been a sizable number of Ransomware attacks in 2021 with an increase in volume (by 37 percent) during the ongoing pandemic. SonicWall recorded an all-time high of 78.4 million ransomware attacks globally in June 2021.
Ransomware is a destructive threat to your data and in this blog, I will discuss various techniques of AWS Backup that can help you to sustain Ransomware attacks on your AWS workload.

Some Ransomware Artifacts
One of the recent reports says that the U.S. government gives ransomware attacks the same level of priority as terrorism. For any organization, Ransomware attacks can have a highly staggering impact.
American oil pipeline system Colonial Pipeline, Acer, Chicago-based CNA Financial Corp, Kia Motors and many Indian companies are the victims of Ransomware attacks of 2021.  

What is Ransomware attack
Ransomware is malicious code designed by cybercriminals to gain unauthorized access to systems and data and encrypt that data to block access by legitimate users. Once ransomware has locked users out of their systems and encrypted their sensitive data, cybercriminals demand a ransom to provide a decryption key to unlock the blocked systems and decrypt data. In theory, if the ransom is paid within the allotted time, systems and data are decrypted and made available once again and normal operations continue. However, if the ransom demand is not satisfied, organizations risk permanent destruction or public-facing data leaks controlled by the attacker. That there is no rule of the game.

Unique Features of AWS Backup
There are a few capabilities that enrich the security posture to protect against Ransomware attacks on your backup.

1. AWS Multi-account backup method
2. AWS Backup Vault Lock

United as everyone knows is a global airline company  with hundreds of destinations around the world.  
The cloud engineering team governs all the AWS Accounts at United. And this solution we came up with because we wanted to protect United's cloud infrastructure  from a ransomware event. 
Assuming you are an application owner you  need to back up your applications, this diagram can walk you   through the flow.
The child account as you are the owner is one of the child accounts that United Airlines has, approximately more than 150  accounts which ensures the AWS Multi-account backup method is well achieved.
The central account where we manage all this central backup solution. 
Technology stacks: IAM, Backup Vault, CloudTrail, Lambda, Stepfunction, EventBridge, Athena, QuickSight, and CloudFormation.
As every application has an  IAM role, when that IAM role is created that triggers a CloudTrail event (A). 
Then the CloudTrail event is forwarded to the EventBridge in the central account (B). 
So this EventBridge triggers a Step Function and the Step Function takes the CloudTrail as a payload and this Step Function is a series of Lambda functions (C). 
The Lambda function creates a backup. And a part of that creates a backup vault and a backup policy. The backup policy tells the backup how  often the backup needs to be taken and the way the backup needs to be copied. It's a series of events occurring leading to the creation of the backup vaults as well as the replication plan. (D)
Each application has one backup  vault, but in general, we have two primary regions.  So when a backup is taken, it copies  it to both the regions as well as it copies it to a network-isolated region to protect it from a ransomware event. This will protect it from being compromised.  
United Airlines has almost upward of 300 applications and  that number is fast increasing as the AWS  presence increases within United. And they almost  back up two petabytes of data.

RESTORING:
Application owners can restore from backup by going to United Airlines Servicenow and opening a help hub request (E). They’re providing all  the information what needs to be restored
The ServiceNow triggers a CloudFormation  template providing all the information as an input (F).
This CloudFormation creates an IAM role (G), this  IAM role is a just-in-time access, so for example if they wanted to have you know how long they wanted to have this IAM role to be available. 
Then user application owner can restore from the backup vault (H)
So once that once they complete their function, their policy is again removed and nobody will have access to the backup vault. That IAM role gives access to this particular backup vault for that particular application. 

MONITORING:
QuickSight is also leveraged in their central accounts as we can see.
Whenever a backup happens, all the information is sent to the EventBridge performs a series  of functions eventually all the information is stored in Athena tables. (I)
QuickSight leverages  the Athena tables to provide that functionality and the operational teams get visibility into all the backups that are happening (M). For example, failures are happening or like how often the backups are happening.  
All that information is available via custom dashboards to have visibility through the EventBridge. 

