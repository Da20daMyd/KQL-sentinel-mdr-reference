# Microsoft Defender KQL Reference

Comprehensive KQL documentation for Microsoft Defender XDR and Azure Monitor, optimized for Context7 integration.

## Repository Structure

```
kql-reference/
├── kql/                         # Core KQL language documentation
│   ├── README.md               # KQL documentation overview
│   ├── operators.md            # Core operators (sort, take, count, etc.)
│   ├── aggregation-functions.md # Aggregation functions (sum, avg, count)
│   ├── string-functions.md     # String manipulation functions
│   ├── datetime-functions.md   # Date/time functions
│   ├── data-types.md          # KQL data types reference
│   └── kql-complete-reference.md # Complete KQL reference
├── defender/                    # Microsoft Defender XDR documentation
│   ├── mde-table-schemas.md    # Advanced hunting table schemas
│   └── kql-query-examples.md   # Defender-specific query patterns
├── sentinel/                    # Microsoft Sentinel documentation
│   └── azure-monitor-auditlogs.md # AuditLogs table reference
└── context7-kql-catalog.json   # Machine-readable catalog
```

## Core KQL Language (`/kql/`)

Complete KQL language reference including:

- **Operators**: `sort`, `take`, `count`, `distinct`, `parse`, `evaluate`
- **Aggregation Functions**: `count()`, `avg()`, `sum()`, `min()`, `max()`, `dcount()`  
- **String Functions**: `strcat()`, `substring()`, `tolower()`, `toupper()`
- **DateTime Functions**: `ago()`, `now()`, `datetime_add()`, `datetime_diff()`
- **Data Types**: Complete type system documentation

## Microsoft Defender (`/defender/`)

Advanced hunting schemas and queries:

- **40+ Table Schemas** with complete field documentation
- **Detection Queries** for real-time threat identification
- **Hunting Queries** for proactive threat investigation
- **Analytics Queries** for statistical analysis

## Microsoft Sentinel (`/sentinel/`)

Azure Monitor documentation:

- **AuditLogs Table** complete schema and examples
- **Investigation Queries** for Azure AD audit analysis

## Context7 Integration

The `context7-kql-catalog.json` provides:
- Structured metadata for automated ingestion
- Platform identification (defender/sentinel/azure)
- Complexity ratings (basic/intermediate/advanced)
- Table and operator dependencies

## Quick Examples

```kusto
// Basic threat detection
EmailEvents
| where ThreatTypes has "Malware" or ThreatTypes has "Phish"

// Privilege escalation detection  
DeviceProcessEvents
| where ProcessTokenElevation == "TokenElevationTypeFull"
| where InitiatingProcessTokenElevation != "TokenElevationTypeFull"

// Azure audit failures
AuditLogs
| where Result == "failure"
| summarize count() by ActivityDisplayName
```

All queries extracted from Microsoft official documentation, ready for production use.