# KQL Data Types

## Scalar Data Types

| Type | Aliases | Description | Example |
|------|---------|-------------|---------|
| `bool` | `boolean` | Boolean true/false | `true`, `false`, `1`, `0` |
| `datetime` | `date` | Instant in time | `datetime(2023-01-01 12:00:00)` |
| `decimal` | | 128-bit decimal | `decimal(123.456)` |
| `dynamic` | | Array, bag, or any scalar | `dynamic([1,2,3])`, `dynamic({"key":"value"})` |
| `guid` | `uuid`, `uniqueid` | 128-bit unique ID | `guid(74be27de-1e4e-49d9-b579-fe0b331d3642)` |
| `int` | | 32-bit integer | `42` |
| `long` | | 64-bit integer | `9223372036854775807` |
| `real` | `double` | 64-bit floating-point | `3.14159` |
| `string` | | Unicode text | `"Hello, World!"` |
| `timespan` | `time` | Time interval | `1d`, `2h`, `30m`, `45s` |

## Type Conversion Functions

```kql
// String to other types
print tolong("123")
print todouble("3.14")
print todatetime("2023-01-01")
print totimespan("1.02:03:04.5")
print tobool("true")
print toguid("74be27de-1e4e-49d9-b579-fe0b331d3642")

// Dynamic conversions
print todynamic('{"name":"John","age":30}')
print todynamic('[1,2,3,4,5]')

// Type checking
print gettype(123)           // int
print gettype(123.456)       // real
print gettype("text")        // string
print gettype(now())         // datetime
```

## Working with Dynamic Data

```kql
// Accessing dynamic properties
let data = dynamic({"user":"alice", "score":95, "tags":["admin","power-user"]});
print 
    user = data.user,
    score = data.score,
    firstTag = data.tags[0]

// Dynamic array operations
let numbers = dynamic([1,2,3,4,5]);
print 
    length = array_length(numbers),
    sum = array_sum(numbers),
    reversed = array_reverse(numbers)

// Bag operations
datatable(id:int, properties:dynamic)
[
    1, dynamic({"name":"Server1", "cpu":80}),
    2, dynamic({"name":"Server2", "cpu":65})
]
| extend name = properties.name, cpu = properties.cpu
```

## Null Handling

```kql
// Check for null values
print isnull(null)           // true
print isnotnull("value")     // true
print isempty("")            // true
print isnotempty("text")     // true

// Coalesce null values
print coalesce(null, null, "default", "other")  // "default"

// Handle nulls in aggregations
datatable(value:int)
[
    1, 2, null, 4, null, 6
]
| summarize 
    count_all = count(),           // 6 (includes nulls)
    count_values = countif(isnotnull(value)),  // 4
    sum_values = sum(value),        // 13 (nulls ignored)
    avg_values = avg(value)         // 3.25 (13/4)
```

## Timespan Literals

```kql
// Timespan suffixes
print 
    days = 2d,
    hours = 3h,
    minutes = 45m,
    seconds = 30s,
    milliseconds = 500ms,
    microseconds = 1000us,
    ticks = 10000000tick

// Timespan arithmetic
print 
    total = 1d + 2h + 30m,
    difference = datetime(2023-01-02) - datetime(2023-01-01),
    scaled = 2 * 1h
```

## GUID Operations

```kql
// Create and manipulate GUIDs
print 
    new_guid = new_guid(),
    parsed = guid("12345678-1234-1234-1234-123456789012"),
    as_string = tostring(guid(74be27de-1e4e-49d9-b579-fe0b331d3642))

// Filter by GUID
datatable(id:guid, name:string)
[
    guid(74be27de-1e4e-49d9-b579-fe0b331d3642), "Item1",
    guid(c49d8b3a-4b5e-4c7a-9f8a-2e3456789abc), "Item2"
]
| where id == guid(74be27de-1e4e-49d9-b579-fe0b331d3642)
```

## Type-Safe Comparisons

```kql
// Numeric comparisons
datatable(value:real)
[1.0, 2.5, 3.7, 4.2]
| where value between (2.0 .. 4.0)

// String comparisons (case-insensitive)
datatable(text:string)
["Hello", "HELLO", "hello", "World"]
| where text =~ "hello"

// DateTime comparisons
datatable(timestamp:datetime)
[datetime(2023-01-01), datetime(2023-06-15), datetime(2023-12-31)]
| where timestamp >= datetime(2023-06-01)

// Dynamic comparisons
datatable(data:dynamic)
[dynamic({"score":85}), dynamic({"score":92}), dynamic({"score":78})]
| where data.score > 80
```