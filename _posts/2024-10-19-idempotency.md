---
layout: post
title: "Idempotency and how to implement it?"
---

### Idempotency
A REST API method is called idempotent when making multiple identical requests to an API has the same effect as making a single request. GET, DELETE, PUT are idempotent by default, making multiple of these requests make no difference than making one succesful
  request. But when implement a POST or PATCH request, we have to be careful, POST requests are usually used to create resources, firing a second POST requests normally results in unexpected behaviors.

### Idempotency key
The only way to implement this is to differentiate one request from another so that server can tell they are different. This is 
called idempotency key. The client attaches a unique to the body request and sends it to server. From the server side, the server 
extracts the idempotency key and checks if this is already processed. It could simply return error or something meaningful.

Because the request is user-supplied, you can't never guarantee the idempotency key is globally unique, combining the idempontency key with user_id and the path url is good enough.

### Idempotency storage
Now there is a need to store the idempotency key somewhere. It is a clear choice to use some NoSQL databae like DynamoDB or an 
in-memory database like Redis. There is a TTL for keys, it could be 10 minutes.

### Flow
Now a request comming in to payment server asking to checkout a heavy basket. The client construct a idempotency by concatenating uuid + user_id + url send it to the server. At server, the server look up the idempontecy key in a Redis instance, if the key is not existed, simply return the coresponding value to to client, else write through the value to cache.

This is some diagram of the above idea.

<img src="https://raw.githubusercontent.com/quynhdinh/quynhdinh.github.io/refs/heads/master/img/idempotency.png" width="600">