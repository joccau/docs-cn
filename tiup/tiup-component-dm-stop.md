---
title: tiup dm stop
summary: tiup dm stop 命令用于停止指定集群的所有服务或部分服务。核心服务停止后集群将无法提供服务。语法为 tiup dm stop <cluster-name> [flags]。选项包括 -N, --node（strings，默认为 []，表示所有节点），-R, --role（strings，默认为 []，表示所有角色），-h, --help。停止服务的日志将会输出。
---

# tiup dm stop

命令 `tiup dm stop` 用于停止指定集群的所有服务或部分服务。

> **注意：**
>
> 核心服务停止后集群将无法提供服务。

## 语法

```shell
tiup dm stop <cluster-name> [flags]
```

`<cluster-name>` 为要操作的集群名字，如果忘记集群名字可查看[集群列表](/tiup/tiup-component-dm-list.md)。

## 选项

### -N, --node（strings，默认为 []，表示所有节点）

指定要停止的节点，不指定则表示所有节点。该选项的值为以逗号分割的节点 ID 列表，节点 ID 为[集群状态](/tiup/tiup-component-dm-display.md)表格的第一列。

> **注意：**
> 
> 若同时指定了 `-R, --role`，那么将停止它们的交集中的服务。

### -R, --role（strings，默认为 []，表示所有角色）

指定要停止的角色，不指定则表示所有角色。该选项的值为以逗号分割的节点角色列表，角色为[集群状态](/tiup/tiup-component-dm-display.md)表格的第二列。

> **注意：**
> 
> 若同时指定了 `-N, --node`，那么将停止它们的交集中的服务。

### -h, --help

- 输出帮助信息。
- 数据类型：`BOOLEAN`
- 该选项默认关闭，默认值为 `false`。在命令中添加该选项，并传入 `true` 值或不传值，均可开启此功能。

## 输出

停止服务的日志。
