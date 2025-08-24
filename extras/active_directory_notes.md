## Active Directory  

### Introduction  

| Term                          | Description                                                                                  | Example/Notes                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| **Active Directory**           | Directory service for Windows network environments; centralized management of resources      | Users, computers, GPOs, trusts                                  |
| **Authentication & Authorization** | Provides auth and access control in Windows domain environment                             | Login via Kerberos, NTLM                                        |
| **Forest**                     | Container of separate domains and objects under the same umbrella                            | Root forest with multiple child domains                        |
| **ADFS**                       | Active Directory Federation Service for SSO with claim-based model using token authentication | Cross-domain SSO setup                                          |
| **gMSA**                       | Group Managed Service Accounts for secure automation, password rotation, anti-kerberoasting   | Used for services, tasks, applications                          |  

---

### Active Directory Structure  

| Term                          | Description                                                                                  | Example/Notes                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| **Hierarchical Structure**     | Forest → Domains → OUs → Objects hierarchy                                                   | Multiple domains under one forest                              |
| **Domain**                     | Contains users, devices, groups                                                               | child1.domain.com, child2.domain.com                           |
| **OU (Organizational Unit)**   | Logical container for users, computers, groups                                                | IT OU, HR OU                                                   |
| **Trust Relationships**        | Two top-level domains with bidirectional trust but children may not trust each other          | Child of A can’t auth child of B                                |

---

### Active Directory Terminology  

| Term                          | Description                                                                                  | Example/Notes                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| **Object**                     | Any resource in AD: Users, Computers, OUs, DCs                                                | User = bjones, Computer = IT-PC01                              |
| **Attribute**                  | Properties associated with objects; have LDAP names for querying                             | hostname, IPAddress                                            |
| **Schema**                     | Blueprint defining object types and attributes                                               | Add custom attribute → schema update                           |
| **Container**                  | Object holding other objects in hierarchy                                                    | OU → Users & Computers                                         |
| **GUID**                       | 128-bit unique identifier for objects                                                        | objectGUID in PowerShell                                       |
| **Security Principal**         | Authenticated entities like users, computers, processes                                      | SID assigned to each                                           |
| **SID**                        | Security Identifier, unique per user/group, never reused after deletion                      | S-1-5-21-...                                                   |
| **Distinguished Name (DN)**     | Full path to object in AD                                                                    | cn=bjones,ou=IT,dc=domain,dc=local                             |
| **FSMO Roles**                  | Critical AD roles divided for DCs; Schema Master, Domain Naming Master, RID, PDC, Infra Master| First DC holds all roles initially                             |
| **Global Catalog (GC)**         | Stores object attributes across domains for authentication/search                            | GC enabled on DC                                               |
| **RODC**                        | Read-Only Domain Controller, no password caching                                             | Branch offices                                                 |
| **Replication**                 | DCs replicate via KCC and connection objects                                                 | Multi-DC setup                                                 |
| **SPN**                         | Service Principal Name used by Kerberos for service auth                                     | MSSQLSvc/sqlserver:1433                                        |
| **GPO**                         | Group Policy Objects controlling environment settings                                       | Password policies, scripts                                     |
| **DACL**                        | Discretionary ACL listing security principals with access permissions                        | No DACL → Full access                                          |
| **SACL**                        | Logs access attempts on secure objects                                                       | Security event log auditing                                    |
| **Tombstone**                   | Deleted object storage (60–180 days)                                                         | isDeleted = TRUE                                               |
| **AD Recycle Bin**               | Preserves most attributes for easy restore                                                   | Default 60 days                                                |
| **SYSVOL**                       | Public share for scripts, GPOs replicated on all DCs                                        | NETLOGON scripts folder                                        |
| **AdminSDHolder**                | Controls ACLs for privileged AD groups, enforced by SDProp                                   | Protects Admin accounts                                        |
| **dsHeuristics**                  | Configurable behavior for AD features                                                       | Exclude groups from SDProp                                     |
| **adminCount**                    | Attribute marking privileged accounts for SDProp protection                                 | adminCount=1 for privileged                                    |
| **ADUC**                          | GUI for managing AD Users & Computers                                                        | dsa.msc console                                                |
| **ADSI Edit**                     | Advanced AD attribute and object editing tool                                                | adsiedit.msc console                                           |
| **sIDHistory**                    | Previous SIDs preserved for migrations, can be abused if insecure                            | SID filtering recommended                                     |
| **NTDS.dit**                      | AD database storing users, groups, password hashes                                           | C:\Windows\NTDS\NTDS.dit                                       |

---

### AD Admin Commands  

| Purpose                          | Command Example                                           | Description / Usage                                  |
|----------------------------------|-----------------------------------------------------------|-----------------------------------------------------|
| List all AD users                 | `Get-ADUser -Filter *`                                     | Displays all users in AD                             |
| View specific user details        | `Get-ADUser -Identity username -Properties *`              | Shows attributes of user                             |
| List AD groups                    | `Get-ADGroup -Filter *`                                    | Shows all groups                                     |
| View group members                | `Get-ADGroupMember -Identity "GroupName"`                  | Displays group membership                            |
| Create new user                   | `New-ADUser -Name "John Doe" -SamAccountName jdoe -AccountPassword(Read-Host -AsSecureString "Enter Password") -Enabled $true` | Adds a new AD user |
| Reset user password               | `Set-ADAccountPassword -Identity jdoe -Reset -NewPassword (ConvertTo-SecureString "NewPass123" -AsPlainText -Force)` | Resets password |
| Unlock user account               | `Unlock-ADAccount -Identity jdoe`                         | Unlocks AD account                                   |
| Enable/Disable user               | `Enable-ADAccount -Identity jdoe` / `Disable-ADAccount -Identity jdoe` | Enables/disables account        |
| Search for locked accounts        | `Search-ADAccount -LockedOut`                              | Finds locked out accounts                            |
| Get FSMO role holders              | `netdom query fsmo`                                        | Displays FSMO role owners                            |
| Check replication status          | `repadmin /replsummary`                                    | Summarizes AD replication health                     |
| View AD sites                     | `Get-ADReplicationSite -Filter *`                          | Lists all AD sites                                   |
| Force group policy update         | `gpupdate /force`                                          | Forces policy refresh                                |

---

### SOC Analyst Commands  

| Purpose                          | Command Example                                           | Description / Usage                                  |
|----------------------------------|-----------------------------------------------------------|-----------------------------------------------------|
| Search security logs              | `Get-WinEvent -LogName Security | ? { $_.Id -eq 4625 }`      | Find failed logon attempts                           |
| Real-time event monitoring        | `Get-WinEvent -LogName Security -MaxEvents 50 | Format-Table` | Live log analysis                                    |
| Find account logon failures        | `Get-EventLog -LogName Security -InstanceId 4625`          | Checks failed logons                                 |
| Search privileged group changes    | `Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4728}` | Detect added members to admin groups                |
| Check for SPN registrations        | `setspn -Q */*`                                            | List all registered SPNs                             |
| Kerberos tickets analysis          | `klist`                                                   | Shows Kerberos tickets on system                     |
| Detect pass-the-hash indicators    | `Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} | ? { $_.Properties[5].Value -like "*NTLM*" }`        | Checks for NTLM auth patterns                        |
| Check service creation events      | `Get-WinEvent -FilterHashtable @{LogName='Security'; ID=7045}` | Detects new service installations                   |
| Check scheduled tasks              | `schtasks /query /fo LIST /v`                              | Detects persistence via scheduled tasks             |
| Net session tracking               | `net session`                                              | Shows active network sessions                        |
| List network connections           | `netstat -ano`                                             | Check suspicious remote connections                  |
| Detect DNS cache poisoning          | `ipconfig /displaydns`                                     | Displays DNS resolver cache                          |

---

### Useful Tools  

| Tool Name         | Purpose                                         | Usage Example                                  |
|-------------------|--------------------------------------------------|-----------------------------------------------|
| **ADUC**           | AD Users & Computers GUI                         | `dsa.msc`                                       |
| **ADSI Edit**      | Advanced AD attributes edit tool                 | `adsiedit.msc`                                  |
| **Event Viewer**   | GUI for viewing logs                             | `eventvwr.msc`                                  |
| **PowerView**      | AD Enumeration (Red/Blue team tool)              | `Import-Module .\PowerView.ps1`                 |
| **BloodHound**     | AD Attack path mapping                           | `bloodhound-python -d domain.local -u user -p pass -ip 10.10.1.5 -c All` |

---
