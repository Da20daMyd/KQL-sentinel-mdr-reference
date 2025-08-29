# KQL Complete Reference for Context7

## Data Types

```kql
// Boolean
let flag = true;
let disabled = false;

// Numbers
let integer = 42;
let longNum = 9223372036854775807;
let realNum = 3.14159;
let decimalNum = decimal(123.456);

// Strings
let text = "Hello, World!";
let multiline = @"Line 1
Line 2";

// DateTime and TimeSpan
let timestamp = datetime(2023-01-01 12:00:00);
let currentTime = now();
let duration = 2d + 3h + 45m + 30s;

// Dynamic (JSON-like)
let jsonObj = dynamic({"name":"Alice", "age":30});
let jsonArray = dynamic([1,2,3,4,5]);

// GUID
let uniqueId = guid(74be27de-1e4e-49d9-b579-fe0b331d3642);
let newId = new_guid();

// Null handling
let nullValue = real(null);
```

## Core Operators

### where - Filter rows
```kql
TableName
| where Column == "value"
| where Number > 100
| where Timestamp between (ago(7d) .. now())
| where Column in ("value1", "value2", "value3")
| where Column contains "text"
| where Column startswith "prefix"
| where Column matches regex @"\d{3}-\d{4}"
```

### summarize - Aggregate data
```kql
TableName
| summarize Count = count() by Category
| summarize Sum = sum(Value), Avg = avg(Value) by bin(Timestamp, 1h)
| summarize Min = min(Value), Max = max(Value), Percentile = percentile(Value, 95)
| summarize DistinctCount = dcount(UserId)
| summarize Values = make_list(Value), UniqueValues = make_set(Value)
```

### join - Combine tables
```kql
// Inner join
Table1
| join kind=inner (Table2) on Key

// Left outer join
Table1
| join kind=leftouter (Table2) on Key

// Time window join
Table1
| join kind=inner (Table2) on UserId, $left.Time == $right.Time

// Multi-key join
Table1
| join kind=inner (Table2) on Key1, Key2
```

### project - Select columns
```kql
TableName
| project Column1, Column2, Column3
| project NewName = OldColumn, Calculated = Column1 + Column2
| project-away UnwantedColumn
| project-keep Column*
| project-rename NewName = OldName
| project-reorder Column3, Column1, Column2
```

### extend - Add calculated columns
```kql
TableName
| extend NewColumn = Expression
| extend Hour = datetime_part("hour", Timestamp)
| extend IsWeekend = dayofweek(Timestamp) in (0d, 6d)
| extend ParsedJson = parse_json(JsonColumn)
| extend Field = ParsedJson.field
```

### parse - Extract structured data
```kql
// Simple parse
TableName
| parse Message with * "Error: " ErrorCode:int " - " ErrorMessage

// Regex parse
TableName
| parse Message with * @"(\d{3})-(\d{4})" AreaCode:string "-" LocalNumber:string *

// Parse JSON
TableName
| extend Parsed = parse_json(JsonColumn)
| extend Name = Parsed.name, Age = Parsed.age

// Parse CSV
TableName
| parse CsvData with Field1:string "," Field2:int "," Field3:datetime
```

### sort/order - Sort results
```kql
TableName
| sort by Column desc
| order by Timestamp asc, Value desc
| top 10 by Value
| top-nested 5 of Category by sum(Value), top-nested 3 of Product by avg(Rating)
```

### take/limit - Limit rows
```kql
TableName
| take 100
| limit 50
| sample 10
| sample-distinct 5 of Category
```

### distinct - Unique values
```kql
TableName
| distinct Column
| distinct Column1, Column2
| summarize by Column1, Column2  // Same as distinct
```

### count - Count rows
```kql
TableName
| count
| summarize TotalCount = count()
| summarize CountByCategory = count() by Category
```

### union - Combine tables vertically
```kql
Table1
| union Table2
| union kind=outer Table2, Table3
| union withsource=TableName Table1, Table2
```

### evaluate - Advanced analytics
```kql
// Pivot
TableName
| evaluate pivot(Category, sum(Value))

// Bag unpack
TableName
| evaluate bag_unpack(Properties)

// Time series analysis
TableName
| make-series Value = sum(Metric) on Timestamp step 1h
| evaluate series_decompose(Value)
```

## Aggregation Functions

```kql
TableName
| summarize 
    // Count functions
    TotalCount = count(),
    ConditionalCount = countif(Condition),
    DistinctCount = dcount(Column),
    DistinctCountIf = dcountif(Column, Condition),
    
    // Statistical functions
    Average = avg(Value),
    AverageIf = avgif(Value, Condition),
    Sum = sum(Value),
    SumIf = sumif(Value, Condition),
    Min = min(Value),
    Max = max(Value),
    StdDev = stdev(Value),
    Variance = variance(Value),
    
    // Percentiles
    Median = percentile(Value, 50),
    P90 = percentile(Value, 90),
    P95 = percentile(Value, 95),
    P99 = percentile(Value, 99),
    
    // Collection functions
    List = make_list(Value),
    Set = make_set(Value),
    Bag = make_bag(pack(Key, Value)),
    
    // Advanced
    ArgMin = arg_min(Value, *),
    ArgMax = arg_max(Value, *),
    HLL = hll(Column),
    TDigest = tdigest(Value)
```

## String Functions

```kql
// Concatenation
print strcat("Hello", " ", "World")
print strcat_array(dynamic(["A", "B", "C"]), "-")

// Substring
print substring("Hello World", 0, 5)
print substring("Hello World", 6)

// Case conversion
print tolower("HELLO")
print toupper("hello")

// Length and emptiness
print strlen("Hello")
print isempty("")
print isnotempty("text")

// Trimming
print trim(" text ")
print trim_start(" text")
print trim_end("text ")

// Splitting and joining
print split("a,b,c", ",")
print strcat_delim(",", "a", "b", "c")

// Pattern matching
print "text" contains "ex"
print "text" startswith "te"
print "text" endswith "xt"
print "text" matches regex @"\w+"

// Replacement
print replace_string("Hello World", "World", "KQL")
print replace_regex("123-456", @"\d+", "X")

// Extraction
print extract(@"(\d+)", 1, "abc123def")
print extract_all(@"(\d+)", "a1b2c3")
```

## DateTime Functions

```kql
// Current time
print now()
print now(-1d)  // Yesterday
print ago(1h)   // 1 hour ago

// Date creation
print datetime(2023-01-01)
print datetime(2023-01-01 12:30:45)
print make_datetime(2023, 1, 1, 12, 30, 45)

// Date parts
print year = datetime_part("year", now())
print month = datetime_part("month", now())
print day = datetime_part("day", now())
print hour = datetime_part("hour", now())
print dayofweek = dayofweek(now())
print dayofyear = dayofyear(now())

// Date arithmetic
print datetime_add("day", 7, now())
print datetime_add("hour", -3, now())
print datetime_diff("day", datetime(2023-01-01), now())

// Date rounding
print startofday(now())
print endofday(now())
print startofweek(now())
print startofmonth(now())
print startofyear(now())

// Date formatting
print format_datetime(now(), "yyyy-MM-dd HH:mm:ss")
print format_datetime(now(), "yy-MMM-dd")
```

## Dynamic/JSON Operations

```kql
// Creating dynamic
let obj = dynamic({"name": "Alice", "age": 30});
let arr = dynamic([1, 2, 3, 4, 5]);

// Accessing properties
print obj.name
print obj["age"]
print arr[0]

// Array functions
print array_length(arr)
print array_sum(arr)
print array_concat(arr, dynamic([6, 7]))
print array_slice(arr, 1, 3)
print array_index_of(arr, 3)
print array_reverse(arr)
print array_sort_asc(arr)

// Bag/Object functions
print bag_keys(obj)
print bag_merge(obj, dynamic({"city": "NYC"}))
print bag_remove_keys(obj, dynamic(["age"]))

// Type checking
print gettype(obj)
print isnull(obj.missing)
print isnotnull(obj.name)
```

## Advanced Patterns

### Time Series Analysis
```kql
TableName
| make-series Value = sum(Metric) on Timestamp step 1h by Category
| extend (anomalies, score, baseline) = series_decompose_anomalies(Value)
| extend forecast = series_decompose_forecast(Value, 24)
| extend trend = series_fit_line(Value)
| extend seasonal = series_seasonal(Value)
```

### Geospatial Analysis
```kql
TableName
| extend Point = geo_point_to_h3cell(Longitude, Latitude, 8)
| extend Distance = geo_distance_2points(Lon1, Lat1, Lon2, Lat2)
| where geo_point_in_polygon(Longitude, Latitude, PolygonGeoJson)
| extend S2Cell = geo_point_to_s2cell(Longitude, Latitude, 12)
```

### Machine Learning
```kql
// Clustering
TableName
| evaluate autocluster()

// Basket analysis
TableName
| evaluate basket()

// Outlier detection
TableName
| make-series Value = avg(Metric) on Timestamp step 1h
| extend outliers = series_outliers(Value, 3)
```

### Window Functions
```kql
TableName
| sort by Timestamp asc
| extend PrevValue = prev(Value)
| extend NextValue = next(Value)
| extend RowNum = row_number()
| extend RunningSum = row_cumsum(Value)
| extend Rank = row_rank()
```

## Security Analytics Queries

### Failed Logon Detection
```kql
SecurityEvent
| where EventID == 4625
| summarize FailedCount = count() by Account, Computer, bin(TimeGenerated, 5m)
| where FailedCount > 5
```

### Lateral Movement Detection
```kql
SecurityEvent
| where EventID in (4624, 4648, 4672)
| where AccountType == "User"
| summarize 
    UniqueComputers = dcount(Computer),
    ComputerList = make_set(Computer)
    by Account, bin(TimeGenerated, 1h)
| where UniqueComputers > 3
```

### Data Exfiltration Detection
```kql
CommonSecurityLog
| where DeviceAction == "Allowed"
| summarize 
    TotalBytes = sum(SentBytes),
    Sessions = count()
    by SourceIP, DestinationIP, bin(TimeGenerated, 1h)
| where TotalBytes > 1000000000
```

### Privilege Escalation Detection
```kql
SecurityEvent
| where EventID == 4672
| where PrivilegeList has_any ("SeDebugPrivilege", "SeTakeOwnershipPrivilege", "SeLoadDriverPrivilege")
| project TimeGenerated, Account, Computer, PrivilegeList
```

### Anomalous Process Creation
```kql
SecurityEvent
| where EventID == 4688
| where ProcessName endswith @"\cmd.exe" or ProcessName endswith @"\powershell.exe"
| where ParentProcessName endswith @"\services.exe" or ParentProcessName endswith @"\svchost.exe"
| project TimeGenerated, Computer, Account, ProcessName, ParentProcessName, CommandLine
```

## Performance Monitoring

### CPU Usage Analysis
```kql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize 
    AvgCPU = avg(CounterValue),
    MaxCPU = max(CounterValue),
    P95CPU = percentile(CounterValue, 95)
    by Computer, bin(TimeGenerated, 5m)
| where P95CPU > 80
```

### Memory Usage Tracking
```kql
Perf
| where ObjectName == "Memory" and CounterName == "Available MBytes"
| extend AvailableGB = CounterValue / 1024
| summarize MinAvailable = min(AvailableGB) by Computer, bin(TimeGenerated, 5m)
| where MinAvailable < 2
```

### Disk I/O Analysis
```kql
Perf
| where ObjectName == "LogicalDisk" and CounterName == "Disk Transfers/sec"
| where InstanceName != "_Total"
| summarize AvgIOPS = avg(CounterValue) by Computer, InstanceName, bin(TimeGenerated, 5m)
| where AvgIOPS > 1000
```

## Log Analysis

### Error Log Analysis
```kql
AppLogs
| where Level == "Error"
| parse Message with * "Exception: " ExceptionType " Message: " ErrorMessage
| summarize ErrorCount = count() by ExceptionType, bin(TimeGenerated, 1h)
| render columnchart
```

### Request Duration Analysis
```kql
RequestLogs
| extend Duration = EndTime - StartTime
| summarize 
    AvgDuration = avg(Duration),
    P50 = percentile(Duration, 50),
    P90 = percentile(Duration, 90),
    P99 = percentile(Duration, 99)
    by RequestType, bin(TimeGenerated, 5m)
```

### User Activity Tracking
```kql
ActivityLogs
| summarize 
    UniqueUsers = dcount(UserId),
    TotalActions = count(),
    Actions = make_set(ActionType)
    by bin(TimeGenerated, 1h)
| extend ActionsPerUser = TotalActions * 1.0 / UniqueUsers
```

## Cost Optimization

### Resource Usage by Tag
```kql
Usage
| where IsBillable == true
| extend Tags = parse_json(Tags)
| extend Environment = Tags.Environment, Project = Tags.Project
| summarize TotalGB = sum(Quantity) / 1024 by Environment, Project, DataType
| order by TotalGB desc
```

### Unused Resources Detection
```kql
Heartbeat
| summarize LastSeen = max(TimeGenerated) by Computer
| where LastSeen < ago(7d)
| join kind=leftouter (
    Usage
    | where TimeGenerated > ago(30d)
    | summarize MonthlyGB = sum(Quantity) / 1024 by Computer
) on Computer
```

## Best Practices

### Query Optimization
```kql
// Good: Filter early
Table | where TimeGenerated > ago(1d) | where Status == "Error"

// Better: Combine filters
Table | where TimeGenerated > ago(1d) and Status == "Error"

// Use materialized views for frequently accessed aggregations
materialized_view("DailyStats")

// Use shuffle for large joins
Table1 | join kind=inner hint.strategy=shuffle (Table2) on Key

// Limit data scanned
Table | where TimeGenerated between (starttime .. endtime)
```

### Common Patterns
```kql
// Deduplication
Table | summarize arg_max(TimeGenerated, *) by UniqueId

// Running totals
Table | sort by TimeGenerated asc | extend RunningTotal = row_cumsum(Value)

// Percentage calculation
Table | summarize Count = count() by Category
| extend Percentage = round(100.0 * Count / toscalar(Table | count), 2)

// Conditional aggregation
Table | summarize 
    SuccessCount = countif(Status == "Success"),
    FailureCount = countif(Status == "Failure"),
    SuccessRate = round(100.0 * countif(Status == "Success") / count(), 2)
```

## Platform-Specific Features

### Microsoft Sentinel
```kql
// Threat Intelligence matching
ThreatIntelligenceIndicator
| where TimeGenerated > ago(7d)
| join kind=inner (
    CommonSecurityLog
    | where TimeGenerated > ago(1d)
) on $left.NetworkIP == $right.DestinationIP

// UEBA enrichment
BehaviorAnalytics
| where ActivityType == "FailedLogon"
| where UsersInsights.BlastRadius == "High"
```

### Azure Monitor
```kql
// Application Insights
requests
| where success == false
| summarize FailureRate = count() * 100.0 / toscalar(requests | count()) by operation_Name

// Container Insights
ContainerInventory
| summarize Containers = dcount(ContainerID) by Image, ContainerState
| where ContainerState == "Failed"
```

### Microsoft Fabric
```kql
// Real-time analytics
.create-or-alter function with (folder = "Analytics") 
CalculateMetrics(T:(Timestamp:datetime, Value:real)) {
    T
    | summarize avg(Value), percentile(Value, 95) by bin(Timestamp, 5m)
}
```