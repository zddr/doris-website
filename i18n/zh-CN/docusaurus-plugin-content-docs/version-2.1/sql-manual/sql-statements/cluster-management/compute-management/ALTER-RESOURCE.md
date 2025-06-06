---
{
"title": "ALTER RESOURCE",
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

该语句用于修改一个已有的资源。仅 root 或 admin 用户可以修改资源。

## 语法
```sql
ALTER RESOURCE '<resource_name>'
PROPERTIES (
  `<property>`, 
  [ , ... ]
);
```

## 参数

1.`<property>`

`<property>` 格式为 `<key>` = `<value>`，不支持修改`<key>`等于 type 的`<value>`。

其他可修改的 properties 参数参考 [CREATE-RESOURCE](./CREATE-RESOURCE.md) 章节;

## 示例

1. 修改名为 spark0 的 Spark 资源的工作目录：
```sql
ALTER RESOURCE 'spark0' PROPERTIES ("working_dir" = "hdfs://127.0.0.1:10000/tmp/doris_new");
```
2. 修改名为 remote_s3 的 S3 资源的最大连接数：
```sql
ALTER RESOURCE 'remote_s3' PROPERTIES ("s3.connection.maximum" = "100");
```
3. 修改冷热分层 S3 资源相关信息
- 支持修改项
  - `s3.access_key` s3 的 ak 信息
  - `s3.secret_key` s3 的 sk 信息
  - `s3.session_token` s3 的 session token 信息
  - `s3.connection.maximum` s3 最大连接数，默认 50
  - `s3.connection.timeout` s3 连接超时时间，默认 1000ms
  - `s3.connection.request.timeout` s3 请求超时时间，默认 3000ms
- 禁止修改项
  - `s3.region`
  - `s3.bucket"`
  - `s3.root.path`
  - `s3.endpoint`

```sql
  ALTER RESOURCE "showPolicy_1_resource" PROPERTIES("s3.connection.maximum" = "1111");
```