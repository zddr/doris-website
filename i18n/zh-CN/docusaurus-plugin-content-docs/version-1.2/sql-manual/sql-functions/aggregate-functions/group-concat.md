---
{
    "title": "GROUP_CONCAT",
    "language": "zh-CN"
}
---

## group_concat
## 描述
## 语法

`VARCHAR GROUP_CONCAT([DISTINCT] VARCHAR str[, VARCHAR sep] [ORDER BY { col_name | expr} [ASC | DESC])`


该函数是类似于 sum() 的聚合函数，group_concat 将结果集中的多行结果连接成一个字符串。第二个参数 sep 为字符串之间的连接符号，该参数可以省略。该函数通常需要和 group by 语句一起使用。

支持Order By进行多行结果的排序，排序和聚合列可不同。

:::caution
`group_concat`暂不支持`distinct`和`order by`一起用。
:::

## 举例

```
mysql> select value from test;
+-------+
| value |
+-------+
| a     |
| b     |
| c     |
| c     |
+-------+

mysql> select GROUP_CONCAT(value) from test;
+-----------------------+
| GROUP_CONCAT(`value`) |
+-----------------------+
| a, b, c, c               |
+-----------------------+

mysql> select GROUP_CONCAT(DISTINCT value) from test;
+-----------------------+
| GROUP_CONCAT(`value`) |
+-----------------------+
| a, b, c               |
+-----------------------+

mysql> select GROUP_CONCAT(value, " ") from test;
+----------------------------+
| GROUP_CONCAT(`value`, ' ') |
+----------------------------+
| a b c c                    |
+----------------------------+

mysql> select GROUP_CONCAT(value, NULL) from test;
+----------------------------+
| GROUP_CONCAT(`value`, NULL)|
+----------------------------+
| NULL                       |
+----------------------------+

SELECT abs(k3), group_concat(distinct cast(abs(k2) as varchar) order by abs(k1), k5) FROM bigtable group by abs(k3) order by abs(k3);     +------------+-------------------------------------------------------------------------------+
| abs(`k3`)  | group_concat(DISTINCT CAST(abs(`k2`) AS CHARACTER), ORDER BY abs(`k1`), `k5`) |
+------------+-------------------------------------------------------------------------------+
|        103 | 255                                                                           |
|       1001 | 1989, 1986                                                                    |
|       1002 | 1989, 32767                                                                   |
|       3021 | 1991, 32767, 1992                                                             |
|       5014 | 1985, 1991                                                                    |
|      25699 | 1989                                                                          |
| 2147483647 | 255, 1991, 32767, 32767                                                       |
+------------+-------------------------------------------------------------------------------+
```

### keywords
GROUP_CONCAT,GROUP,CONCAT
