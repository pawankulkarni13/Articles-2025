# Redis

## What is Redis ?
Redis is an open-source, in-memory data structure store that is often used as a cache, message broker, and database. 
It supports various data structures such as:

- Strings
- Lists
- Sets
- Sorted sets
- Hashes
- Streams
- Bitmaps
- HyperLogLogs
- Geospatial indexes

### Key Features of Redis
1. In-Memory Storage: Data is stored in RAM, making reads and writes extremely fast.
2. Key-Value Store: It operates as a NoSQL key-value store, allowing efficient data retrieval.
3. Persistence Options: Supports RDB (Redis Database Backup) and AOF (Append-Only File) persistence for durability.
4. Replication: Supports primary-replica (master-slave) replication for high availability.
5. Pub/Sub Messaging: Redis can be used as a message broker via its publish/subscribe (Pub/Sub) feature.
6. Automatic Partitioning (Sharding): It allows horizontal scaling by distributing data across multiple Redis nodes.
7. Transactions & Scripting: Supports transactions using MULTI/EXEC and scripting using Lua.
8. High Availability & Clustering: Redis Sentinel and Redis Cluster enable fault tolerance and load balancing.
9. Atomic Operations: Redis operations on data structures are atomic, ensuring consistency.
10. Extensive Client Support: Redis supports multiple programming languages like Java, Python, Node.js, Go, C#, and more.

### Common Use Cases
- Caching: Speeding up applications by caching database queries, API responses, or session data.
- Session Management: Storing user sessions in a scalable and fast manner.
- Message Queues: Using Redis Lists or Streams for real-time messaging.
- Leaderboard & Ranking: Using Sorted Sets for ranking in gaming and social media applications.
- Rate Limiting: Preventing excessive API calls using the INCR command.

### Redis vs Other Databases

|Feature	|Redis	| Traditional RDBMS (MySQL, PostgreSQL)	 | NoSQL (MongoDB)               |
|----|----|----------------------------------------|-------------------------------|
|Storage	|In-memory	| Disk-based	                            | Disk-based                    |
|Speed	|Extremely Fast	| Slower	| Fast|
|Data Model	|Key-Value	| Relational (Tables)	                   | Document-based                |
|Use Case	|Caching, Queues	| Transactions, Complex Queries	         | Flexible Schema, JSON Storage |

## How does Redis Store values ?
Redis stores key-value pairs in-memory using an efficient hash table (dictionary) with linked lists, skip lists, and other data structures optimized for high-speed access. 
The keys are unique strings, and the values can be one of several data structures.

Redis is an in-memory data store, meaning it primarily keeps data in RAM for ultra-fast read and write operations. 
However, to prevent data loss, Redis provides two main persistence mechanisms:
- RDB (Snapshotting): Saves data at intervals.
- AOF (Append-Only File): Logs every command for durability.

It also supports a hybrid approach that combines RDB and AOF for reliability and performance.

#### What is RDB?
RDB (Redis Database) creates snapshots of the dataset at scheduled intervals.
The snapshot is stored in a binary file (dump.rdb).
It allows fast startup time since the dataset is loaded in bulk.
##### How RDB Works?
1. At specific intervals, Redis forks a child process.
2. The child process writes the current dataset to a .rdb file.
3. Once the dump is completed, Redis replaces the old .rdb file with the new one.
4. If Redis crashes, the latest .rdb file is used to restore the data.

##### RDB Configuration in redis.conf
You can configure automatic snapshot saving based on key modifications.
save 900 1    # Save every 900 seconds (15 min) if at least 1 key changed

To trigger a manual RDB snapshot:

SAVE  # Blocks Redis until the snapshot is complete
BGSAVE  # Runs in the background using a forked process

##### Pros of RDB

- Efficient for large datasets (snapshot compression reduces size).
- Faster restart times (bulk loading is quicker than AOF replay).
- Lower performance overhead (I/O operations happen in a background process).

##### Cons of RDB
- Data loss risk (only saves at intervals, so the latest changes may be lost if Redis crashes).
- Heavy disk writes (especially for large datasets).

#### What is AOF?
AOF logs every write operation (SET, HSET, SADD, etc.) to a file (appendonly.aof).
Instead of saving snapshots, Redis replays commands on startup to rebuild the dataset.
Offers higher durability but comes with higher disk I/O costs.

##### How AOF Works?
1. Every Redis write command is logged sequentially to the appendonly.aof file.
2. If Redis crashes, it replays the commands from AOF to restore the dataset.
3. To avoid excessive file growth, Redis compacts the log (AOF rewrite) periodically.

AOF Configuration in redis.conf
Enable AOF:

```
appendonly yes  # Enables AOF persistence
appendfilename "appendonly.aof"  # AOF file name
```

Configure how often AOF writes to disk:

```
appendfsync always  # Writes to disk after every operation (slow, safest)
appendfsync everysec  # Writes once per second (default, best trade-off)
appendfsync no  # Lets the OS handle writing (fast, but risks data loss)
```

To manually trigger AOF rewriting (compaction):
```
BGREWRITEAOF  # Runs in background, rewriting a smaller AOF file
```

##### Pros of AOF
- No data loss risk (logs every write).
- Easier to recover from crashes (replays logs).
- More readable (commands stored as plain text).

##### Cons of AOF
- Larger file size compared to RDB.
- Slower startup time (since commands must be replayed).
- Higher disk I/O (frequent writes).


RDB vs AOF - When to Use What?

|Feature	| RDB (Snapshot)	                                               | AOF (Append-Only)        |
|----|---------------------------------------------------------------|--------------------------|
|Durability	| Periodic snapshots (may lose last changes)	| Almost no data loss|
|Startup Time	| Faster (bulk loading)	| Slower (replays logs)    |
|Performance	| Lower I/O impact	| Higher I/O impact        |
|Disk Usage	| Compressed binary file (small)	| Large due to logs        |
|Use Case	| Best for backups and fast recovery	| Best for high durability |

Recommended Hybrid Approach
Most production systems use both RDB + AOF for the best balance:|
```
save 300 10  # RDB snapshot every 5 minutes if at least 10 keys changed
appendonly yes  # Enable AOF for durability
appendfsync everysec  # AOF syncs every second (performance trade-off)
```
This setup ensures fast recovery with minimal data loss.

#### Advanced Persistence Features
AOF Rewrite Optimization
AOF logs can grow large over time. To optimize, Redis rewrites the AOF file:
- Old redundant commands are removed (e.g., multiple SET commands for the same key).
- The rewritten file contains only the latest necessary commands.
  How to trigger manually:
```
BGREWRITEAOF
```
Redis does this automatically if:

```
auto-aof-rewrite-percentage 100  # Rewrite when AOF grows 100% larger
auto-aof-rewrite-min-size 64mb  # Minimum size before rewrite starts
```

## Crash Recovery & Data Consistency
What Happens If Redis Crashes?
- With RDB only → You lose the data since the last snapshot.
- With AOF only → You recover all operations logged before the crash.
- With RDB + AOF → Redis prioritizes AOF to restore the most recent state.

What Happens If AOF Gets Corrupted?
Redis provides a built-in AOF repair tool:(This will remove corrupted entries and keep valid ones.)
```
redis-check-aof --fix appendonly.aof
```
## Best Practices for Redis Persistence
- Use RDB for backups, AOF for durability.
- Adjust snapshot intervals (save configuration) to balance performance.
- Enable AOF rewrites to prevent excessive file growth.
- Use appendfsync everysec for good performance & safety.
- Monitor disk usage to prevent unexpected storage issues.

## What is TTL for Key ?

## How is Expiry Handled ?

## Alternate Designs to Handle Expiry ? Can we have a custom design ?
