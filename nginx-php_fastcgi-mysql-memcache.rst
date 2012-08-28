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

FastCGI
^^^^^^^^^^^^^

FastCGI is a protocol for interfacing interactive programs with a web server. FastCGI is a variation on the earlier Common Gateway Interface(CGI); FastCGI's main aim is to reduce the overhead associated with interfacing the web server and CGI programs, allowing a server to handle more web page requests at once.

CGI is a protocol for interfacing external applications to web servers. CGI applications run in separate processes, which are created at the start of each request and torn down at the end. This "one new process per request" model makes CGI programs very simple to implement, but limits efficiency and scalability. At the high loads, the operating system process creation and destruction overhead becomes significant. In addition, the CGI process model limits resource reuse techniques (such as reusing database connection, in-memory caching, etc.).

Instead of creating a new process for each request, FastCGI uses persistent process to handle a series of requests. This processes are owned by the FastCGI server, not the web server.

To service an incoming request, the web server sends environment information and the page request itself to a FastCGI process over a socket(in the case of local FastCGI processes on the web server) or TCP connection (for remote FastCGI processes in a server farm). Responses are returned from the process to the web server over the same connection, and the web server subsequently delivers that response to the end-user. The connection may be closed at the end of a response, but both the web server and the FastCGI service processes persist.

Each individual FastCGI process can handle many requests over its lifetime, thereby avoiding the overhead of per-request process creation and termination. Processing of multiple requests simultaneously can be achieved in several ways: by using a single connection with internal multiplexing (i.e. multiple requests over a single connection); by using multiple connections; or by combination of these techniques. Multiple FastCGI servers can be configured, increasing stability and scalability.

Web site administrators and programmers can find that the separation of web applications from the web server in FastCGI has many advantages over embedded interpreters (mod_perl, mod_php, etc.). This separation allows server and application processes to be restarted independently – an important consideration for busy web sites. It also enables the implementation of per-application / hosting service security policies, which is an important requirement for ISPs and web hosting companies. Different types of incoming requests can be distributed to specific FastCGI servers which have been equipped to handle those particular types of requests efficiently.

Common Gateway Interface(CGI)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Common Gateway Interface (CGI) is a standard method for web server software to delegate the generation of web content to executable files. Such files are known as CGI scripts; they are programs, often stand-alone applications, usually written in a scripting language.

A web server that supports CGI can be configured to interpret a URL that it serves as a reference to a CGI script. A common convention is to have a cgi-bin/ directory at the base of the directory tree and treat all executable files within it as CGI scripts.

In the case of HTTP PUT OR POSTS, the user-submitted data is provided to the program via the standard input. The web server creates a small and efficient subset of the environment variables passed to it and adds details pertinent to the execution of the program.

The program returns the result to the web server in the form of standard output, beginning with a header and a blank line. The header is encoded in the same way as an HTTP header and must include the MIME type of the document returned. The headers, supplemented by the web server, are generally forwarded with the response back to the user.

**Drawbacks**

Calling a command generally means the invocation of a newly created process on the server. Starting the process can consume much more time and memory than the actual work of generating the output, especially when the program still needs to be interpreted or compiled. If the command is called often, the resulting workload can quickly overwhelm the web server.

The overhead involved in interpretation may be reduced by using compiled CGI programs, such as those in C/C++, rather than using Perl or other scripting languages. The overhead involved in process creation can be reduced by solutions such as FastCGI, or by running the application code entirely within the web server using extension modules such as mod_php.

