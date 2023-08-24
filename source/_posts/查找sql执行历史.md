---
title: 通过一个事故学习查找sql的执行历史
date: 2023-08-24-14:00:00
tags: Dev
---

## 代码

```sql
SELECT   spid,
         blocked,
         DB_NAME(sp.dbid) AS DBName,
         program_name,
         waitresource,
         sp.waittime,
         sp.stmt_start,
         lastwaittype,
         sp.loginame,
         sp.Status,
         sp.hostname,
         a.[Text] AS [TextData],
         SUBSTRING(A.text, sp.stmt_start / 2,
         (CASE WHEN sp.stmt_end = -1 THEN DATALENGTH(A.text) ELSE sp.stmt_end
         END - sp.stmt_start) / 2) AS [current_cmd]
FROM     sys.sysprocesses AS sp OUTER APPLY sys.dm_exec_sql_text (sp.sql_handle) AS A
WHERE    spid > 50 and lastwaittype='HADR_SYNC_COMMIT'            
ORDER BY blocked DESC, DB_NAME(sp.dbid) ASC, a.[text];
```
忘记从哪个老哥那里抄来的了，在此感谢老哥！

## 故事梗概

23号快下班的时候，收到客户的信息，说是在执行了一个500行左右插入的sql后，服务器卡死了。

上手先看日志，没发现什么异常。

对执行插入的表SELECT TOP (100)，也没有异常，正常查询出来结果。

没办法只能先重启服务试一下。然而，没什么效果（；´д｀）ゞ

把组里之前做这个项目的哥请过来看，打开任务管理器，CPU占有率100%映入眼帘，一看都是SSMS占有的，直接把数据库脱机，服务暂停，再看日志，这个时候日志打印出来了，报错连接数为0。

暂时还没想到什么问题，联机数据库，重启服务，恢复正常了。查询直接插入的数据，一条也查不到，怀疑发生了回滚。和组里的哥怀疑是插入动作和其他用户的查询操作发生了碰撞，计划晚上再看。

晚上，我先查询数据库的执行历史，在客户提供截图上的提交时间，根本没查询到任何的sql提交。再把客户的sql拿过来执行，本来以为会无事发生的，没想到复现问题，同时按照时间倒序查询该表也无法查询，真的是sql的锅！

一开始怀疑是数据类型或者主键的问题，于是我自己写了一个sql，把客户的数据拿过来，执行成功，毫无问题！

不是数据的问题，有点迷茫，打开SQL profile,再执行一遍，监视动态，也看不出来任何问题XD

一筹莫展的时候，突然想起来锁，一看客户的sql，begin trans后没有commit！！！

**原因发现**：问题是插入的sql里打开了一个事务，但是结尾的地方没有commit，导致事务一直处于未完成的状态，不会修改实际数据，相关的数据行也会加锁，导致所有相关的读操作都会等待，且数据库链接一断开就直接回滚，所以在历史sql执行记录里找不到执行历史，也无法实际成功插入。

这就是为什么能查TOP(1000),但无法倒序查，也无法查相关数据的原因，因为排他锁！

**事件教训**：良好的代码习惯很重要！数据操作要谨慎，不然漏一行代码，可以干爆服务器。大学的数据库课程发挥了作用，数据库的书还是要留着拿出来翻翻。