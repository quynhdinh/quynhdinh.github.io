---
layout: post
title: "[WIP] Draft: SQL Iceberg"
---
This is the writing up of my learnings from this amazing [video](https://www.youtube.com/watch?v=JZRWkfXNQOk). In short, this is 
the video that the engineer of CockroachDB explaining terms in SQL from most widely known trivia to the deep, highly technical internals.
#### ascending key problem
[Timestamp](https://youtu.be/JZRWkfXNQOk?t=5345): Given a table `create table d(timestamp primary key, b string)`. In a write heavy application, 
you are constantly writing, reading to/from one end of the table. So your application does not effectively distribute requests
to that load, now you having the ascending key problem. One solution to this is using the hash key primary key `create table d(a timestamp primary key key using hash, b string)` 
The database will distribute your primary key into fixed(16) number of buckets.

#### sargability



