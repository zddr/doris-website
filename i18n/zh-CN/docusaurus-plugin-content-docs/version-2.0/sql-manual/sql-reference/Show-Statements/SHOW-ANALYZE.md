---
{
    "title": "SHOW-ANALYZE",
    "language": "zh-CN"
}
---

## SHOW-ANALYZE

### Name

SHOW ANALYZE

## 描述

通过 `SHOW ANALYZE` 来查看统计信息收集作业的信息。

语法如下：

```SQL
SHOW [AUTO] ANALYZE < table_name | job_id >
    [ WHERE [ STATE = [ "PENDING" | "RUNNING" | "FINISHED" | "FAILED" ] ] ];
```

- AUTO：仅仅展示自动收集历史作业信息。需要注意的是默认只保存过去 20000 个执行完毕的自动收集作业的状态。
- table_name：表名，指定后可查看该表对应的统计作业信息。可以是  `db_name.table_name`  形式。不指定时返回所有统计作业信息。
- job_id：统计信息作业 ID，执行 `ANALYZE` 异步收集时得到。不指定 id 时此命令返回所有统计作业信息。

输出：

| 列名                   | 说明         |
| :--------------------- | :----------- |
| `job_id`               | 统计作业 ID  |
| `catalog_name`         | catalog 名称 |
| `db_name`              | 数据库名称   |
| `tbl_name`             | 表名称       |
| `col_name`             | 列名称列表       |
| `job_type`             | 作业类型     |
| `analysis_type`        | 统计类型     |
| `message`              | 作业信息     |
| `last_exec_time_in_ms` | 上次执行时间 |
| `state`                | 作业状态     |
| `schedule_type`        | 调度方式     |

下面是一个例子：

```sql
mysql> show analyze 245073\G;
*************************** 1. row ***************************
              job_id: 245073
        catalog_name: internal
             db_name: default_cluster:tpch
            tbl_name: lineitem
            col_name: [l_returnflag,l_receiptdate,l_tax,l_shipmode,l_suppkey,l_shipdate,l_commitdate,l_partkey,l_orderkey,l_quantity,l_linestatus,l_comment,l_extendedprice,l_linenumber,l_discount,l_shipinstruct]
            job_type: MANUAL
       analysis_type: FUNDAMENTALS
             message: 
last_exec_time_in_ms: 2023-11-07 11:00:52
               state: FINISHED
            progress: 16 Finished  |  0 Failed  |  0 In Progress  |  16 Total
       schedule_type: ONCE
```

<br/>

每个收集作业中可以包含一到多个任务，每个任务对应一列的收集。用户可通过如下命令查看具体每列的统计信息收集完成情况。

语法：

```sql
SHOW ANALYZE TASK STATUS [job_id]
```

下面是一个例子：

```
mysql> show analyze task status 20038 ;
+---------+----------+---------+----------------------+----------+
| task_id | col_name | message | last_exec_time_in_ms | state    |
+---------+----------+---------+----------------------+----------+
| 20039   | col4     |         | 2023-06-01 17:22:15  | FINISHED |
| 20040   | col2     |         | 2023-06-01 17:22:15  | FINISHED |
| 20041   | col3     |         | 2023-06-01 17:22:15  | FINISHED |
| 20042   | col1     |         | 2023-06-01 17:22:15  | FINISHED |
+---------+----------+---------+----------------------+----------+


```

### Keywords

SHOW, ANALYZE
