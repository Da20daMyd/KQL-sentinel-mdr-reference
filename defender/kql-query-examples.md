# KQL Query Examples for Microsoft Defender

## Detection Queries

### Get User Accounts from Email Addresses
```kusto
EmailEvents
| where Timestamp > ago(7d)
| project RecipientEmailAddress, AccountName = tostring(split(RecipientEmailAddress, "@")[0])
```

### Merge Identity Information with Email Events
```kusto
EmailEvents
| where Timestamp > ago(7d)
| where ThreatTypes has "Malware" or ThreatTypes has "Phish"
| join (IdentityInfo | distinct AccountUpn, AccountDisplayName, JobTitle,
Department, City, Country) on $left.RecipientEmailAddress == $right.AccountUpn
| project Timestamp, NetworkMessageId, Subject, ThreatTypes,
SenderFromAddress, RecipientEmailAddress, AccountDisplayName, JobTitle,
Department, City, Country
```

### Get Device Information for Compromised Account
```kusto
DeviceInfo
| where LoggedOnUsers contains '<account-name>'
| distinct DeviceId
| join kind=inner AlertEvidence on DeviceId
| project AlertId
| join AlertInfo on AlertId
| project AlertId, Timestamp, Title, Severity, Category
```

### Get File Event Information
```kusto
DeviceInfo
| where Timestamp > ago(1d)
| where ClientVersion startswith "20.1"
| summarize by DeviceId
| join kind=inner (
    DeviceFileEvents
    | where Timestamp > ago(1d)
) on DeviceId
| take 10
```

### Get Network Event Information
```kusto
DeviceInfo
| where Timestamp > ago(1d)
| where ClientVersion startswith "20.1"
| summarize by DeviceId
| join kind=inner (
    DeviceNetworkEvents
    | where Timestamp > ago(1d)
) on DeviceId
| take 10
```

## Hunting Queries

### List Logon Activities After Failed ZAP
```kusto
EmailPostDeliveryEvents
| where Timestamp > ago(7d)
| where ActionType has "ZAP" and ActionResult == "Error"
| project ZapTime = Timestamp, ActionType, NetworkMessageId , RecipientEmailAddress
| join kind=inner IdentityLogonEvents on $left.RecipientEmailAddress == $right.AccountUpn
| where Timestamp between ((ZapTime-24h) .. (ZapTime+24h))
| project ZapTime, ActionType, NetworkMessageId , RecipientEmailAddress, AccountUpn,
LogonTime = Timestamp, AccountDisplayName, Application, Protocol, DeviceName, LogonType
```

### Get Logon Attempts After Credential Theft
```kusto
AlertInfo
| where Timestamp > ago(30d)
| where Category == "CredentialAccess"
| join AlertEvidence on AlertId
| extend IsJoined=(parse_json(AdditionalFields).Account.IsDomainJoined)
| extend TargetAccountSid=tostring(parse_json(AdditionalFields).Account.Sid)
| where IsJoined has "true"
| join kind=inner IdentityLogonEvents on $left.TargetAccountSid == $right.AccountSid
| project AccountDisplayName, TargetAccountSid, Application, Protocol, DeviceName, LogonType
```

### Check Files from Malicious Sender on Devices
```kusto
EmailAttachmentInfo
| where SenderFromAddress =~ "MaliciousSender@example.com"
| where isnotempty(SHA256)
| join (
DeviceFileEvents
| project FileName, SHA256, DeviceName, DeviceId
) on SHA256
| project Timestamp, FileName , SHA256, DeviceName, DeviceId,  NetworkMessageId, SenderFromAddress, RecipientEmailAddress
```

### Review Logon Attempts After Malicious Emails
```kusto
let MaliciousEmails=EmailEvents
| where ThreatTypes has "Malware"
| project TimeEmail = Timestamp, Subject, SenderFromAddress, AccountName = tostring(split(RecipientEmailAddress, "@")[0]);
MaliciousEmails
| join (
IdentityLogonEvents
| project LogonTime = Timestamp, AccountName, DeviceName
) on AccountName
| where (LogonTime - TimeEmail) between (0min.. 30min)
| take 10
```

### Review PowerShell Activities After Suspicious Email
```kusto
let EmailsFromBadSender=EmailEvents
| where SenderFromAddress =~ "MaliciousSender@example.com"
| project TimeEmail = Timestamp, Subject, SenderFromAddress, AccountName = tostring(split(RecipientEmailAddress, "@")[0]);
EmailsFromBadSender
| join (
DeviceProcessEvents
| where FileName =~ "powershell.exe"
| project TimeProc = Timestamp, AccountName, DeviceName, InitiatingProcessParentFileName, InitiatingProcessFileName, FileName, ProcessCommandLine
) on AccountName
| where (TimeProc - TimeEmail) between (0min.. 30min)
```

## Investigation Queries

### Find Devices Running Outdated macOS
```kusto
DeviceInfo
| where Timestamp > ago(1d)
| where OSPlatform == "macOS" and  OSVersion !contains "10.15" and OSVersion !contains "11."
| summarize by DeviceId
| join kind=inner (
    DeviceInfo
    | where Timestamp > ago(1d)
) on DeviceId
| take 10
```

### Check Device Onboarding Status
```kusto
DeviceInfo
| where Timestamp > ago(1d)
| where OnboardingStatus != "Onboarded"
| summarize by DeviceId
| join kind=inner (
    DeviceInfo
    | where Timestamp > ago(1d)
) on DeviceId
| take 10
```

### Find Suspicious Process Creation
```kusto
DeviceProcessEvents
| where Timestamp > ago(1h)
| where ProcessIntegrityLevel == "Low" and ProcessTokenElevation == "TokenElevationTypeFull"
| project Timestamp, DeviceName, FileName, ProcessCommandLine, AccountName, InitiatingProcessFileName
```

### Track File Modifications by Process
```kusto
DeviceFileEvents
| where Timestamp > ago(1h)
| where ActionType == "FileModified"
| where InitiatingProcessFileName in~ ("powershell.exe", "cmd.exe", "wscript.exe", "cscript.exe")
| project Timestamp, DeviceName, FileName, FolderPath, InitiatingProcessFileName, InitiatingProcessCommandLine, AccountName
```

### Monitor Network Connections to Suspicious IPs
```kusto
DeviceNetworkEvents
| where Timestamp > ago(1h)
| where RemoteIPType == "Public"
| where RemotePort in (3389, 22, 23, 445, 1433)
| project Timestamp, DeviceName, RemoteIP, RemotePort, InitiatingProcessFileName, AccountName
```

### Detect Registry Persistence Mechanisms
```kusto
DeviceRegistryEvents
| where Timestamp > ago(1h)
| where RegistryKey has @"CurrentVersion\Run" 
    or RegistryKey has @"CurrentVersion\RunOnce"
    or RegistryKey has @"CurrentVersion\RunServices"
| where ActionType == "RegistryValueSet"
| project Timestamp, DeviceName, RegistryKey, RegistryValueName, RegistryValueData, InitiatingProcessFileName, AccountName
```

## Analytics Queries

### Email Threat Summary by Day
```kusto
EmailEvents
| where Timestamp > ago(30d)
| where ThreatTypes != ""
| summarize ThreatCount = count() by ThreatTypes, bin(Timestamp, 1d)
| render timechart
```

### Top Targeted Users
```kusto
EmailEvents
| where Timestamp > ago(7d)
| where ThreatTypes has "Phish" or ThreatTypes has "Malware"
| summarize AttemptCount = count() by RecipientEmailAddress
| top 20 by AttemptCount desc
```

### Device Vulnerability Summary
```kusto
DeviceTvmSoftwareVulnerabilities
| summarize VulnerabilityCount = dcount(CveId) by DeviceName, VulnerabilitySeverityLevel
| order by VulnerabilityCount desc
```

### Failed Logon Attempts by Account
```kusto
IdentityLogonEvents
| where Timestamp > ago(1d)
| where ActionType == "LogonFailed"
| summarize FailedAttempts = count() by AccountName, AccountDomain, FailureReason
| where FailedAttempts > 5
| order by FailedAttempts desc
```

### Process Creation Frequency
```kusto
DeviceProcessEvents
| where Timestamp > ago(1h)
| summarize ProcessCount = count() by FileName, FolderPath
| where ProcessCount > 10
| order by ProcessCount desc
```

## Performance Queries

### Optimize Large Dataset Queries
```kusto
DeviceProcessEvents
| where Timestamp > ago(1h)
| where isnotempty(SHA256)
| summarize arg_min(Timestamp, *) by SHA256, DeviceId
| project-reorder Timestamp, DeviceName, FileName, SHA256
```

### Efficient Join Operations
```kusto
let SuspiciousEmails = EmailEvents
| where Timestamp > ago(1h)
| where ThreatTypes != ""
| distinct NetworkMessageId, RecipientEmailAddress;
SuspiciousEmails
| join kind=inner (
    EmailAttachmentInfo
    | where Timestamp > ago(1h)
) on NetworkMessageId
| project NetworkMessageId, RecipientEmailAddress, FileName, SHA256
```

### Time-Based Aggregation
```kusto
DeviceNetworkEvents
| where Timestamp > ago(1d)
| summarize NetworkEvents = count(), 
            DataTransferred = sum(tolong(AdditionalFields)),
            UniqueRemoteIPs = dcount(RemoteIP)
    by bin(Timestamp, 1h), DeviceName
| order by Timestamp desc
```

## Advanced Hunting Patterns

### Detect Lateral Movement
```kusto
IdentityLogonEvents
| where Timestamp > ago(4h)
| where LogonType in ("RemoteInteractive", "Network")
| where AccountName !endswith "$"
| join kind=inner (
    DeviceProcessEvents
    | where Timestamp > ago(4h)
    | where FileName in~ ("psexec.exe", "wmic.exe", "winrs.exe", "mstsc.exe")
) on DeviceName
| project Timestamp, AccountName, DeviceName, LogonType, FileName, ProcessCommandLine
```

### Find Data Exfiltration Attempts
```kusto
DeviceNetworkEvents
| where Timestamp > ago(1h)
| where RemotePort in (21, 22, 443, 445)
| join kind=inner (
    DeviceFileEvents
    | where Timestamp > ago(1h)
    | where ActionType == "FileCreated" and FileSize > 1048576
) on DeviceId
| where (Timestamp - Timestamp1) between (0min .. 5min)
| project Timestamp, DeviceName, FileName, FileSize, RemoteIP, RemotePort
```

### Detect Privilege Escalation
```kusto
DeviceProcessEvents
| where Timestamp > ago(1h)
| where ProcessTokenElevation == "TokenElevationTypeFull"
| where InitiatingProcessTokenElevation != "TokenElevationTypeFull"
| where InitiatingProcessFileName !in~ ("services.exe", "svchost.exe", "wininit.exe")
| project Timestamp, DeviceName, FileName, ProcessCommandLine, 
    InitiatingProcessFileName, InitiatingProcessCommandLine, AccountName
```

### Monitor Suspicious PowerShell Activities
```kusto
DeviceProcessEvents
| where Timestamp > ago(1h)
| where FileName =~ "powershell.exe"
| where ProcessCommandLine has_any ("-EncodedCommand", "-enc", "-e", 
    "bypass", "hidden", "noprofile", "downloadstring", "invoke-expression")
| project Timestamp, DeviceName, ProcessCommandLine, AccountName, InitiatingProcessFileName
```

### Track Abnormal Service Creation
```kusto
DeviceEvents
| where Timestamp > ago(1h)
| where ActionType == "ServiceInstalled"
| extend ServiceName = tostring(parse_json(AdditionalFields).ServiceName)
| extend ServicePath = tostring(parse_json(AdditionalFields).ServiceFileName)
| where ServicePath has_any ("temp", "appdata", "users", "public")
| project Timestamp, DeviceName, ServiceName, ServicePath, AccountName
```