# core windows processes

- use tools like process hacker, visible parent child relation 

## System

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\ntoskrnl.exe (NT OS Kernel) |
| PID        | 4                                               |
| Parent process | Sytem idle process(0)                     |
| Instances on   | single instance                             |
| Start time     | At boot                                     |


--- 

## System > smss.exe

- session mamanger subsystem or windows session manager 
- first user mode process started by the kernel
- created 4 processes (csrss.exe and wininit.exe in session 0) and (csrss.exe and winlogon.exe in session 1)
- SMSS is also responsible for creating environment variables, virtual memory paging files and starts winlogon.exe (the Windows Logon Manager).


| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\smss.exe |
| Parent process | System (4)                     |
| Instances on   | two instances per session, once parent one child             |
| start time     | Winthin one second after boot

--- 


## csrss.exe

- this is user side of process. responsible for Win32 console and process thread creation
- responsible for making the Windows API available to other processes, mapping drive letters, and handling the Windows shutdown process

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\csrss.exe |
| Parent process | No existatant parent (488)                    |
| Instances on   | two instances            |
| Start time     | After one second of boot |

---


## wininit.exe

- responsible for windows services startup, launches services.exe, lsass.exe and lsaiso.exe

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\wininit.exe |
| Parent process | No existatant parent (488)                    |
| Instances on   | one            |
| Start time     | After one second of boot |


---

## service control manager or services.exe

- primarly response for loading, interacting, starting or ending the services
- also maintains a db table called sc.exe
- launches couple of new serviced such as svchost.exe, spoolsv.exe, msmpeng.exe, and dllhost.exe

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\services.exe |
| Parent process | wininit.exe                    |
| Instances on   | one            |
| Start time     | After one second of boot |


---

## svchost.exe

- responsible to host windows services 

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\svchost.exe |
| Parent process | services.exe                    |
| Instances on   | User Account: Varies (SYSTEM, Network Service, Local Service) depending on the svchost.exe instance. In Windows 10, some instances run as the logged-in user.       |
| Start time     | After one second of boot |

* remember to check for -k paramter in command line. C:\Windows\system32\svchost.exe -k DcomLaunch -p

## Local security authority subsystem service or lsass.exe

- used for security, SAM account login, AD or netlogon, also writes to Windows security logs
- it uses authentication package from HKLM\System\CurrentControlSet\Control\Lsa.

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\lsass.exe |
| Parent process | wininit.exe                    |
| Instances on   | 1       |
| Start time     | After one second of boot |


## winlogon

- responsible to handle SAS secure attention sequence. 
- handles loading of user profiles, loads the user's NTUSER.DAT into HKCU, and userinit.exe loads the user's shell
- responsible for locking the screen and running the user's screensaver, among other functions.


| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\system32\winlogon.exe |
| Parent process | No existatant parent (488)                    |
| Instances on   | one or more          |
| Start time     | After one second of boot |


## explorer.exe

- winlogon.exe-> userinit.exe-> explorer.exe, userinit.exe terminates after spawning explorer.exe
- repsonsible for file and folder access, taskbar etc

| Type      | Value                                              |
|------------------|------------------------------------------------------------|
| Image path | C:\Windows\explorer.exe |
| Parent process | No existatant parent (4008)                    |
| Instances on   | one or more          |
| Start time     | After first user interactive session begins |

* it should not be running as unknown user
* no outbound TCP/IP connections
