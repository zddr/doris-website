---
{
    "title": "CANCEL MATERIALIZED VIEW TASK",
    "language": "zh-CN"
}
---

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->


## 描述

该语句用于取消物化视图的 task


语法：

```sql
CANCEL MATERIALIZED VIEW TASK taskId=INTEGER_VALUE ON mvName=multipartIdentifier
```

## 示例

1. 取消物化视图 mv1 的 id 为 1 的 task


    ```sql
    CANCEL MATERIALIZED VIEW TASK 1 on mv1;
    ```
   
## 关键词

    CANCEL, MATERIALIZED, VIEW, TASK

## 最佳实践
