---
title: PITR 之使用实践和性能调优
summary: 了解 PITR 的使用方式和性能调优参数。
---

# PITR 的使用实践和限制

## 在 TiDB 集群中使用 PITR

PITR（Point-in-time recovery）作为数据库的容灾功能，可以支持数据库恢复到任意时刻点的数据状态。TiDB 集群中 PITR 的使用基于全量快照备份/恢复和日志备份/恢复功能。

|  | 上游(备份)集群 | 下游（恢复）集群 |
|---------|---------|-------------|
| 快照（全量）阶段 | 快照备份 | 快照恢复 |
| 日志（增量）阶段 | 备份日志 | 恢复日志 |
| 使用规则 | 备份阶段（2 个命令）- 开启日志备份任务，使用 br log start 命令，默认从执行时间点开始备份数据变更日志；- 创建全量快照备份，使用 br backup full 命令；默认备份命令执行时间点的快照数据。| 恢复阶段（1 个命令）：- 恢复全量快照数据 - 恢复日志数据 直接使用 br restore point，执行时需带上 --restored-ts 表示恢复到的目标时间点； |

ClusterA 为上游集群，备份操作仅在上游集群操作：
- 先在 ClusterA 上创建日志备份任务，这样 ClusterA 上的数据变更日志就会源源不断的备份到 Log1 中；
- 在 ClusterA 上创建全量快照。
ClusterB 为下游集群，恢复操作仅在下游集群操作：
- 使用 br restore point 命令一键将下游集群恢复到指定时间点的数据状态，执行命令时需要指定全量快照路径，备份日志路径和恢复的时间点。
如果需要将经过 PITR 恢复的集群 ClusterB 作为新的备份上游，可以等到 ClusterB PITR 恢复完成后再创建日志备份任务和全量快照，这些数据可以用来恢复到其他集群。 

## 

# PITR 之性能调优