---
{
    "title": "MONEY_FORMAT",
    "language": "zh-CN"
}
---

## money_format
## 描述
## 语法

`VARCHAR money_format(Number)`


将数字按照货币格式输出，整数部分每隔3位用逗号分隔，小数部分保留2位

## 举例

```
mysql> select money_format(17014116);
+------------------------+
| money_format(17014116) |
+------------------------+
| 17,014,116.00          |
+------------------------+

mysql> select money_format(1123.456);
+------------------------+
| money_format(1123.456) |
+------------------------+
| 1,123.46               |
+------------------------+

mysql> select money_format(1123.4);
+----------------------+
| money_format(1123.4) |
+----------------------+
| 1,123.40             |
+----------------------+
```
### keywords
    MONEY_FORMAT,MONEY,FORMAT
