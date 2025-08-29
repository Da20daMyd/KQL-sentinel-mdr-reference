# Microsoft Defender for Endpoint (MDE) Table Schemas

## Overview
Microsoft Defender XDR advanced hunting schema contains multiple tables for threat hunting across devices, emails, apps, and identities.

## Core Tables

### DeviceProcessEvents
Process creation and related events on endpoints.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `FileName` | `string` | Name of the file that the recorded action was applied to |
| `FolderPath` | `string` | Folder containing the file that the recorded action was applied to |
| `SHA1` | `string` | SHA-1 of the file that the recorded action was applied to |
| `SHA256` | `string` | SHA-256 of the file that the recorded action was applied to |
| `MD5` | `string` | MD5 hash of the file that the recorded action was applied to |
| `FileSize` | `long` | Size of the file in bytes |
| `ProcessId` | `long` | Process ID (PID) of the newly created process |
| `ProcessCommandLine` | `string` | Command line used to create the new process |
| `ProcessIntegrityLevel` | `string` | Integrity level of the newly created process |
| `ProcessTokenElevation` | `string` | Token elevation type of the newly created process |
| `ProcessCreationTime` | `datetime` | Date and time the process was created |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `LogonId` | `long` | Identifier for a logon session |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessAccountUpn` | `string` | User principal name (UPN) of the account that ran the process responsible for the event |
| `InitiatingProcessAccountObjectId` | `string` | Microsoft Entra object ID of the user account that ran the process responsible for the event |
| `InitiatingProcessLogonId` | `long` | Identifier for a logon session of the process that initiated the event |
| `InitiatingProcessIntegrityLevel` | `string` | Integrity level of the process that initiated the event |
| `InitiatingProcessTokenElevation` | `string` | Token type indicating UAC privilege elevation applied to the process that initiated the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process file that initiated the event |
| `InitiatingProcessFileSize` | `long` | Size of the file that ran the process responsible for the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `InitiatingProcessParentId` | `long` | Process ID (PID) of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentFileName` | `string` | Name of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentCreationTime` | `datetime` | Date and time when the parent of the process responsible for the event was started |
| `InitiatingProcessSignerType` | `string` | Type of file signer of the process (image file) that initiated the event |
| `InitiatingProcessSignatureStatus` | `string` | Information about the signature status of the process (image file) that initiated the event |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AppGuardContainerId` | `string` | Identifier for the virtualized container used by Application Guard to isolate browser activity |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceFileEvents
File creation, modification, and other file system events on endpoints.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `FileName` | `string` | Name of the file that the recorded action was applied to |
| `FolderPath` | `string` | Folder containing the file that the recorded action was applied to |
| `SHA1` | `string` | SHA-1 of the file that the recorded action was applied to |
| `SHA256` | `string` | SHA-256 of the file that the recorded action was applied to |
| `MD5` | `string` | MD5 hash of the file that the recorded action was applied to |
| `FileSize` | `long` | Size of the file in bytes |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process that initiated the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `InitiatingProcessParentId` | `long` | Process ID (PID) of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentFileName` | `string` | Name of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentCreationTime` | `datetime` | Date and time when the parent of the process responsible for the event was started |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceNetworkEvents
Network connection and related events on endpoints.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `RemoteIP` | `string` | IP address that was being connected to |
| `RemotePort` | `int` | TCP port on the remote device that was being connected to |
| `RemoteUrl` | `string` | URL or fully qualified domain name (FQDN) that was being connected to |
| `LocalIP` | `string` | IP address assigned to the local device used during communication |
| `LocalPort` | `int` | TCP port on the local device used during communication |
| `Protocol` | `string` | Protocol used during the communication |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process that initiated the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `InitiatingProcessParentId` | `long` | Process ID (PID) of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentFileName` | `string` | Name of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentCreationTime` | `datetime` | Date and time when the parent of the process responsible for the event was started |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceLogonEvents
Sign-ins and other authentication events on devices.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `LogonType` | `string` | Type of logon session |
| `LogonId` | `long` | Identifier for a logon session |
| `RemoteIP` | `string` | IP address of the remote device that initiated the logon |
| `RemoteDeviceName` | `string` | Name of the device that performed a remote operation on the affected machine |
| `RemotePort` | `int` | TCP port on the remote device that was being connected to |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process that initiated the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceRegistryEvents
Creation and modification of registry entries on endpoints.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `RegistryKey` | `string` | Registry key that the recorded action was applied to |
| `RegistryValueType` | `string` | Data type of the registry value |
| `RegistryValueName` | `string` | Name of the registry value that the recorded action was applied to |
| `RegistryValueData` | `string` | Data of the registry value that the recorded action was applied to |
| `PreviousRegistryKey` | `string` | Original registry key of the registry value before it was modified |
| `PreviousRegistryValueName` | `string` | Original name of the registry value before it was modified |
| `PreviousRegistryValueData` | `string` | Original data of the registry value before it was modified |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process that initiated the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `InitiatingProcessParentId` | `long` | Process ID (PID) of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentFileName` | `string` | Name of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentCreationTime` | `datetime` | Date and time when the parent of the process responsible for the event was started |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceEvents
Multiple event types, including events triggered by security controls such as Microsoft Defender Antivirus and exploit protection.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ActionType` | `string` | Type of activity that triggered the event |
| `FileName` | `string` | Name of the file that the recorded action was applied to |
| `FolderPath` | `string` | Folder containing the file that the recorded action was applied to |
| `SHA1` | `string` | SHA-1 of the file that the recorded action was applied to |
| `SHA256` | `string` | SHA-256 of the file that the recorded action was applied to |
| `MD5` | `string` | MD5 hash of the file that the recorded action was applied to |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `RemoteIP` | `string` | IP address that was being connected to |
| `RemoteUrl` | `string` | URL or fully qualified domain name (FQDN) that was being connected to |
| `InitiatingProcessAccountDomain` | `string` | Domain of the account that ran the process responsible for the event |
| `InitiatingProcessAccountName` | `string` | User name of the account that ran the process responsible for the event |
| `InitiatingProcessAccountSid` | `string` | Security Identifier (SID) of the account that ran the process responsible for the event |
| `InitiatingProcessSHA1` | `string` | SHA-1 hash of the process (image file) that initiated the event |
| `InitiatingProcessSHA256` | `string` | SHA-256 of the process (image file) that initiated the event |
| `InitiatingProcessMD5` | `string` | MD5 hash of the process (image file) that initiated the event |
| `InitiatingProcessFileName` | `string` | Name of the process that initiated the event |
| `InitiatingProcessId` | `long` | Process ID (PID) of the process that initiated the event |
| `InitiatingProcessCommandLine` | `string` | Command line used to run the process that initiated the event |
| `InitiatingProcessCreationTime` | `datetime` | Date and time when the process that initiated the event was started |
| `InitiatingProcessFolderPath` | `string` | Folder containing the process (image file) that initiated the event |
| `InitiatingProcessParentId` | `long` | Process ID (PID) of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentFileName` | `string` | Name of the parent process that spawned the process responsible for the event |
| `InitiatingProcessParentCreationTime` | `datetime` | Date and time when the parent of the process responsible for the event was started |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### DeviceInfo
Machine information, including OS information.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the record was generated |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `ClientVersion` | `string` | Version of the endpoint agent or sensor running on the machine |
| `PublicIP` | `string` | Public IP address used by the onboarded device to connect to the Microsoft Defender for Endpoint service |
| `OSArchitecture` | `string` | Architecture of the operating system running on the device |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `OSBuild` | `string` | Build version of the operating system running on the device |
| `OSVersion` | `string` | Version of the operating system running on the device |
| `OSDistribution` | `string` | Distribution of the OS platform |
| `IsAzureADJoined` | `boolean` | Boolean indicator of whether machine is joined to Microsoft Entra ID |
| `AadDeviceId` | `string` | Unique identifier for the device in Microsoft Entra ID |
| `MachineGroup` | `string` | Machine group used for grouping and organization |
| `DeviceCategory` | `string` | Broader classification that groups certain device types |
| `DeviceType` | `string` | Type of device based on purpose and functionality |
| `DeviceSubtype` | `string` | Additional modifier for certain types of devices |
| `Model` | `string` | Model name or number of the product from the vendor or manufacturer |
| `Vendor` | `string` | Name of the product vendor or manufacturer |
| `LoggedOnUsers` | `string` | List of all users that are logged on the device at the time of the event in JSON array format |
| `RegistryDeviceTag` | `string` | Device tag added through the registry |
| `ReportId` | `long` | Event identifier based on a repeating counter |
| `OnboardingStatus` | `string` | Indicates whether the device is currently onboarded or not to Microsoft Defender for Endpoint |

## Email-Related Tables

### EmailEvents
Microsoft 365 email events, including email delivery and blocking events.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `NetworkMessageId` | `string` | Unique identifier for the email, generated by Microsoft 365 |
| `InternetMessageId` | `string` | Public-facing identifier for the email that is set by the sending email system |
| `SenderFromAddress` | `string` | Sender email address in the FROM header |
| `SenderFromDomain` | `string` | Sender domain in the FROM header |
| `SenderMailFromAddress` | `string` | Sender email address in the MAIL FROM header |
| `SenderMailFromDomain` | `string` | Sender domain in the MAIL FROM header |
| `SenderDisplayName` | `string` | Name of the sender displayed in the address book |
| `SenderObjectId` | `string` | Unique identifier for the sender's account in Microsoft Entra ID |
| `RecipientEmailAddress` | `string` | Email address of the recipient |
| `RecipientObjectId` | `string` | Unique identifier for the email recipient in Microsoft Entra ID |
| `Subject` | `string` | Subject of the email |
| `EmailClusterId` | `string` | Identifier for the group of similar emails clustered based on heuristic analysis of their contents |
| `EmailDirection` | `string` | Direction of the email relative to your network: Inbound, Outbound, Intra-org |
| `DeliveryAction` | `string` | Delivery action of the email: Delivered, Junked, Blocked, or Replaced |
| `DeliveryLocation` | `string` | Location where the email was delivered: Inbox/Folder, On-premises/External, Junk, Quarantine, Failed, Dropped |
| `ThreatTypes` | `string` | Verdict from the email filtering stack on whether the email contains malware, phishing, or other threats |
| `ThreatNames` | `string` | Detection name for malware or other threats found |
| `DetectionMethods` | `string` | Methods used to detect malware, phishing, or other threats found in the email |
| `ConfidenceLevel` | `string` | List of confidence levels of any spam or phishing verdicts |
| `EmailAction` | `string` | Final action taken on the email based on filter verdict, policies, and user actions |
| `EmailActionPolicy` | `string` | Action policy that took effect |
| `EmailActionPolicyGuid` | `string` | Unique identifier of the policy that determined the final mail action |
| `AttachmentCount` | `int` | Number of attachments in the email |
| `UrlCount` | `int` | Number of embedded URLs in the email |
| `EmailLanguage` | `string` | Detected language of the email content |
| `AuthenticationDetails` | `string` | Details about authentication checks for the email |
| `ReportId` | `string` | Unique event identifier |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |

### EmailAttachmentInfo
Information about files attached to emails.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `NetworkMessageId` | `string` | Unique identifier for the email, generated by Microsoft 365 |
| `SenderFromAddress` | `string` | Sender email address in the FROM header |
| `SenderDisplayName` | `string` | Name of the sender displayed in the address book |
| `SenderObjectId` | `string` | Unique identifier for the sender's account in Microsoft Entra ID |
| `RecipientEmailAddress` | `string` | Email address of the recipient |
| `RecipientObjectId` | `string` | Unique identifier for the email recipient in Microsoft Entra ID |
| `FileName` | `string` | Name of the file that the recorded action was applied to |
| `FileType` | `string` | File extension type |
| `SHA256` | `string` | SHA-256 of the file |
| `FileSize` | `long` | Size of the file in bytes |
| `ThreatTypes` | `string` | Verdict from the email filtering stack on whether the file contains malware, phishing, or other threats |
| `ThreatNames` | `string` | Detection name for malware or other threats found |
| `DetectionMethods` | `string` | Methods used to detect malware, phishing, or other threats found in the email |
| `ReportId` | `string` | Unique event identifier |

### EmailUrlInfo
Information about URLs on emails.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `NetworkMessageId` | `string` | Unique identifier for the email, generated by Microsoft 365 |
| `Url` | `string` | Full URL in the email |
| `UrlDomain` | `string` | Domain name or host name of the URL |
| `ReportId` | `string` | Unique event identifier |

### EmailPostDeliveryEvents
Security events that occur post-delivery, after Microsoft 365 delivers the emails to the recipient mailbox.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `NetworkMessageId` | `string` | Unique identifier for the email, generated by Microsoft 365 |
| `InternetMessageId` | `string` | Public-facing identifier for the email that is set by the sending email system |
| `Action` | `string` | Action taken on the entity |
| `ActionType` | `string` | Type of activity that triggered the event: Manual remediation, Phish ZAP, Malware ZAP |
| `ActionTrigger` | `string` | Indicates whether an action was triggered by an administrator or by approval of a pending action |
| `ActionResult` | `string` | Result of the action |
| `RecipientEmailAddress` | `string` | Email address of the recipient |
| `DeliveryLocation` | `string` | Location where the email was delivered |
| `ReportId` | `string` | Unique event identifier |

## Identity and Authentication Tables

### IdentityLogonEvents
Authentication events on Active Directory and Microsoft online services.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `ActionType` | `string` | Type of activity that triggered the event |
| `Application` | `string` | Application that performed the recorded action |
| `LogonType` | `string` | Type of logon session, specifically interactive, remote interactive (RDP), network, batch, and service |
| `Protocol` | `string` | Network protocol used |
| `FailureReason` | `string` | Information explaining why the recorded action failed |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `AccountDisplayName` | `string` | Name of the account user displayed in the address book |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `DeviceType` | `string` | Type of device |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `IPAddress` | `string` | IP address assigned to the endpoint and used during related network communications |
| `Port` | `int` | TCP port used during communication |
| `DestinationDeviceName` | `string` | Name of the device running the server application that processed the recorded action |
| `DestinationIPAddress` | `string` | IP address of the device running the server application that processed the recorded action |
| `DestinationPort` | `int` | Destination port of related network communications |
| `TargetAccountUpn` | `string` | User principal name (UPN) of the account that the recorded action was applied to |
| `TargetAccountObjectId` | `string` | Microsoft Entra object ID of the account that the recorded action was applied to |
| `TargetAccountDisplayName` | `string` | Display name of the account that the recorded action was applied to |
| `Location` | `string` | City, country, or other geographic location associated with the event |
| `ISP` | `string` | Internet service provider associated with the IP address |
| `ReportId` | `string` | Unique identifier for the event |
| `AdditionalFields` | `string` | Additional information about the entity or event |

### IdentityInfo
Account information from various sources, including Microsoft Entra ID.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `AccountDisplayName` | `string` | Name of the account user displayed in the address book |
| `AccountDomain` | `string` | Domain of the account |
| `EmailAddress` | `string` | Email address associated with the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `CloudSid` | `string` | Cloud Security Identifier of the account |
| `GivenName` | `string` | Given name or first name of the account user |
| `Surname` | `string` | Surname, family name, or last name of the account user |
| `Department` | `string` | Name of the department that the account user belongs to |
| `JobTitle` | `string` | Job title of the account user |
| `City` | `string` | City where the account user is located |
| `Country` | `string` | Country where the account user is located |
| `IsAccountEnabled` | `boolean` | Indicates whether the account is enabled or not |
| `ReportId` | `string` | Unique identifier for the event |

### IdentityQueryEvents
Queries for Active Directory objects, such as users, groups, devices, and domains.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `ActionType` | `string` | Type of activity that triggered the event |
| `Application` | `string` | Application that performed the recorded action |
| `QueryType` | `string` | Type of query, such as QueryGroup, QueryUser, or EnumerateUsers |
| `QueryTarget` | `string` | Name of user, group, device, domain, or any other entity type being queried |
| `Query` | `string` | String used to run the query |
| `Protocol` | `string` | Protocol used during the communication |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `IPAddress` | `string` | IP address assigned to the endpoint and used during related network communications |
| `DestinationDeviceName` | `string` | Name of the device running the server application that processed the recorded action |
| `DestinationIPAddress` | `string` | IP address of the device running the server application that processed the recorded action |
| `DestinationPort` | `int` | Destination port of related network communications |
| `TargetAccountUpn` | `string` | User principal name (UPN) of the account that the recorded action was applied to |
| `TargetAccountObjectId` | `string` | Microsoft Entra object ID of the account that the recorded action was applied to |
| `TargetAccountDisplayName` | `string` | Display name of the account that the recorded action was applied to |
| `Location` | `string` | City, country, or other geographic location associated with the event |
| `ReportId` | `string` | Unique identifier for the event |
| `AdditionalFields` | `string` | Additional information about the entity or event |

### IdentityDirectoryEvents
Events involving an on-premises domain controller running Active Directory (AD).

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `ActionType` | `string` | Type of activity that triggered the event |
| `Application` | `string` | Application that performed the recorded action |
| `TargetAccountUpn` | `string` | User principal name (UPN) of the account that the recorded action was applied to |
| `TargetAccountDisplayName` | `string` | Display name of the account that the recorded action was applied to |
| `TargetDeviceName` | `string` | Fully qualified domain name (FQDN) of the device that the recorded action was applied to |
| `DestinationDeviceName` | `string` | Name of the device running the server application that processed the recorded action |
| `DestinationIPAddress` | `string` | IP address of the device running the server application that processed the recorded action |
| `DestinationPort` | `int` | Destination port of the activity |
| `Protocol` | `string` | Protocol used during the communication |
| `AccountDomain` | `string` | Domain of the account |
| `AccountName` | `string` | User name of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `IPAddress` | `string` | IP address assigned to the device during communication |
| `Port` | `int` | TCP port used during communication |
| `Location` | `string` | City, country, or other geographic location associated with the event |
| `ISP` | `string` | Internet service provider associated with the IP address |
| `ReportId` | `string` | Unique identifier for the event |
| `AdditionalFields` | `string` | Additional information about the entity or event |

## Alert Tables

### AlertInfo
Alerts from Microsoft Defender for Endpoint, Microsoft Defender for Office 365, Microsoft Defender for Cloud Apps, and Microsoft Defender for Identity.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `AlertId` | `string` | Unique identifier for the alert |
| `Title` | `string` | Title of the alert |
| `Category` | `string` | Type of threat indicator or breach activity identified by the alert |
| `Severity` | `string` | Indicates the potential impact (high, medium, or low) of the threat indicator or breach activity identified by the alert |
| `ServiceSource` | `string` | Product or service that provided the alert information |
| `DetectionSource` | `string` | Detection technology or sensor that identified the notable component or activity |
| `AttackTechniques` | `string` | MITRE ATT&CK techniques associated with the activity that triggered the alert |

### AlertEvidence
Files, IP addresses, URLs, users, or devices associated with alerts.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `AlertId` | `string` | Unique identifier for the alert |
| `EntityType` | `string` | Type of object, such as a file, a process, a device, or a user |
| `EvidenceRole` | `string` | How the entity is involved in an alert, indicating whether it is impacted or is merely related |
| `EvidenceDirection` | `string` | Indicates whether the entity is the source or the destination of a network connection |
| `FileName` | `string` | Name of the file that the recorded action was applied to |
| `FolderPath` | `string` | Folder containing the file that the recorded action was applied to |
| `SHA1` | `string` | SHA-1 of the file that the recorded action was applied to |
| `SHA256` | `string` | SHA-256 of the file that the recorded action was applied to |
| `FileSize` | `int` | Size of the file in bytes |
| `ThreatFamily` | `string` | Malware family that the suspicious or malicious file or process has been classified under |
| `RemoteIP` | `string` | IP address that was being connected to |
| `RemoteUrl` | `string` | URL or fully qualified domain name (FQDN) that was being connected to |
| `AccountName` | `string` | User name of the account |
| `AccountDomain` | `string` | Domain of the account |
| `AccountSid` | `string` | Security Identifier (SID) of the account |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `AccountUpn` | `string` | User principal name (UPN) of the account |
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `LocalIP` | `string` | IP address assigned to the local device used during communication |
| `NetworkMessageId` | `string` | Unique identifier for the email, generated by Microsoft 365 |
| `EmailSubject` | `string` | Subject of the email |
| `ApplicationId` | `int` | Unique identifier for the application |
| `Application` | `string` | Application that performed the recorded action |
| `ProcessCommandLine` | `string` | Command line used to create the new process |
| `AdditionalFields` | `string` | Additional information about the event in JSON array format |
| `RegistryKey` | `string` | Registry key that the recorded action was applied to |
| `RegistryValueName` | `string` | Name of the registry value that the recorded action was applied to |
| `RegistryValueData` | `string` | Data of the registry value that the recorded action was applied to |

## Cloud App Tables

### CloudAppEvents
Events involving accounts and objects in Office 365 and other cloud apps and services.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `Timestamp` | `datetime` | Date and time when the event was recorded |
| `ActionType` | `string` | Type of activity that triggered the event |
| `Application` | `string` | Application that performed the recorded action |
| `ApplicationId` | `int` | Unique identifier for the application |
| `AccountObjectId` | `string` | Unique identifier for the account in Microsoft Entra ID |
| `AccountDisplayName` | `string` | Name of the account user displayed in the address book |
| `IsAdminOperation` | `boolean` | Indicates whether the activity was performed by an administrator |
| `DeviceType` | `string` | Type of device based on purpose and functionality |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `IPAddress` | `string` | IP address assigned to the endpoint and used during related network communications |
| `IsAnonymousProxy` | `boolean` | Indicates whether the IP address belongs to a known anonymous proxy |
| `CountryCode` | `string` | Two-letter code indicating the country where the client IP address is geolocated |
| `City` | `string` | City where the client IP address is geolocated |
| `ISP` | `string` | Internet service provider associated with the IP address |
| `UserAgent` | `string` | User agent information from the web browser or other client application |
| `ActivityType` | `string` | Type of activity that triggered the event |
| `ActivityObjects` | `string` | List of objects, such as files or folders, that were involved in the recorded activity |
| `ObjectName` | `string` | Name of the object that the recorded action was applied to |
| `ObjectType` | `string` | Type of object, such as a file, a folder, or a user |
| `ObjectId` | `string` | Unique identifier of the object that the recorded action was applied to |
| `ReportId` | `string` | Unique identifier for the event |
| `RawEventData` | `string` | Raw event information from the source application or service in JSON format |
| `AdditionalFields` | `string` | Additional information about the entity or event |

## Threat Intelligence Tables

### DeviceTvmSoftwareVulnerabilities
Software vulnerabilities found on devices and the list of available security updates that address each vulnerability.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `OSVersion` | `string` | Version of the operating system running on the device |
| `OSArchitecture` | `string` | Architecture of the operating system running on the device |
| `SoftwareVendor` | `string` | Name of the software vendor |
| `SoftwareName` | `string` | Name of the software product |
| `SoftwareVersion` | `string` | Version number of the software product |
| `CveId` | `string` | Unique identifier assigned to the security vulnerability under the Common Vulnerabilities and Exposures (CVE) system |
| `VulnerabilitySeverityLevel` | `string` | Severity level assigned to the security vulnerability based on the CVSS score and dynamic factors influenced by the threat landscape |
| `RecommendedSecurityUpdate` | `string` | Name or description of the security update provided by the software vendor to address the vulnerability |
| `RecommendedSecurityUpdateId` | `string` | Identifier of the applicable security updates or identifier for the corresponding guidance or knowledge base (KB) articles |

### DeviceTvmSoftwareVulnerabilitiesKB
Knowledge base of publicly disclosed vulnerabilities, including whether exploit code is publicly available.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `CveId` | `string` | Unique identifier assigned to the security vulnerability under the Common Vulnerabilities and Exposures (CVE) system |
| `CvssScore` | `decimal` | Common Vulnerability Scoring System (CVSS) score assigned to the security vulnerability |
| `IsExploitAvailable` | `boolean` | Indicates whether exploit code for the vulnerability is publicly available |
| `VulnerabilitySeverityLevel` | `string` | Severity level assigned to the security vulnerability based on the CVSS score and dynamic factors influenced by the threat landscape |
| `LastModifiedTime` | `datetime` | Date and time the item or related metadata was last modified |
| `PublishedDate` | `datetime` | Date vulnerability was disclosed to public |
| `VulnerabilityDescription` | `string` | Description of the vulnerability and associated risks |
| `AffectedSoftware` | `string` | List of all software products affected by the vulnerability |

### DeviceTvmSoftwareInventory
Inventory of software installed on devices, including their version information and end-of-support status.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `OSVersion` | `string` | Version of the operating system running on the device |
| `OSArchitecture` | `string` | Architecture of the operating system running on the device |
| `SoftwareVendor` | `string` | Name of the software vendor |
| `SoftwareName` | `string` | Name of the software product |
| `SoftwareVersion` | `string` | Version number of the software product |
| `NumberOfWeaknesses` | `int` | Number of weaknesses on the software product on the device |
| `DiskPaths` | `string` | Disk evidence that the product is installed on the device |
| `RegistryPaths` | `string` | Registry evidence that the product is installed on the device |
| `SoftwareFirstSeenTimestamp` | `datetime` | First time this software was seen on the device |
| `EndOfSupportStatus` | `string` | Indicates the lifecycle stage of the software product relative to its specified end-of-support (EOS) or end-of-life (EOL) date |
| `EndOfSupportDate` | `datetime` | End-of-support (EOS) or end-of-life (EOL) date of the software product |

### DeviceTvmSecureConfigurationAssessment
Microsoft Defender Vulnerability Management assessment events, indicating the status of various security configurations on devices.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `DeviceId` | `string` | Unique identifier for the device in the service |
| `DeviceName` | `string` | Fully qualified domain name (FQDN) of the device |
| `OSPlatform` | `string` | Platform of the operating system running on the device |
| `Timestamp` | `datetime` | Date and time when the record was generated |
| `ConfigurationId` | `string` | Unique identifier for a specific configuration |
| `ConfigurationCategory` | `string` | Category or grouping to which the configuration belongs |
| `ConfigurationSubcategory` | `string` | Subcategory or subgrouping to which the configuration belongs |
| `ConfigurationImpact` | `decimal` | Rated impact of the configuration to the overall configuration score (1-10) |
| `IsCompliant` | `boolean` | Indicates whether the configuration or policy is properly configured |
| `IsApplicable` | `boolean` | Indicates whether the configuration or policy applies to the device |
| `Context` | `string` | Additional contextual information about the configuration or policy |
| `IsExpectedUserImpact` | `boolean` | Indicates whether there will be user impact if the configuration or policy will be applied |

### DeviceTvmSecureConfigurationAssessmentKB
Knowledge base of various security configurations used by Microsoft Defender Vulnerability Management to assess devices.

#### Schema
| Column | Type | Description |
|--------|------|-------------|
| `ConfigurationId` | `string` | Unique identifier for a specific configuration |
| `ConfigurationName` | `string` | Display name of the configuration |
| `ConfigurationDescription` | `string` | Description of the configuration |
| `RiskDescription` | `string` | Description of the associated risk |
| `ConfigurationCategory` | `string` | Category or grouping to which the configuration belongs |
| `ConfigurationSubcategory` | `string` | Subcategory or subgrouping to which the configuration belongs |
| `ConfigurationBenchmarks` | `string` | List of industry benchmarks recommending the same or similar configuration |
| `Tags` | `string` | Labels representing various attributes used to identify or categorize a security configuration |
| `RemediationOptions` | `string` | Recommended actions to reduce or address any associated risks |