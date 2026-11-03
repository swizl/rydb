# rydb

用「日月」语言（`.心` 源码编译为原生 Windows 可执行文件）实现的嵌入式关系数据库，CLI 与 SQL 语法对齐 sqlite3 常用子集。

文件格式为自有格式，**不承诺与 sqlite3 文件二进制兼容**。

## 规划中的里程碑

| 里程碑 | 内容 | 验收 |
| --- | --- | --- |
| C1 存储层 | 4 KiB 分页、页缓存、空闲页链表、文件头与元数据页；崩溃安全提交（journal 或双写） | 写入 10 万行后重开校验；进程中断后可恢复 |
| C2 B+ 树 | 表 B+ 树（rowid 主键）、记录编解码（NULL/整数/浮点/文本）、游标 | 随机插入/删除/查找 10 万行，与内存模型逐条比对 |
| C3 SQL 前端 | 词法/语法（CREATE TABLE、INSERT、SELECT、UPDATE、DELETE、WHERE、ORDER BY、LIMIT、聚合、JOIN）；执行器 | 50 条 SQL 用例与 sqlite3 输出一致 |
| C4 索引与事务 | CREATE INDEX、索引扫描、BEGIN/COMMIT/ROLLBACK、唯一约束、EXPLAIN | 索引命中用例；回滚后数据与事务前一致 |
| C5 CLI 与稳定性 | `rydb 数据库文件` 交互式 shell、`.tables`/`.schema`/`.dump`；随机 SQL 模糊测试 | 1 万条随机语句无崩溃；100 MB 库性能达标 |

## 当前状态

**尚未开工。** 依赖日月的原始内存读写内置函数（已就绪）。
