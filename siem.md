## SIEM 


### SIEM log collection 

- windows: sysmon, event viewer and osquery etc
- linux: all logs stored at /var/log, ex /var/log/httpd

### Important linux logs

|  Logs                                   | Description                                         |
|-----------------------------------------|-----------------------------------------------------|
| /var/log/httpd or /var/log/apache           | All http related logs                   |
| /var/log/auth.log or /var/log/secure        | All authentication related logs         |
| /var/log/cron                               | logs related to scheduled jobs          |
| /var/log/kern                               | kernel related events                   |


### SIEM log ingestion techniques

- agent/ forwarder: a light weight agent installed and forwarder to forward all the logs
- syslog: protocol to collect logs
- manual upload
- port-forwarding: siem system can act as listerners on certain ports

