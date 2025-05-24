# sysinternals

- 70+ tools used for windows administartion and red teaming

## install systinternals

- two ways, download the suite or live
- suite can be downloaded from [sysinternal_suite_download](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
- to run the tool live, we need to have webclient service running and network discovery must be turned on.
- to insatll webclient use powershell  `Install-WindowsFeature WebDAV-Redirector –Restart`. A restart is needed after install
- verify the webclient install `Get-WindowsFeature WebDAV-Redirector | Format-Table –Autosize`
- run it like `\\live.sysinternals.com\tools\tool_name` or map it to a drive `net * \live.sysinternals.com\tools\`

go throught the doc [sysinternal_suite](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)