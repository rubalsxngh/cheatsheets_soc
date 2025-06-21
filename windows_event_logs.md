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

| Command | Description |
|--------|-------------|
| `Get-WinEvent -ListLog *` | List all event logs on the system |
| `Get-WinEvent -ListProvider *` | List all event log providers |
| `Get-WinEvent -ListLog Setup \| Format-List -Property *` | Get all properties of the "Setup" event log |
| `(Get-WinEvent -ListLog Application).ProviderNames` | List all providers associated with the "Application" log |
| `(Get-WinEvent -ListProvider 'Microsoft-Windows-GroupPolicy').Events \| Format-Table Id, Description` | Display event IDs and descriptions for a specific provider |
| `Get-WinEvent -Path "C:\path\to\log.evtx"` | Read and display logs from an EVTX file |
| `Get-WinEvent -Path "C:\path\to\log.evtx" \| Measure-Object` | Count the number of events in the EVTX file |
| `Get-WinEvent -Path "C:\path\to\log.evtx" \| Select-Object -ExpandProperty Id \| Sort-Object \| Get-Unique` | Get a list of unique event IDs |
| `Get-WinEvent -Path "C:\path\to\log.evtx" \| Group-Object Id` | Group events by ID and show count per ID |




- FilterHashtable

```
Get-WinEvent -FilterHashtable @{
  LogName='Application' 
  ProviderName='WLMS' 
}
```

- FilterXPath example

| Command | Description |
|--------|-------------|
| `Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName']="System"' -MaxEvents 1` | List one Security log event where account name is "System" |
| `Get-WinEvent -Path C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Filtering.evtx -FilterXPath '*/System/EventID=3' \| Sort-Object TimeCreated \| Select-Object -First 1 \| Format-List *` | Get the latest Sysmon Event ID 3 from the given log file |



---

### live hunt examples

- check for all the files downloaded using eventID 1 commandline output 

```
Get-WinEvent -LogName Microsoft-Windows-Sysmon/Operational -FilterXPath '*/System/EventID=1' | Where-Object {$_.Message -like '*explorer*'} |
>> ForEach-Object {
>> $xml = [xml]$_.ToXML()
>> $xml.Event.EventData.Data |
>> Where-Object { $_.Name -eq 'CommandLine' } |
>> Select-Object -ExpandProperty "#text"
>> }
```
