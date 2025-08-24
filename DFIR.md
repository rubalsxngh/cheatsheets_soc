## Digital Forensics, incident and response.

### basics

- artifacts: evidence that points to the activity performed. for ex- registry hive if hacker manipulated the reg keys
- chain of custody: the evidence should be in proper hands so no one should question the integrity.

### Windows registry forensics 

| Reg hive                                | function                                         |
|-----------------------------------------|-----------------------------------------------------|
| HKEY CURRENT USER  aka HKCU            | store conf info of current logged in user, user folders, display settings and control panel settings |
| HKEY USERS aka HKU                           | store user profiles, its a subkey of HCU             |
| HKEY LOCAL MACHINE aka HKLM                           | conf info of a computer                     |
| HKEY CLASSES ROOT   aka HKCT                           | subkey of HKLM/Software, make sures correct program is opened when needed |
| HKEY CURRENT CONFIG                                    | hardware profile of local computer used during system startup             |
| C:\Windows\AppCompat\Programs\Amcache.hve              | Windows use this hive to store recetly run programs                       |
| C:\Windows\System32\Config\RegBack                     | Registry backups                                                          |
| SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache | to check the backward compatibility                              |
| C:\Windows\appcompat\Programs\Amcache.hve | program execution, contains all info as well as sha 1 |
| SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID} | bam and dam info bro background and desktop activity monitoring         |
| SYSTEM\CurrentControlSet\Enum\USBSTOR or SYSTEM\CurrentControlSet\Enum\USB | USB info                                              |



- some major hives, the below can be found at C:\Windows\Config

| Reg hive                                |
|-----------------------------------------|
| HKEY_USERS\Default          |
| HKEY_LOCAL_MACHINE\SAM      |
| HKEY_LOCAL_MACHINE\Secuirty |
| HKEY_LOCAL_MACHINE\Software |
| HKEY_LOCAL_MACHINE\System   |

- USRCLASS.DAT mounted on HKEY_CURRENT_USER\SOftware\CLASSES present in C:\Users\Username\AppData\Local\Microsoft\Windows
- NTUSER.DAT mounted on HKEY_CURRENT_USER

### popular reg paths

| Reg path                              | function                                         |
|-----------------------------------------|-----------------------------------------------------|
| SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName | computer info                    |
| SYSTEM\CurrentControlSet\Control\TimeZoneInformation       | time zone info                   |
| SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces | all the interfaces network    |
| NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run, NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce,SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce, SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run, SOFTWARE\Microsoft\Windows\CurrentVersion\Run |                   Scheduled tasks               |
| SYSTEM\CurrentControlSet\Services           | all the services                                |
| SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged  | all the past network connections  |
| SAM\Domains\Account\Users                                                 | users info          |
| NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs  | all the recents docs open         |
| NTUSER.DAT\Software\Microsoft\Office\VERSION  | office files              |
| USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags, USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU, NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU, NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags | shellbag, all the user layout info |
| NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU, NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU | open or save dialogs |
| NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count  | user program execution count |
| SOFTWARE\Microsoft\Windows Portable Devices\Devices | the device name |
| WordWheelQuery | check for searches perfomed in file explorer |
| SYSTEM\CurrentControlSet\Services\LanmanServer\Shares | Shared Folders |
| Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU | commands executed in run, in HKLM current user |



### File system forensics

- ntfs: $MFT, $LOGFILE, $usnJournal

- MFTECmd.exe -f /path/to/mft --csv /path/to/csv

| Artifacts                              | function                                         |
|-----------------------------------------|-----------------------------------------------------|
| prefetch files .pf, C:\Windows\Prefetch | PECmd.exe -f <path-to-Prefetch-files> --csv <path-to-save-csv>                    |
| windows recently run applications  | WxTCmd.exe -f C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db --csv <path-to-save-csv>   |
| Windows Jump Lists | JLECmd.exe -f :\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations --csv <path-to-save-csv> |
| Shortcut files | LECmd.exe -f C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\ orC:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\ --csv /path/ |


### linux forensics

- cat /etc/os-version : get the OS info
- etc/passwd : user info
- etc/group : group info 
- etc/sudoers : information about elevated users
- last /var/log/wdmp.log : login attempts
- last /var/log/btmp.log : failed login attemps
- cat /var/log/auth.log 
- /etc/hostname
- /etc/timezone : timezone info
- /etc/network/interfaces
- ip addr show
- netstat -ntap
- /etc/hosts : dns name info
- /etc/resolv.conf : 
- /home/user_name/.bash_histroy : check all the command run on the terminal