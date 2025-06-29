---
{
    "title": "SHOW BACKEND CONFIG",
    "language": "en"
}
---

## Description

Display the configuration items and their current values for BACKEND (i.e., BE).

## Syntax

```sql
[ ADMIN ] SHOW BACKEND CONFIG [ LIKE <config_key_pattern> ] [ FROM <backend_id> ]
```

## Optional Parameters

**<config_key_pattern>**

> Provides a wildcard pattern to match BE configuration items. The matching rules are the same as those for LIKE expressions. For writing rules, please refer to the "Matching Expressions" section.

**<backend_id>**

> The ID of the BE. Used to view the configuration of the BE with the specified ID. The ID of the BE can be obtained through the SHOW BACKENDS command. Please refer to the "SHOW BACKENDS" command for details.

## Return Value

- BackendId: The ID of the BE
- Host: The host address of the BE
- Key: The name of the configuration item
- Value: The corresponding value of the configuration item
- Type: The type of the configuration value

## Access Control Requirements

Users executing this SQL command must have at least ADMIN_PRIV permissions.

## Examples

### Query All Configuration Items

```sql
SHOW BACKEND CONFIG
```
Result:

```sql
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
| BackendId | Host         | Key                            | Value | Type                     | IsMutable |
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
| 12793     | 172.16.123.1 | LZ4_HC_compression_level       | 9     | int64_t                  | true      |
| 12793     | 172.16.123.1 | agent_task_trace_threshold_sec | 2     | int32_t                  | true      |
...
| 12794     | 172.16.123.2 | zone_map_row_num_threshold     | 20    | int32_t                  | true      |
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
```

### Query Configuration Items for a Specified BE ID

```sql
SHOW BACKEND CONFIG FROM 12793
```

Result:

```sql
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
| BackendId | Host         | Key                            | Value | Type                     | IsMutable |
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
| 12793     | 172.16.123.1 | LZ4_HC_compression_level       | 9     | int64_t                  | true      |
| 12793     | 172.16.123.1 | agent_task_trace_threshold_sec | 2     | int32_t                  | true      |
...
| 12793     | 172.16.123.1 | zone_map_row_num_threshold     | 20    | int32_t                  | true      |
+-----------+--------------+--------------------------------+-------+--------------------------+-----------+
```

### Query Configuration Items Matching a Specified Pattern

```sql
SHOW BACKEND CONFIG LIKE '%compression_level%'
```

Result:

```sql
+-----------+--------------+--------------------------+-------+---------+-----------+
| BackendId | Host         | Key                      | Value | Type    | IsMutable |
+-----------+--------------+--------------------------+-------+---------+-----------+
| 12793     | 172.16.123.1 | LZ4_HC_compression_level | 9     | int64_t | true      |
| 12794     | 172.16.123.2 | LZ4_HC_compression_level | 9     | int64_t | true      |
+-----------+--------------+--------------------------+-------+---------+-----------+
```