# Redeemer

|            |                                                                          |
|------------|--------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                  |
| Difficulty | Very Easy                                                                |
| OS         | Linux                                                                    |
| Tags       | Redis, Databases, Vulnerability Assessment, Reconnaissance, Anonymous Access |
| Tools      | `nmap`, `redis-cli` (from `redis-tools`)                                 |

## Summary

An unauthenticated Redis 5.0.7 instance. Connect with `redis-cli`, find the
`flag` key in db0, read it.

## Notes

- Handy reference: [Redis Cheat Sheet](https://lzone.de/cheat-sheet/Redis)
- `redis-cli` ships in the `redis-tools` package on Debian/Ubuntu.

## Recon

Full TCP scan:

```bash
nmap -sT -p- --min-rate=3000 10.129.136.187
```

```
PORT     STATE SERVICE
6379/tcp open  redis
```

## Enumeration

Pull server info to learn the version:

```bash
redis-cli -h 10.129.136.187 INFO | grep version
```

```
redis_version:5.0.7
gcc_version:9.3.0
```

## Foothold

No auth required. Drop into an interactive session, select db0, list keys:

```bash
redis-cli -h 10.129.136.187
10.129.136.187:6379> info keyspace
# Keyspace
db0:keys=4,expires=0,avg_ttl=0
10.129.136.187:6379> select 0
OK
10.129.136.187:6379> keys *
1) "flag"
2) "temp"
3) "stor"
4) "numb"
10.129.136.187:6379> get flag
"03e1d2b376c37ab3f5319922053953eb"
```

## Flag

```
03e1d2b376c37ab3f5319922053953eb
```

## Guided Tasks

| #  | Question | Answer |
|----|----------|--------|
| 1  | Which TCP port is open on the machine? | 6379 |
| 2  | Which service is running on the port that is open on the machine? | redis |
| 3  | What type of database is Redis? | In-memory Database |
| 4  | Which command-line utility is used to interact with the Redis server? | redis-cli |
| 5  | Which flag is used with the Redis command-line utility to specify the hostname? | -h |
| 6  | Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server? | info |
| 7  | What is the version of the Redis server being used on the target machine? | 5.0.7 |
| 8  | Which command is used to select the desired database in Redis? | select |
| 9  | How many keys are present inside the database with index 0? | 4 |
| 10 | Which command is used to obtain all the keys in a database? | `keys *` |
| Flag | Submit Root Flag | `03e1d2b376c37ab3f5319922053953eb` |
