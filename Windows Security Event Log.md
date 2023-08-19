# Windows Security Event Log

### Troubleshooting queries :
```
// Failed Authentication in last 15 mins (RDP, SMB)
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(15m)
```

```
// Authentication Success in last 15 mins (RDP, SMB)
SecurityEvent
| where EventID == 4624
| where TimeGenerated > ago(15m)
```

```
// Brute Force Attempt 
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by SourceIP = IpAddress, EventID, Activity
| where FailureCount >= 10
```
### Analytic Rule : 
```
// Windows Brute Force Success 
let FailedLogons = SecurityEvent
| where EventID == 4625 and LogonType == 3
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by AttackerIP = IpAddress, EventID, Activity, LogonType, DestinationHostName = Computer
| where FailureCount >= 5;
let SuccessfulLogons = SecurityEvent
| where EventID == 4624 and LogonType == 3
| where TimeGenerated > ago(60m)
| summarize SuccessfulCount = count() by AttackerIP = IpAddress, LogonType, DestinationHostName = Computer, AuthenticationSuccessTime = TimeGenerated;
SuccessfulLogons
| join kind = leftouter FailedLogons on DestinationHostName, AttackerIP, LogonType
| project AuthenticationSuccessTime, AttackerIP, DestinationHostName, FailureCount, SuccessfulCount

```
