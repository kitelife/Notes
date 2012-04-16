Web安全
=========

HTTP Secure
-------------

**Hypertext Transfer Protocol Secure (HTTPS)** is a combination of Hypertext Transfer Protocol (HTTP) with SSL/TLS protocol. It provides encrypted communication and secure identification of a network web server. HTTPS connections are often used for payment transactions on the World Wide Web and for sensitive transaction in corporate information systems.

**HTTPS** should not be confused with the little-used Secure HTTP (S-HTTP).

HTTPS is a URI scheme which has identical syntax to the standard HTTP scheme, aside from its scheme token. However, HTTPS signals the browser to use an added encryption layer of SSL/TLS to protect the traffic. SSL is especially suited for HTTP since it can provide some protection even if only one side of the communication is authenticated. This is the case with HTTP transactions over the Internet, where typically only the server is authenticated.

The main idea of HTTPS is to create a secure channel over an insecure network. This ensures reasonable protection from eavesdroppers(窃听) and man-in-the-middle attacks, provided that adequate cipher suites(密码套件) are used and that the server certificate is verified and trusted.

Difference from HTTP
^^^^^^^^^^^^^^^^^^^^^^

HTTPS URLs begin with "https://" and use port 443 by default, where HTTP URLs begin with "http://" and use port 80 by default.

Network layers
^^^^^^^^^^^^^^^^

HTTP operates at the highest layer of the OSI Model, the Application layer; but the security protocol operates at a lower sublayer, encrypting an HTTP message prior to transmission and decrypting a message upon arrival. Strictly speaking, HTTPS is not a separate protocol, but refers to use the ordinary HTTP over an encrypted SSL/TLS connection.

Everything in the HTTPS message is encrypted, including the headers, and the request/reponse load. 

Server setup
^^^^^^^^^^^^^^^^

To prepare a web server to accept HTTPS connections, the administrator must create a public key certificate for the web server. This certificate must be signed by a trusted certificate authority for the web browser to accept it without warning.

Limitations
^^^^^^^^^^^^^

SSL comes in two options, simple and mutual.

The mutual version is more secure, but requires the user to install a personal certificate in their browser in order to authenticate themselves.

Whatever strategy is used (simple or mutual), the level of protection strongly depends on the correctness of the implementation of the web browser and the server software and the actual cryptographic algorithms supported.

Because SSL operates below HTTP and has no knowledge of higher-level protocols, SSL servers can only strictly present one certificate for a particular IP/port combination. This means that, in most cases, it is not feasible to use name-based virtual hosting with HTTPS. A solution called Server Name Indication (SNI) exists, which sends the hostname to the server before encrypting the connection.

Transport Layer Security
--------------------------

**Transport Layer Security (TLS)** and its predecessor, **Secure Sockets Layer (SSL)** , are cryptographic protocols(加密协议) that provide communication security over the Internet. TLS and SSL encrypt the segments of network connections above the Transport Layer, using asymmetric cryptography(非对称密码) for key exchange, symmetric encryption for privacy, and message authentication codes for message integrity.

SSL/TLS协议的优势在于它是与应用层协议独立无关的。应用层协议能透明地建立于SSL/TLS协议之上。SSL/TLS协议在应用层协议通信之前就已经完成加密算法，通信密钥的协商以及服务器认证工作。在此之后应用层协议所传送的数据都会被加密，从而保证通信的私密性。

TLS is an IETF standards track protocol, last updated in RFC 5246, and is based on the earlier SSL specifications developed by Netscape Communications.



Since most protocols can be used either with or without TLS (or SSL) it is necessary to indicate to the server whether the client is making a TLS connection or not. There are two main ways of achieving this, one option is to use a different port number for TLS connections. The other is to use the regular port number and have the client request that the server switch the connection to TLS using a protocol specific mechanism (for example STARTTLS for mail and news protocols).

Once the client and server have decided to use TLS they negotiate a stateful connection by using a handshaking procedure. During this handshake, the client and server agree on various parameters used to establish the connection's security.

- The handshake begins when a client connects to a TLS-enabled server requesting a secure connection and presents a list of supported cipher suites (ciphers and hash functions).
- From this list, the server picks the strongest cipher and hash function that it also supports and notifies the client of the decision.
- The server sends back its identification in the form of a digital certificate. The certificate usually contains the server name, the trusted certificate authority (CA) and the server's public encryption key.

- The client may contact the server that issued the certificate (the trusted CA as above) and confirm the validity of the certificate before proceeding.
- In order to generate the session keys used for the secure connection, the client encrypts a random number with the server's public key and sends the result to the server. Only the server should be able to decrypt it, with its private key.
- From the random number, both parties generate key material for encryption and decryption.

Applications
^^^^^^^^^^^^^^

In application design, TLS is usually implemented on top of any of the Transport Layer protocols, encapsulating(封装) the application-specific protocols such as HTTP, FTP, SMTP, NNTP and XMPP. Historically it has been used primarily with reliable transport protocols such as the Transmission Control Protocol(TCP). However, it has also been implemented with datagram-oriented transport protocols, such as the User Datagram Protocol(UDP) and the Datagram Congestion Control Protocol(DCCP), usage which has been standardized independently using the term Datagram Transport Layer Security(DTLS).

Robots exclusion standard
---------------------------

The **Robot Exclusion Standard** , also known as the **Robots Exclusion Protocol** or **robots.txt protocol** , is a convention to prevent cooperating web crawlers and other web robots from accessing all or part of a website which is otherwise publicly viewable. Robots are often used by search engines to categorize and archive web sites, or by webmasters to proofread source code. The standard is different from, but can be used in conjunction with, Sitemaps, a robot inclusion standard for websites.

About the standard
^^^^^^^^^^^^^^^^^^^^

If a site owner wishes to give instructions to web robots they must place a text file called robots.txt in the root of the web site hierarchy. This text file should contain the instructions in a specific format. Robots that choose to follow the instructions try to fetch this file and read the instructions before fetching any other file from the web site. If this file doesn't exist web robots assume that the web owner wishes to provide no specific instructions.

A robots.txt file on a website will function as a request that specified robots ignore specified files or directories when crawling a site.

For websites with multiple subdomains, each subdomain must have its own robots.txt file. If example.com had a robots.txt file but a.example.com did not, the rules that would apply for example.com would not apply to a.example.com

Disadvantages
^^^^^^^^^^^^^^^

Despite the use of the terms "allow" and "disallow", the protocol is purely advisory. It relies on the cooperation of the web robot, so that marking an area of a site bounds with robots.txt does not guarantee exclusion of all web robots.

There is no official standards body or RFC for the robots.txt protocol.

Examples
^^^^^^^^^^

This example tells all robots to visit all files because the wildcard * specifies all robots:
::

    User-agent:*
    Disallow:

This example tells all robots to stay out of a website:
::

    User-agent:*
    Disallow:/

The next is an example that tells all robots not to enter four directories of a website:
::

    User-agent:*
    Disallow:/cgi-bin/
    Disallow:/images/
    Disallow:/tmp/
    Disallow:/private/

Nonstandard extensions
^^^^^^^^^^^^^^^^^^^^^^^^

**Crawl-delay directive**

Several major crawlers support a Crawl-delay parameter, set to the number of seconds to wait between successive(连续的) requests to the same server:
::

    User-agent:*
    Crawl-delay:10

**Allow directive**

**Sitemap**

Some crawlers support a Sitemap directive, allowing multiple Sitemaps in the same robots.txt in the form:
::

    Sitemap: http://www.gstatic.com/s2/sitemaps/profiles-sitemap.xml
    Sitemap: http://www.google.com/hostednews/sitemap_index.xml


