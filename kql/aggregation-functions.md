# KQL Aggregation Functions

## count() aggregation function

**Syntax:**
```kusto
count()
```

**Description:**
Counts the number of records per summarization group.

**Examples:**
```kusto
StormEvents
| summarize Count=count() by State
```

```kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize EventCount=count() by EventID
```

## avg() aggregation function

**Syntax:**
```kusto
avg(expr)
```

**Parameters:**
- expr: Expression for aggregation (null values ignored)

**Examples:**
```kusto
StormEvents
| summarize AvgDamageToCrops = avg(DamageCrops) by State
```

```kusto
Perf
| where CounterName == "% Processor Time"
| summarize AvgCpuUsage = avg(CounterValue) by Computer
```

## sum() aggregation function

**Syntax:**
```kusto
sum(expr)
```

**Parameters:**
- expr: Expression for aggregation (null values ignored)

**Examples:**
```kusto
StormEvents
| summarize EventCount=count(), TotalDamages = sum(DamageCrops+DamageProperty) by State
| sort by TotalDamages
```

```kusto
Perf
| where CounterName == "Bytes Received/sec"
| summarize TotalBytes = sum(CounterValue) by Computer
```

## min() aggregation function

**Syntax:**
```kusto
min(expr)
```

**Parameters:**
- expr: Expression to find minimum value

**Examples:**
```kusto
StormEvents
| summarize FirstEvent=min(StartTime)
```

```kusto
Heartbeat
| summarize EarliestHeartbeat = min(TimeGenerated) by Computer
```

## max() aggregation function

**Syntax:**
```kusto
max(expr)
```

**Parameters:**
- expr: Expression to find maximum value

**Examples:**
```kusto
StormEvents
| summarize LatestEvent=max(StartTime)
```

```kusto
Perf
| where CounterName == "% Processor Time"
| summarize MaxCpuUsage = max(CounterValue) by Computer
```

## dcount() aggregation function

**Syntax:**
```kusto
dcount(expr [, accuracy])
```

**Parameters:**
- expr: Expression whose distinct values are counted
- accuracy: Estimation accuracy (0-4, default: 1)

**Description:**
Estimates the number of distinct values using HyperLogLog algorithm.

**Accuracy levels:**
- 0: 1.6% error, 2^12 entries
- 1: 0.8% error, 2^14 entries (default)
- 2: 0.4% error, 2^16 entries
- 3: 0.28% error, 2^17 entries
- 4: 0.2% error, 2^18 entries

**Examples:**
```kusto
StormEvents
| summarize DifferentEvents=dcount(EventType) by State
| order by DifferentEvents
```

```kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize UniqueUsers = dcount(Account) by Computer
```

```kusto
SigninLogs
| where TimeGenerated > ago(7d)
| summarize UniqueIPs = dcount(IPAddress, 2)
```