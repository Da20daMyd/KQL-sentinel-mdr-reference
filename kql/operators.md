# KQL Operators

## sort operator

**Syntax:**
```kusto
T | sort by column [ asc | desc ] [ nulls first | nulls last ] [ , ...]
```

**Parameters:**
- T: The tabular input to sort
- column: The column to sort by (numeric, date, time, or string)
- asc/desc: Sort order (default: desc)
- nulls first/last: Null value placement

**Examples:**
```kusto
StormEvents
| sort by State asc, StartTime desc
```

```kusto
StormEvents
| sort by StartTime desc
| take 10
```

## take operator

**Syntax:**
```kusto
take NumberOfRows
```

**Parameters:**
- NumberOfRows: Number of rows to return

**Examples:**
```kusto
StormEvents | take 5
```

```kusto
SecurityEvent 
| where TimeGenerated > ago(1h)
| take 100
```

## count operator

**Syntax:**
```kusto
T | count
```

**Examples:**
```kusto
StormEvents | count
```

```kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| count
```

## distinct operator

**Syntax:**
```kusto
T | distinct ColumnName [, ColumnName2, ...]
```

**Parameters:**
- ColumnName: Column name to search for distinct values
- *: All columns (use asterisk for wide tables)

**Examples:**
```kusto
StormEvents
| where InjuriesDirect > 45
| distinct State, EventType
```

```kusto
SecurityEvent
| distinct Computer
```

## parse operator

**Syntax:**
```kusto
T | parse [ kind= kind [ flags= regexFlags ]] expression with [ * ] stringConstant columnName [ : columnType ] [ * ] , ...
```

**Parameters:**
- kind: simple (default), regex, relaxed
- regexFlags: U (ungreedy), m (multi-line), s (match newline), i (case-insensitive)
- expression: String expression to parse
- stringConstant: String constant to search for
- columnName: Column name for extracted value
- columnType: Data type (default: string)

**Examples:**
```kusto
Traces
| parse EventText with * "resourceName=" resourceName ", totalSlices=" totalSlices: long * "sliceNumber=" sliceNumber: long *
```

```kusto
SecurityEvent
| parse CommandLine with * "cmd.exe" * "/c" command *
| project Computer, command
```

**Regex mode example:**
```kusto
Traces
| parse kind=regex EventText with "(.*?)[a-zA-Z]*=" resourceName @", totalSlices=\s*\d+\s*.*?sliceNumber=" sliceNumber: long
```

## evaluate operator

**Syntax:**
```kusto
[ T | ] evaluate [ evaluateParameters ] PluginName ( [ PluginArgs ] )
```

**Parameters:**
- T: Tabular input (optional)
- evaluateParameters: Control parameters (hint.distribution, hint.pass_filters, etc.)
- PluginName: Plugin to invoke
- PluginArgs: Plugin arguments

**Examples:**
```kusto
StormEvents 
| evaluate autocluster()
```

```kusto
T 
| evaluate pivot(PivotColumn, AggregatedColumn=count())
```

```kusto
StormEvents
| where State == "TEXAS"
| evaluate basket()
```