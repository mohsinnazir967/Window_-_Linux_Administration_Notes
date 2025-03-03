

# Critical Windows Event IDs Every SOC Analyst Must Monitor! 🚨  
  
## 🔍 Authentication & Account Monitoring:  

✅ Failed Login Attempts - Event ID: 4625 (Brute-force attack indicator)  
✅ Account Lockouts - Event ID: 4740 (Possible attack or misconfigured account)  
✅ Successful Login Outside Business Hours - Event ID: 4624 (Potential insider threat)  
✅ New User Creation - Event ID: 4720 (Privilege escalation risk)  
✅ Privileged Account Usage - Event ID: 4672 (Administrative actions tracking)  
✅ User Account Changes - Event IDs: 4722, 4723, 4724, 4725, 4726 (Monitor critical modifications)  
✅ Logon from Unusual Locations - Event ID: 4624 (Requires geolocation correlation)  
✅ Dormant Account Usage - Event ID: 4624 (Rarely used accounts being accessed)  
  
## 🔐 Access & Privilege Escalation Monitoring  

🔹 Group Membership Changes - Event IDs: 4727, 4731, 4735, 4737 (Potential privilege escalation)  
🔹 Excessive Logon Failures - Event ID: 4625 (Brute-force detection)  
🔹 Disabled Account Activity - Event ID: 4725 (Unauthorized account reactivation)  
🔹 Service Account Activity - Event IDs: 4624, 4672 (Monitor service account usage)  
🔹 RDP Access Monitoring - Event ID: 4624 (Filter for RDP connections)  
🔹 Lateral Movement Detection - Event ID: 4648 (Network logon tracking)  
  
## 🛡️ Endpoint & System Security  

🔸 File and Folder Access - Event ID: 4663 (Unauthorized file access detection)  
🔸 Unauthorized File Sharing - Event IDs: 5140, 5145 (Suspicious SMB activity)  
🔸 Registry Changes - Event ID: 4657 (Malware persistence attempts)  
🔸 Application Installation & Removal - Event IDs: 11707, 1033 (Potential unauthorized installs)  
🔸 USB Device Usage - Event IDs: 20001, 20003 (Detect unauthorized device usage)  
🔸 Windows Firewall Changes - Event IDs: 4946, 4947, 4950, 4951 (Potential backdoor attempts)  
🔸 Scheduled Task Creation - Event ID: 4698 (Persistence mechanism used by attackers)  
🔸 Process Execution Monitoring - Event ID: 4688 (Malicious process detection)  
🔸 System Restart or Shutdown - Event IDs: 6005, 6006, 1074 (Unexpected shutdowns)  
🔸 Event Log Clearing - Event ID: 1102 (Indicators of log tampering)  
  
## 🦠 Threat Intelligence & Malware Indicators  

⚠️ Malware Execution - Event IDs: 4688, 1116 (Monitor suspicious process execution)  
⚠️ Shadow Copy Deletion - Event ID: 524 (Ransomware indicator)  
⚠️ Execution of Suspicious Scripts - Event ID: 4688 (Monitor PowerShell, cmd, etc.)  
⚠️ Service Installation or Modification - Event ID: 4697 (Persistence method for malware)  
  
## 🌐 Network & Active Directory Security  

🌍 Unusual Network Connections - Event ID: 5156 (Monitor suspicious outbound connections)  
🌍 Unauthorized Access to Shared Files - Event ID: 5145 (Potential data exfiltration)  
🌍 DNS Query for Malicious Domains - Event ID: 5158 (Requires DNS logs)  
🌍 LDAP Search Abuse - Event ID: 4662 (AD enumeration attempts)