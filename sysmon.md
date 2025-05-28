## sysmon 

### basic

- a sysiternal tool and a windows device driver once installed can give more granular control and can even perist a reboot.

- sysmon needs a config file: [Swistonsec_config_file](https://github.com/SwiftOnSecurity/sysmon-config)
[ion_strom_config_file](https://github.com/ion-storm/sysmon-config/blob/develop/sysmonconfig-export.xml)

- in windows event viewer, symon logs can be found under 'applications and services logs/microsoft/windows/sysmon/operational

- 20 event IDs which can be further configured on how to log

### install sysmom

- download sysmon: [Microsoft_doc_to download binart](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

- start sysmon: `Sysmon.exe -accepteula -i ..\Configurations\swift.xml`


### hunt examples

- hunting metasploit: can investigate for network connection to port 4444 or 5555. Meterpreter shell's default port is 4444, so it makes sense!!
- Get-WinEvent -Path /logs/ -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"]=4444'

### hunting mimikatz

- mimikatz is tools used for cred dumping, mimikatz target lsass.exe, lets hunting any rogue process accessing lsass
- Get-WinEvent -Path /logs/ -FilterXPath '/System/EventID= 10 and */EventData/Data[@Name="TargetImage"]="lsass.exe"'

### Defensive envasion

- there are multiple techniques for defensice evasion
- using ADS, filter event ID 15 and look for ADS locations /temp, /Downloads and /Startup and file ending with '.hta' and '.bat'
- adversaries created room threads to make process look legit, look for event ID 8 and 1.

