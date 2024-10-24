---
layout: post
title: "[WIP] SQL Iceberg"
---
This is the writing up of my learnings from this amazing [video](https://www.youtube.com/watch?v=JZRWkfXNQOk). In short, this is 
the video that an engineer of CockroachDB explaining terms in SQL from most widely known trivia to the deep, highly technical internals.
#### ascending key problem
[Timestamp](https://youtu.be/JZRWkfXNQOk?t=5345): Given a table `create table d(a timestamp primary key, b string)`. In a write heavy application, 
you are constantly writing, reading to/from one end of the table. So your application does not effectively distribute requests
to that load, now you having the ascending key problem. One solution to this is using the hash key primary key `create table d(a timestamp primary key using hash, b string)` 
The database will distribute your primary key into fixed(16) number of buckets.

#### sargability

#### MCVCC garbage collection
Some database implement isolations using MVCC. MVCC stands for multi version concurrency control. It means when you insert a piece of data. It inserts the timestamp also. 
And when you update a piece of data off your database. You are not wiping out the data off the DB but you write the new data next to the given key

The purpose of this is the DB needs this information to do isolation levels.

When you are constantly inserting new information into the database which means your database grow indefinitely. The DB has to do MVCC GC which means deleting old versions of keys.

#### Write skew
[What does write skew look like](https://justinjaffray.com/what-does-write-skew-look-like/)
[Strong consistency model](https://aphyr.com/posts/313-strong-consistency-models)

### [Zigzag join](https://youtu.be/JZRWkfXNQOk?t=3854)
At high levels, there are two types of joins. Physical joins, and logical joins. Logical joins are like inner, full, outer joins. There are two other cool ones like anti join and semi join. 
Anti join is give me everything in the left not the right that matched. Semi join is giving everything in the left that do join the things on the right. Physical join are how the DB actually 
select all of the data in the DB that matches the logical join. There are some of these(nested loop join, hash join, merge join, [zigzag join](https://www.youtube.com/watch?v=ofhEyDBpngM))
