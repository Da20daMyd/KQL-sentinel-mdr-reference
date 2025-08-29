# KQL String Functions

## strcat() function

**Syntax:**
```kusto
strcat(argument1, argument2 [, argument3 ...])
```

**Parameters:**
- argument1...argumentN: Expressions to concatenate (1-64 arguments)

**Description:**
Concatenates between 1 and 64 arguments. Non-string arguments are converted to string.

**Examples:**
```kusto
print str = strcat("hello", " ", "world")
```

```kusto
print MultiLineString = strcat("Line 1\n", "Line 2\n", "Line 3")
```

```kusto
SecurityEvent
| extend FullCommand = strcat(Process, " ", CommandLine)
| project Computer, FullCommand
```

```kusto
SigninLogs
| extend UserLocation = strcat(UserPrincipalName, " from ", Location)
```

## substring() function

**Syntax:**
```kusto
substring(source, startingIndex [, length])
```

**Parameters:**
- source: Source string
- startingIndex: Zero-based starting position (negative = from end)
- length: Number of characters (optional, default: to end of string)

**Examples:**
```kusto
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
substring("123456", -2, 2)    // 56
```

```kusto
SecurityEvent
| extend ProcessName = substring(Process, 0, 10)
| project Computer, ProcessName
```

```kusto
Event
| extend EventIDShort = substring(tostring(EventID), 0, 3)
```

## tolower() function

**Syntax:**
```kusto
tolower(value)
```

**Parameters:**
- value: String value to convert to lowercase

**Examples:**
```kusto
tolower("Hello") == "hello"
```

```kusto
SecurityEvent
| extend ProcessLower = tolower(Process)
| where ProcessLower contains "cmd"
```

```kusto
SigninLogs
| extend UserLower = tolower(UserPrincipalName)
| summarize count() by UserLower
```

## toupper() function

**Syntax:**
```kusto
toupper(value)
```

**Parameters:**
- value: String value to convert to uppercase

**Examples:**
```kusto
toupper("hello") == "HELLO"
```

```kusto
SecurityEvent
| extend ProcessUpper = toupper(Process)
| project Computer, ProcessUpper
```

```kusto
Event
| extend SourceUpper = toupper(Source)
| where SourceUpper startswith "MICROSOFT"
```