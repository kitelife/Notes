Web安全
=========

HTTP Secure
-------------

**Hypertext Transfer Protocol Secure (HTTPS)** is a combination of Hypertext Transfer Protocol (HTTP) with SSL/TLS protocol. It provides encrypted communication and secure identification of a network web server. HTTPS connections are often used for payment transactions on the World Wide Web and for sensitive transaction in corporate information systems.

**HTTPS** should not be confused with the little-used Secure HTTP (S-HTTP).

HTTPS is a URI scheme which has identical syntax to the standard HTTP scheme, aside from its scheme token. However, HTTPS signals the browser to use an added encryption layer of SSL/TLS to protect the traffic. SSL is especially suited for HTTP since it can provide some protection even if only one side of the communication is authenticated. This is the case with HTTP transactions over the Internet, where typically only the server is authenticated.

The main idea of HTTPS is to create a secure channel over an insecure network. This ensures reasonable protection from eavesdroppers(窃听) and man-in-the-middle attacks, provided that adequate cipher suites(密码套件) are used and that the server certificate is verified and trusted.

**Difference from HTTP**

HTTPS URLs begin with "https://" and use port 443 by default, where HTTP URLs begin with "http://" and use port 80 by default.

**Network layers**

HTTP operates at the highest layer of the OSI Model, the Application layer; but the security protocol operates at a lower sublayer, encrypting an HTTP message prior to transmission and decrypting a message upon arrival. Strictly speaking, HTTPS is not a separate protocol, but refers to use the ordinary HTTP over an encrypted SSL/TLS connection.

Everything in the HTTPS message is encrypted, including the headers, and the request/reponse load. 

**Server setup**

To prepare a web server to accept HTTPS connections, the administrator must create a public key certificate for the web server. This certificate must be signed by a trusted certificate authority for the web browser to accept it without warning.

**Limitations**

SSL comes in two options, simple and mutual.

The mutual version is more secure, but requires the user to install a personal certificate in their browser in order to authenticate themselves.

Whatever strategy is used (simple or mutual), the level of protection strongly depends on the correctness of the implementation of the web browser and the server software and the actual cryptographic algorithms supported.

Because SSL operates below HTTP and has no knowledge of higher-level protocols, SSL servers can only strictly present one certificate for a particular IP/port combination. This means that, in most cases, it is not feasible to use name-based virtual hosting with HTTPS. A solution called Server Name Indication (SNI) exists, which sends the hostname to the server before encrypting the connection.


