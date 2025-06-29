---
{
    "title": "SHOW-LOAD-PROFILE",
    "language": "zh-CN"
}
---

## SHOW-LOAD-PROFILE

### Name

SHOW LOAD PROFILE

## 描述

该语句是用来查看导入操作的 Profile 信息，该功能需要用户打开 Profile 设置，0.15 之前版本执行下面的设置：

```sql
SET is_report_success=true;
```

0.15 及之后的版本执行下面的设置：

```sql
SET [GLOBAL] enable_profile=true;
```

语法：

```sql
show load profile "/";

show load profile "/[queryId]"

show load profile "/[queryId]/[TaskId]"

show load profile "/[queryId]/[TaskId]/[FragmentId]/"

show load profile "/[queryId]/[TaskId]/[FragmentId]/[InstanceId]"
```

这个命令会列出当前保存的所有导入 Profile。每行对应一个导入。其中 QueryId 列为导入作业的 ID。这个 ID 也可以通过 SHOW LOAD 语句查看拿到。我们可以选择我们想看的 Profile 对应的 QueryId，查看具体情况

## 举例

1. 列出所有的 Load Profile

```sql
mysql> show load profile "/"\G
*************************** 1. row ***************************
               JobId: 20010
            QueryId: 980014623046410a-af5d36f23381017f
               User: root
            DefaultDb: default_cluster:test
                  SQL: LOAD LABEL xxx
            QueryType: Load
            StartTime: 2023-03-07 19:48:24
            EndTime: 2023-03-07 19:50:45
            TotalTime: 2m21s
         QueryState: N/A
            TraceId:
         AnalysisTime: NULL
            PlanTime: NULL
         ScheduleTime: NULL
      FetchResultTime: NULL
      WriteResultTime: NULL
WaitAndFetchResultTime: NULL
*************************** 2. row ***************************
               JobId: N/A
            QueryId: 7cc2d0282a7a4391-8dd75030185134d8
               User: root
            DefaultDb: default_cluster:test
                  SQL: insert into xxx
            QueryType: Load
            StartTime: 2023-03-07 19:49:15
            EndTime: 2023-03-07 19:49:15
            TotalTime: 102ms
         QueryState: OK
            TraceId:
         AnalysisTime: 825.277us
            PlanTime: 4.126ms
         ScheduleTime: N/A
      FetchResultTime: 0ns
      WriteResultTime: 0ns
WaitAndFetchResultTime: N/A
```

2. 查看有导入作业的子任务概况：

   ```sql
   mysql> show load profile "/980014623046410a-af5d36f23381017f";
   +-----------------------------------+------------+
   | TaskId                            | ActiveTime |
   +-----------------------------------+------------+
   | 980014623046410a-af5d36f23381017f | 3m14s      |
   +-----------------------------------+------------+
   ```

3. 查看子任务的执行树：

   ```sql
   show load profile "/980014623046410a-af5d36f23381017f/980014623046410a-af5d36f23381017f";

                         ┌───────────────────────┐
                         │[-1: OlapTableSink]    │
                         │Fragment: 0            │
                         │MaxActiveTime: 86.541ms│
                         └───────────────────────┘
                                     │
                                     │
                           ┌───────────────────┐
                           │[1: VEXCHANGE_NODE]│
                           │Fragment: 0        │
                           └───────────────────┘
           ┌─────────────────────────┴───────┐
           │                                 │
    ┌─────────────┐              ┌───────────────────────┐
    │[MemoryUsage]│              │[1: VDataStreamSender] │
    │Fragment: 0  │              │Fragment: 1            │
    └─────────────┘              │MaxActiveTime: 34.882ms│
                                 └───────────────────────┘
                                             │
                                             │
                               ┌───────────────────────────┐
                               │[0: VNewOlapScanNode(tbl1)]│
                               │Fragment: 1                │
                               └───────────────────────────┘
                           ┌─────────────────┴───────┐
                           │                         │
                    ┌─────────────┐            ┌───────────┐
                    │[MemoryUsage]│            │[VScanner] │
                    │Fragment: 1  │            │Fragment: 1│
                    └─────────────┘            └───────────┘
                                             ┌───────┴─────────┐
                                             │                 │
                                    ┌─────────────────┐ ┌─────────────┐
                                    │[SegmentIterator]│ │[MemoryUsage]│
                                    │Fragment: 1      │ │Fragment: 1  │
                                    └─────────────────┘ └─────────────┘

   ```

   这一层会显示子任务的查询树，其中标注了 Fragment id。

4. 查看指定 Fragment 的 Instance 概况，同时

   ```sql
   mysql> show load profile "/980014623046410a-af5d36f23381017f/980014623046410a-af5d36f23381017f/1";
   +-----------------------------------+------------------+------------+
   | Instances                         | Host             | ActiveTime |
   +-----------------------------------+------------------+------------+
   | 980014623046410a-88e260f0c43031f2 | 10.81.85.89:9067 | 3m7s       |
   | 980014623046410a-88e260f0c43031f3 | 10.81.85.89:9067 | 3m6s       |
   | 980014623046410a-88e260f0c43031f4 | 10.81.85.89:9067 | 3m10s      |
   | 980014623046410a-88e260f0c43031f5 | 10.81.85.89:9067 | 3m14s      |
   +-----------------------------------+------------------+------------+
   ```

5. 继续查看某一个具体的 Instance 上各个算子的详细 Profile

   ```sql
   mysql> show load profile "/980014623046410a-af5d36f23381017f/980014623046410a-af5d36f23381017f/1/980014623046410a-88e260f0c43031f5"\G
   
   *************************** 1. row ***************************
   
   Instance:
   
         ┌-----------------------------------------┐
         │[-1: OlapTableSink]                      │
         │(Active: 2m17s, non-child: 70.91)        │
         │  - Counters:                            │
         │      - CloseWaitTime: 1m53s             │
         │      - ConvertBatchTime: 0ns            │
         │      - MaxAddBatchExecTime: 1m46s       │
         │      - NonBlockingSendTime: 3m11s       │
         │      - NumberBatchAdded: 782            │
         │      - NumberNodeChannels: 1            │
         │      - OpenTime: 743.822us              │
         │      - RowsFiltered: 0                  │
         │      - RowsRead: 1.599729M (1599729)    │
         │      - RowsReturned: 1.599729M (1599729)│
         │      - SendDataTime: 11s761ms           │
         │      - TotalAddBatchExecTime: 1m46s     │
         │      - ValidateDataTime: 9s802ms        │
         └-----------------------------------------┘
                              │
   ┌-----------------------------------------------------┐
   │[0: BROKER_SCAN_NODE]                                │
   │(Active: 56s537ms, non-child: 29.06)                 │
   │  - Counters:                                        │
   │      - BytesDecompressed: 0.00                      │
   │      - BytesRead: 5.77 GB                           │
   │      - DecompressTime: 0ns                          │
   │      - FileReadTime: 34s263ms                       │
   │      - MaterializeTupleTime(*): 45s54ms             │
   │      - NumDiskAccess: 0                             │
   │      - PeakMemoryUsage: 33.03 MB                    │
   │      - RowsRead: 1.599729M (1599729)                │
   │      - RowsReturned: 1.599729M (1599729)            │
   │      - RowsReturnedRate: 28.295K sec                │
   │      - TotalRawReadTime(*): 1m20s                   │
   │      - TotalReadThroughput: 30.39858627319336 MB/sec│
   │      - WaitScannerTime: 56s528ms                    │
   └-----------------------------------------------------┘
   ```

### Keywords

    SHOW, LOAD, PROFILE


