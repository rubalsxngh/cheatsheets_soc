### Redline 

- redline is fireEye's memory analysis tool and can help in scanning IOC's for endpoint triage
- fireEye helps us with the following: collect registry data( windows only), running processes, collect memory images( below windows 10), collect browser history and look for suspicious strings

[user_guide](https://fireeye.market/assets/apps/211364/documents/877936_en.pdf)

### Redline data collection option

- standard collector - script is configured to collect minimal and req data. It is the fastest method.
- comprehensive collector - script is configured to collect data in depth.
- IOC based collector - we edit IOCs with help of IOC editor.

### edit the script

- redline will give options for memory, disk, system, network and other
- after configuring the script, there will be a file called RunRedlineAudit.bat, run it as admin
- once the audit ran successfully, it will store the file called AnalysisSession1.mans.. AnalysisSession2.mans and so on...

### analysis

- processses- will give on four sections, handles, memory sections, strings and port 
