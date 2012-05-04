Nginx
=======

The nginx web server is a fast, lightweight server designed to efficiently handle the needs of both low and high traffic websites. Although commonly used to serve static content, it's quite capable of handling dynamic pages as well.

为什么选择Nginx？
^^^^^^^^^^^^^^^^^^

- 在高连接并发的情况下，Nginx是Apache服务器不错的替代品。

- Nginx作为负载均衡服务器：Nginx既可以在内部直接支持Rails和PHP程序对外进行服务，也可以支持作为HTTP代理服务器对外进行服务。Nginx采用C进行编写，不论系统资源开销还是CPU使用效率都比Perlbal要好很多。

- 作为邮件代理服务器：Nginx同时也是一个非常优秀的邮件代理服务器

- Nginx一个安装非常简单，配置文件非常简洁(还能够支持Perl语法)，Bug非常少的服务器：Nginx启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。

Memcached
===========

What is Memcached ？
^^^^^^^^^^^^^^^^^^^^^^

Free & open source, high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating(减轻，缓和) database load.

Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of database calls, API calls, or page rendering.

Memcached is simple yet powerful. Its simple design promotes quick deployment, ease of development, and solves many problems facing large data caches. Its API is available for most popular languages.

You can think of it as a short-term memory for your application.

What is Does ?
^^^^^^^^^^^^^^^^^^

memcached allows you to take memory from parts of your system where you have more than you need and make it accessible to areas where you have less than you need.

memcached also allows you to make better use of your memory.

.. image:: http://memcached.org/memcached-usage.png

Memcached (introduced by wikipedia)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In computing, memcached is general-purpose distributed memory caching system that was originally developed by Danga Interactie for LiveJournal, but is now used by many other sites. It is often used to speed up dynamic database-driven websites by caching data and objects in RAM to reduce the number of times an external data source (such as a database or API) must be read.

Memcached's APIs provide a giant hash table distributed across multiple machines. When the table is full, subsequent inserts cause older data to be purged in least recently used (LRU) order. Applications using Memcached typically layer requests and additions into RAM before falling back on a slower backing store, such as a database.

**Architecture**

The system uses a client-server architecture. The servers maintain a key-value associative array; the clients populate this array and query it. Keys are up to 250 bytes long and values can be most 1 megabyte in size.

Client use client side libraries to contact the servers which, by default, expose their service at port 11211. Each client knows all servers; the servers do not communicate with each other. If a client wishes to set or read the value corresponding to a certain key, the client's library first computes a hash of the key to determine the server that will be used. Then it contacts the server. The server will compute a second of hash of the key to determine where to store or read the corresponding value.

The servers keep the values in RAM; if a server runs out of RAM, it discards the oldest values. Therefore, clients must treat Memcached as a transitory (暂时的) cache; they cannot assume that data stored in Memcached is still there when they need it. A Memcached-protocol compatible product known as MemcacheDB provides persistent storage.

If all client libraries use the same hasing algorithm to determine servers, then clients can read each other's cached data, this is obviously desirable.
