# KQL DateTime Functions

## ago() function

**Syntax:**
```kusto
ago(timespan)
```

**Parameters:**
- timespan: Time interval to subtract from current UTC time

**Description:**
Subtracts the given timespan from current UTC time. Multiple uses in same query return same reference time.

**Examples:**
```kusto
T | where Timestamp > ago(1h)
```

```kusto
SecurityEvent
| where TimeGenerated > ago(24h)
| count
```

```kusto
Heartbeat
| where TimeGenerated between (ago(7d) .. ago(1d))
| summarize count() by Computer
```

```kusto
SigninLogs  
| where TimeGenerated > ago(30m)
| where ResultType != 0
```

## now() function

**Syntax:**
```kusto
now([offset])
```

**Parameters:**
- offset: Optional timespan to add to current UTC time (default: 0)

**Description:**
Returns current UTC time, optionally with offset. Same reference time across all uses in single query.

**Examples:**
```kusto
print now()
```

```kusto
print now(-2d)
```

```kusto
StormEvents
| extend Elapsed=now() - StartTime
| take 10
```

```kusto
SecurityEvent
| extend TimeSinceEvent = now() - TimeGenerated
| where TimeSinceEvent < 1h
```

## datetime_add() function

**Syntax:**
```kusto
datetime_add(period, amount, datetime)
```

**Parameters:**
- period: Time unit (Year, Quarter, Month, Week, Day, Hour, Minute, Second, Millisecond, Microsecond, Nanosecond)
- amount: Number of periods to add (can be negative)
- datetime: Base datetime value

**Examples:**
```kusto
print  year = datetime_add('year',1,make_datetime(2017,1,1)),
quarter = datetime_add('quarter',1,make_datetime(2017,1,1)),
month = datetime_add('month',1,make_datetime(2017,1,1)),
week = datetime_add('week',1,make_datetime(2017,1,1)),
day = datetime_add('day',1,make_datetime(2017,1,1)),
hour = datetime_add('hour',1,make_datetime(2017,1,1)),
minute = datetime_add('minute',1,make_datetime(2017,1,1)),
second = datetime_add('second',1,make_datetime(2017,1,1))
```

```kusto
print  year = datetime_add('year',-5,make_datetime(2017,1,1)),
quarter = datetime_add('quarter',12,make_datetime(2017,1,1)),
month = datetime_add('month',-15,make_datetime(2017,1,1)),
week = datetime_add('week',100,make_datetime(2017,1,1))
```

```kusto
SecurityEvent
| extend FutureTime = datetime_add('hour', 24, TimeGenerated)
| project Computer, TimeGenerated, FutureTime
```

## datetime_diff() function

**Syntax:**
```kusto
datetime_diff(period, datetime1, datetime2)
```

**Parameters:**
- period: Time unit (Year, Quarter, Month, Week, Day, Hour, Minute, Second, Millisecond, Microsecond, Nanosecond)
- datetime1: Left-hand side of subtraction
- datetime2: Right-hand side of subtraction

**Description:**
Calculates number of periods between two datetime values (datetime1 - datetime2).

**Examples:**
```kusto
print
year = datetime_diff('year',datetime(2017-01-01),datetime(2000-12-31)),
quarter = datetime_diff('quarter',datetime(2017-07-01),datetime(2017-03-30)),
month = datetime_diff('month',datetime(2017-01-01),datetime(2015-12-30)),
week = datetime_diff('week',datetime(2017-10-29 00:00),datetime(2017-09-30 23:59)),
day = datetime_diff('day',datetime(2017-10-29 00:00),datetime(2017-09-30 23:59)),
hour = datetime_diff('hour',datetime(2017-10-31 01:00),datetime(2017-10-30 23:59)),
minute = datetime_diff('minute',datetime(2017-10-30 23:05:01),datetime(2017-10-30 23:00:59)),
second = datetime_diff('second',datetime(2017-10-30 23:00:10.100),datetime(2017-10-30 23:00:00.900))
```

```kusto
SecurityEvent
| extend EventAge = datetime_diff('hour', now(), TimeGenerated)
| where EventAge < 24
```

```kusto
SigninLogs
| extend SessionDuration = datetime_diff('minute', TimeGenerated, CreatedDateTime)
| where SessionDuration > 0
```