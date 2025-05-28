## Windows event logs

---

## basics

- can be catergorized into

| Category      | Explaination                                              |
|------------------|------------------------------------------------------------|
| System logs | any driver, hardware related changes |
| Secuity logs | user login/logoff or any event related to security |
| Application logs | logs related to the applications installed     |
| Directory service events | logs related to keys deletion, modification or creation |
| File replication service events |  Windows service sharing of GPO policies where user access domain controllers |
| DNS events | dns queries and response events |
| Custom logs | application that requires custom size or attach other paramters |

- further can be classifies into

| Category      | Explaination                                              |
|------------------|----------------------------------------|
| Error          | mission critical event such as unscheduled power off   |
| Warning        | not necesary significant but can be problamatic in future |
| Information    | a regular alert                                           |
| Success audit  |  example successfull login                                |
| Failure audit  | an unsuccessful login                                     |

- tools used 

| Tool |
|------|
| Windows Event Viewer |
| Wevtutil.exe         |
| Get-WinEvent         |

- log rotation- action to take when log repo is full. Action can be to overwrite logs, clear logs manually or archieve the old logs.

[logs_to_monitor](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor)

---

## wevtutil.exe

[wevtutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil)

- can be used to install/uninstall manifests, run queries, export archieve or clear logs
- `wevtutil /?` for help
- can further ask help ex- `wevtutil qe /?`

## Get-WinEvent

[Get-WinEvent](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-5.1)

| Command      | Usage                                              |
|------------------|----------------------------------------|
| Get-WinEvent -ListLog *          | List all of the logs   |
| Get-WinEvent -ListProviders *    | List all the providers |
| "Get-WinEvent -ListLog Setup | Format-List -Property *" | get the properties of log file |
| (Get-WinEvent -ListLog Application).ProviderNames       | all the application log providers |
| "(Get-WinEvent -ListProvider Microsoft-Windows-GroupPolicy).Events | Format-Table Id, Description" | event IDS of providers |
| Get-WinEvent -Path /log/path | Get the logs from evtx file |
| Get-WinEvent -Path /log/ | Measure-Object | No of events in evtx file |
| "Get-WinVEent -Path /log/ | Select-Object ExpandProperty ID | Sort-Object | Get-Unique" | Get the unique event IDs |
| "Get-WinEvent -Path /log/ | Group-Object ID" | group by event ID |




- FilterHashtable

```
Get-WinEvent -FilterHashtable @{
  LogName='Application' 
  ProviderName='WLMS' 
}
```

- FilterXPath example

| Command      | Usage                                              |
|------------------|----------------------------------------|
| `Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="System"' -MaxEvents 1`          | List the logs where account name is system and pick only one event   |
| "Get-WinEvent -Path C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Filtering.evtx -FilterXPath '*/System/EventID=3' | Sort-Object TimeCreated | Select-Object -First 1 | Format-List *" | Get the last event from sysmon event ID 3



