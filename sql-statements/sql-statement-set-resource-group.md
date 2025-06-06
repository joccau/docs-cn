---
title: SET RESOURCE GROUP
summary: TiDB 数据库中 SET RESOURCE GROUP 的使用概况。
---

# SET RESOURCE GROUP

`SET RESOURCE GROUP` 用于设置当前会话使用的资源组。

## 语法图

**SetResourceGroupStmt:**

```ebnf+diagram
SetResourceGroupStmt ::=
    "SET" "RESOURCE" "GROUP" ResourceGroupName

ResourceGroupName ::=
    Identifier
|   "DEFAULT"
```

## 权限

要执行此命令，需满足以下配置和权限：

1. [`tidb_enable_resource_control`](/system-variables.md#tidb_enable_resource_control-从-v660-版本开始引入) 设置为 `ON`。
2. 当系统变量 [`tidb_resource_control_strict_mode`](/system-variables.md#tidb_resource_control_strict_mode-从-v820-版本开始引入) 设置为 `ON`时，你需要有 `SUPER` 或者 `RESOURCE_GROUP_ADMIN` 或者 `RESOURCE_GROUP_USER` 权限；当设置为 `OFF` 时，则无需上述权限。

## 示例

创建一个用户 `user1`，创建两个资源组 `rg1` 和 `rg2`，并将用户 `user1` 绑定资源组 `rg1`。

```sql
CREATE USER 'user1';
CREATE RESOURCE GROUP 'rg1' RU_PER_SEC = 1000;
ALTER USER 'user1' RESOURCE GROUP `rg1`;
```

使用 `user1` 登录，查看当前用户绑定的资源组。

```sql
SELECT CURRENT_RESOURCE_GROUP();
```

```sql
+--------------------------+
| CURRENT_RESOURCE_GROUP() |
+--------------------------+
| rg1                      |
+--------------------------+
1 row in set (0.00 sec)
```

执行 `SET RESOURCE GROUP` 将当前会话的资源组设置为 `rg2`。

```sql
SET RESOURCE GROUP `rg2`;
SELECT CURRENT_RESOURCE_GROUP();
```

```sql
+--------------------------+
| CURRENT_RESOURCE_GROUP() |
+--------------------------+
| rg2                      |
+--------------------------+
1 row in set (0.00 sec)
```

执行 `SET RESOURCE GROUP` 设置当前会话使用默认资源组。

```sql
SET RESOURCE GROUP `default`;
SELECT CURRENT_RESOURCE_GROUP();
```

```sql
+--------------------------+
| CURRENT_RESOURCE_GROUP() |
+--------------------------+
| default                  |
+--------------------------+
1 row in set (0.00 sec)
```

## MySQL 兼容性

MySQL 也支持 [SET RESOURCE GROUP](https://dev.mysql.com/doc/refman/8.0/en/set-resource-group.html)，但是接受的参数和 TiDB 不同，两者并不兼容。

## 另请参阅

* [CREATE RESOURCE GROUP](/sql-statements/sql-statement-create-resource-group.md)
* [DROP RESOURCE GROUP](/sql-statements/sql-statement-drop-resource-group.md)
* [ALTER RESOURCE GROUP](/sql-statements/sql-statement-alter-resource-group.md)
* [使用资源管控 (Resource Control) 实现资源组限制和流控](/tidb-resource-control-ru-groups.md)
