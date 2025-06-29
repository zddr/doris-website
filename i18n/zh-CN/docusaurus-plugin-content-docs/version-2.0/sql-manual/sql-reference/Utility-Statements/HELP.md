---
{
    "title": "HELP",
    "language": "zh-CN"
}
---

## HELP

### Name

HELP

## 描述

通过改命令可以查询到帮助的目录

语法：

``` sql
HELP <item>
```

可以通过  `help`  列出所有的 Doris 命令

```sql
List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.

For server side help, type 'help contents'
```

通过 `help contents` 获取 Doris SQL 帮助目录

```sql
Many help items for your request exist.
To make a more specific request, please type 'help <item>',
where <item> is one of the following
categories:
   sql-functions
   sql-statements
```

## 举例

1. 列出 Doris 所有的 SQL 帮助目录

   ```sql
   help contents
   ```

2. 列出 Doris 集群所有函数目录的命令

   ```sql
   help sql-functions
   ```

3. 列出日期函数下的所有函数列表

   ```sql
   help date-time-functions
   ```

### Keywords

    HELP

### Best Practice

