事务ACID性质

Redis 事务保证了其中的一致性（C）和隔离性（I），但并不保证原子性（A）和持久性（D）。

##### 原子性（Atomicity）

单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

如果一个事务队列中的所有命令都被成功地执行，那么称这个事务执行成功。

另一方面，如果 Redis 服务器进程在执行事务的过程中被停止 —— 比如接到 KILL 信号、宿主机器停机，等等，那么事务执行失败。

当事务失败时，Redis 也不会进行任何的重试或者回滚动作。

##### 一致性（Consistency）

Redis 的一致性问题可以分为三部分来讨论：入队错误、执行错误、Redis 进程被终结。

#### 入队错误

在命令入队的过程中，如果客户端向服务器发送了错误的命令，比如命令的参数数量不对，等等， 那么服务器将向客户端返回一个出错信息， 并且将客户端的事务状态设为`REDIS_DIRTY_EXEC`。

当客户端执行[EXEC](http://redis.readthedocs.org/en/latest/transaction/exec.html#exec)命令时， Redis 会拒绝执行状态为`REDIS_DIRTY_EXEC`的事务， 并返回失败信息。

```
redis 127.0.0.1:6379> MULTI
OK

redis 127.0.0.1:6379> set key
(error) ERR wrong number of arguments for 'set' command

redis 127.0.0.1:6379> EXISTS key
QUEUED

redis 127.0.0.1:6379> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
```

因此，带有不正确入队命令的事务不会被执行，也不会影响数据库的一致性。

