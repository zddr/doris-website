---
{
"title": "IS_IPV4_COMPAT",
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

## IS_IPV4_COMPAT

IS_IPV4_COMPAT

## 描述

## 语法

`VARCHAR IS_IPV4_COMPAT(INET6_ATON(VARCHAR ipv4_addr))`

该函数采用以数字形式表示的二进制字符串形式的 IPv6 地址，由 INET6_ATON() 返回。
如果参数是有效的 IPv4 兼容 IPv6 地址，则返回 1，否则返回 0（除非 expr 为 NULL，在这种情况下该函数返回 NULL）。
IPv4 兼容地址的格式为::ipv4_address。

## 举例

```sql
mysql> SELECT IS_IPV4_COMPAT(INET6_ATON('::ffff:10.0.5.9')) AS is_result;
+-----------+
| is_result |
+-----------+
|         0 |
+-----------+
1 row in set (0.02 sec)

mysql> SELECT IS_IPV4_COMPAT(INET6_ATON('::10.0.5.9')) AS is_result;
+-----------+
| is_result |
+-----------+
|         1 |
+-----------+
1 row in set (0.03 sec)
```

### Keywords

IS_IPV4_COMPAT, IP