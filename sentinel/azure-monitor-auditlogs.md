# Azure Monitor AuditLogs Table

## Overview
Audit log for Azure Active Directory. Includes system activity information about user and group management, managed applications, and directory activities.

## Table Attributes
- **Resource types**: microsoft.azureadgraph/tenants, microsoft.graph/tenants
- **Categories**: Azure Resources, Security
- **Solutions**: LogManagement
- **Basic log**: No
- **Ingestion-time transformation**: Yes

## Schema

| Column | Type | Description |
|--------|------|-------------|
| `AADOperationType` | `string` | Type of the operation. Possible values are Add Update Delete and Other |
| `AADTenantId` | `string` | ID of the ADD tenant |
| `ActivityDateTime` | `datetime` | Date and time the activity was performed in UTC |
| `ActivityDisplayName` | `string` | Activity name or the operation name. Examples include Create User and Add member to group |
| `AdditionalDetails` | `dynamic` | Indicates additional details on the activity |
| `_BilledSize` | `real` | The record size in bytes |
| `Category` | `string` | Currently Audit is the only supported value |
| `CorrelationId` | `string` | Optional GUID that's passed by the client. Can help correlate client-side operations with server-side operations |
| `DurationMs` | `long` | Property is not used and can be ignored |
| `Id` | `string` | GUID that uniquely identifies the activity |
| `Identity` | `string` | Identity from the token that was presented when the request was made |
| `InitiatedBy` | `dynamic` | User or app initiated the activity |
| `_IsBillable` | `string` | Specifies whether ingesting the data is billable |
| `Level` | `string` | Message type. This is currently always Informational |
| `Location` | `string` | Location of the datacenter |
| `LoggedByService` | `string` | Service that initiated the activity |
| `OperationName` | `string` | Name of the operation |
| `OperationVersion` | `string` | REST API version that's requested by the client |
| `Resource` | `string` | Resource information |
| `ResourceGroup` | `string` | Resource group information |
| `ResourceId` | `string` | Resource identifier |
| `ResourceProvider` | `string` | Resource provider information |
| `Result` | `string` | Result of the activity. Possible values are: success failure timeout unknownFutureValue |
| `ResultDescription` | `string` | Additional description of the result |
| `ResultReason` | `string` | Describes cause of failure or timeout results |
| `ResultSignature` | `string` | Property is not used and can be ignored |
| `ResultType` | `string` | Result of the operation. Possible values are Success and Failure |
| `SourceSystem` | `string` | The type of agent the event was collected by |
| `TargetResources` | `dynamic` | Information on which resource was changed due to the activity |
| `TimeGenerated` | `datetime` | Date and time the record was created |
| `Type` | `string` | The name of the table |

## Query Examples

### Get All User Creation Events
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where ActivityDisplayName == "Add user"
| project TimeGenerated, InitiatedBy, TargetResources, Result
```

### Find Failed Operations
```kusto
AuditLogs
| where TimeGenerated > ago(24h)
| where Result == "failure"
| summarize FailureCount = count() by ActivityDisplayName, ResultReason
| order by FailureCount desc
```

### Track Administrative Actions
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where AADOperationType in ("Add", "Update", "Delete")
| extend InitiatorUPN = tostring(parse_json(InitiatedBy).user.userPrincipalName)
| extend InitiatorApp = tostring(parse_json(InitiatedBy).app.displayName)
| project TimeGenerated, ActivityDisplayName, InitiatorUPN, InitiatorApp, Result
```

### Monitor Group Membership Changes
```kusto
AuditLogs
| where TimeGenerated > ago(24h)
| where ActivityDisplayName in ("Add member to group", "Remove member from group")
| extend GroupName = tostring(parse_json(TargetResources)[0].displayName)
| extend ModifiedUser = tostring(parse_json(TargetResources)[1].userPrincipalName)
| project TimeGenerated, ActivityDisplayName, GroupName, ModifiedUser, InitiatedBy
```

### Audit Application Permission Grants
```kusto
AuditLogs
| where TimeGenerated > ago(30d)
| where ActivityDisplayName has "consent"
| extend Application = tostring(parse_json(TargetResources)[0].displayName)
| extend Permissions = tostring(parse_json(TargetResources)[0].modifiedProperties)
| project TimeGenerated, Application, Permissions, InitiatedBy, Result
```

### Track Password Reset Activities
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where ActivityDisplayName has "password"
| extend TargetUser = tostring(parse_json(TargetResources)[0].userPrincipalName)
| extend InitiatorUser = tostring(parse_json(InitiatedBy).user.userPrincipalName)
| project TimeGenerated, ActivityDisplayName, TargetUser, InitiatorUser, Result
```

### Find Role Assignment Changes
```kusto
AuditLogs
| where TimeGenerated > ago(30d)
| where ActivityDisplayName has "role"
| extend RoleName = tostring(parse_json(TargetResources)[0].displayName)
| extend AssignedTo = tostring(parse_json(TargetResources)[1].userPrincipalName)
| project TimeGenerated, ActivityDisplayName, RoleName, AssignedTo, InitiatedBy
```

### Monitor Service Principal Activities
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where InitiatedBy has "servicePrincipal"
| extend ServicePrincipalName = tostring(parse_json(InitiatedBy).app.servicePrincipalName)
| extend ServicePrincipalId = tostring(parse_json(InitiatedBy).app.servicePrincipalId)
| summarize Operations = count() by ServicePrincipalName, ActivityDisplayName
| order by Operations desc
```

### Detect Unusual Activity Patterns
```kusto
AuditLogs
| where TimeGenerated > ago(24h)
| extend Hour = hourofday(TimeGenerated)
| where Hour < 6 or Hour > 22  // Outside business hours
| where AADOperationType in ("Add", "Delete", "Update")
| project TimeGenerated, ActivityDisplayName, InitiatedBy, Result
```

### Track Conditional Access Policy Changes
```kusto
AuditLogs
| where TimeGenerated > ago(30d)
| where ActivityDisplayName has "conditional access"
| extend PolicyName = tostring(parse_json(TargetResources)[0].displayName)
| extend ModifiedBy = tostring(parse_json(InitiatedBy).user.userPrincipalName)
| project TimeGenerated, ActivityDisplayName, PolicyName, ModifiedBy, Result
```

### Summary of Activities by Service
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| summarize ActivityCount = count(), 
           FailureCount = countif(Result == "failure"),
           SuccessRate = round(100.0 * countif(Result == "success") / count(), 2)
    by LoggedByService
| order by ActivityCount desc
```

### Find Deleted Resources
```kusto
AuditLogs
| where TimeGenerated > ago(30d)
| where AADOperationType == "Delete"
| extend DeletedResource = tostring(parse_json(TargetResources)[0].displayName)
| extend DeletedBy = tostring(parse_json(InitiatedBy).user.userPrincipalName)
| project TimeGenerated, ActivityDisplayName, DeletedResource, DeletedBy
```

### Monitor Guest User Activities
```kusto
AuditLogs
| where TimeGenerated > ago(7d)
| where TargetResources contains "#EXT#"  // External/Guest users
| extend GuestUser = tostring(parse_json(TargetResources)[0].userPrincipalName)
| project TimeGenerated, ActivityDisplayName, GuestUser, InitiatedBy, Result
```

### Track MFA Configuration Changes
```kusto
AuditLogs
| where TimeGenerated > ago(30d)
| where ActivityDisplayName has "authentication" or ActivityDisplayName has "MFA"
| extend TargetUser = tostring(parse_json(TargetResources)[0].userPrincipalName)
| project TimeGenerated, ActivityDisplayName, TargetUser, InitiatedBy, Result
```

### Audit Directory Sync Operations
```kusto
AuditLogs
| where TimeGenerated > ago(24h)
| where LoggedByService == "Directory Synchronization"
| summarize SyncCount = count() by bin(TimeGenerated, 1h), Result
| render timechart
```