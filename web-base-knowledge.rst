Web基础知识
============

Hypertext Transfer Protocol
----------------------------

List of HTTP status codes
---------------------------

1xx Informational
^^^^^^^^^^^^^^^^^^^

Request received, continuing process.

This class of status code indicates a provisional(暂时的) reponse, consisting only of the Status-Line and optional headers, and is terminated by an empty line. Since HTTP/1.0 did not define any 1xx status codes, servers must not send a 1xx response to an HTTP/1.0 client except under experimental conditions.

**100 Continue**

**101 Switching Protocols**

**102 Processing**

2xx Sucess
^^^^^^^^^^^^

This class of status codes indicates the action requested by the client was received, understood, accepted and processed successfully.

**200 OK** : Standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request the response will contain an entity describing or containing the result of the action.

**201 Created** : The request has been fulfilled and resulted in a new resource being created.

**202 Accepted** : The request has been accepted for processing, but the processing has not been completed. The request might or might not eventually be acted upon, as it might be disallowed when processing actually takes place.

**203 Non-Authoritative Information** : The server successfully processed the request, but is returning information that may be from another source.

**204 No Content** : The server successfully processed the request, but is not returning any content.

**205 Reset Content** : The server successfully processed the request, but is not returning any content. Unlike a 204 response, this response requires that the requester reset the document view.

3xx Redirection
^^^^^^^^^^^^^^^^

The client must take additional action to complete the request.

This class of status code indicates that further action needs to be taken by the user agent in order to fulfill the request.

**300 Multiple Choices** : Indicates multiple options for the resource that the client may follow. It, for instance, could be used to present different options for video, list files with different extensions, or word sense disambiguation.

**301 Moved Permanently** : This and all future requests should be directed to the given URI.

**302 Found**

**303 See Other (Since HTTP/1.1)** : The response to the request can be found under another URI using a GET method. When received in response to a POST (or PUT/DELETE), it should be assumed that the server has received the data and the redirect should be issued with a separate GET message.

**304 Not Modified**

**305 Use Proxy (since HTTP/1.1)**

**306 Switch Proxy** : No longer used.

**307 Temporary Redirect (since HTTP/1.1)**

4xx Client Error
^^^^^^^^^^^^^^^^^^

The 4xx class of status code is intended for cases in which the client seems to have erred. Except when responding to a HEAD request, the server should include an entity containing an explanation of the error situation, and whether it is a temporary of permanent condition.

**400 Bad Request** : The request cannot be fulfilled due to bad syntax

**401 Unauthorized** : Similar to *403 Forbidden* , but specifically for use when authentication is possible but has failed or not yet been provided.

**403 Forbidden** : The request was a legal request, but the server is refusing to respond to it. Unlike a *401 Unauthorized* response, authenticating will make no difference.

**404 Not Found** : The requested resource could not be found but may be available again in the future.

**405 Method Not Allowed**

**406 Not Acceptable** : The requested resource is only capable of generating content not acceptable according to the Accept headers sent in the request.

**407 Proxy Authentication Required**

**408 Request Timeout**

5xx Server Error
^^^^^^^^^^^^^^^^^^

The server failed to fulfill an apparently valid request.

Response status codes beginning with the digit "5" indicate cases in which the server is aware that it has encountered an error or is otherwise incapable of performing the request. Except when responding to a HEAD request, the server should include an entity containing an explanation of the error situation, and indicate whether it is a temporary or permanent condition.

**500 Internal Server Error** : A generic error message, given when no more specific message is suitable.

**501 Not Implemented** : The server either does not recognise the request method, or it lacks the ability to fulfill the request.

**502 Bad Gateway** : The server was acting as a gateway or proxy and received an invalid reponse from the upstream server.

**503 Service Unavailable** : The server is currently unavailable (because it is overloaded or down for maintenance). Generally, this is a temporary state.

HTTP persistent connection
----------------------------

**HTTP persistent connection** , also called **HTTP keep-alive** , or **HTTP connection reuse** , is the idea of using the same TCP connection to send and receive multiple HTTP requests/responses, as opposed to opening a new connection for every single request/response pair.

Operation
^^^^^^^^^^

**Under HTTP 1.0** , there is no official specification for how keepalive operates. It was, in essence, tacked on to an existing protocol. If the browser supports keep-alive, it adds an additional header to the request:
::

    Connection: Keep-Alive

Then, when the server receives this request and generates a response, it also adds header to the response:
::

    Connection: Keep-Alive

Following this, the connection is NOT dropped, but is instead kept open. When the client sends another request, it uses the same connection. This will continue until either the client or the server decides that the conversation is over, and one of them drops the connection.

**In HTTP 1.1** all connections are considered persistent unless declared otherwise. The HTTP persistent connections do not use separate keepalive messages, they just allow multiple requests to use a single connection. However, the default connection timeout of Apache 2.0 httpd is as little as 15 seconds and for Apache 2.2 only 5 seconds. The advantage of a short timeout is the ability to deliver multiple components of a web page quickly while not tying up multiple server processes or threads for too long.

Disadvantages
^^^^^^^^^^^^^^^

It has been suggested with modern widespread high-bandwith connections, Keep-Alive might not be as useful as it once was. The webserver will keep a connection open for a certain number of seconds, which may hurt performance more than the total performance benefits.

For services where single documents are regularly requested (for example image hosting websites), Keep-Alive can be massively detrimental(有害的) to performance dut to keeping unnecessary connections open for many seconds after the document was retrieved.

Post/Redirect/Get
-------------------

**Post/Redirect/Get(PRG)** is a web development design pattern that prevents some duplicate form submissions, creating a more intuitive interface for user agents (users). **PRG** implements bookmarks and the refresh button in a predictable way that does not create duplicate form submissions.

HTTP cookie
-------------

A cookie, also known as an HTTP cookie, web cookie, or browser cookie, is used for an origin website to send state information to a user's browser and for the browser to return the state information to the origin site. The state information can be used for authentication, identification of a user session, user's preferences, shopping cart contents, or anything else that can be accomplished through storing text data on the user's computer.

Terminologies
^^^^^^^^^^^^^^^

**Session cookie** : A session cookie only lasts for the duration of users using the website. A web browser normally deletes session cookies when it quits. A session cookie is created when no expires directive is provided when the cookie is created.

**Persistent cookie** : A persistent cookie will outlast user sessions. If a persistent cookie has its Max-Age to 1 year, then, within the year, the initial value set in that cookie would be sent back to the server every time the user visited the server. This could be used to record a vital piece of information such as how the user initially came to this website. For this reason persistent cookies are also called tracking cookies.

**Secure cookie** : A secure cookie is only used when a browser is visiting a server via HTTPS, ensuring that the cookie is always encrypted when transmitting from client to server. This makes the cookie less likely to be exposed to cookie theft via eavesdropping.

Uses
^^^^^^

**Session management**

Cookies may be used to maintain data related to the user during navigation, possibly across multiple visits. Cookies were introduced to provide a way to implement a "shopping cart" (or "shopping basket"), a virtual device into which users can store items they want to purchase as they navigate throughout the site.

Shopping basket applications today usually store the list of basket contents in a database on the server side, rather than storing basket items in the cookie itself. A web server typically sends a cookie containing a unique session identifier. The web browser will send back that session identifier with each subsequent request and shopping basket items are stored associated with a unique session identifier.

Allowing users to log in to a website is a frequent use of cookies. Typically the web server will first send a cookie containing a unique session identifier. Users then submit their credentials and the web application authenticates the session and allows the user access to services.

**Personalization**

Cookies may be used to remember the information about the user who has visited a website in order to show relevant content in the future. For example a web server may send a cookie containing the username last used to log in to a website so that it may be filled for future visits.

Many websites use cookies for personalization based on users' preference. Users select their preferences by entering them in a web form and submitting the form to the server. The server encodes the preferences in a cookie and sends the cookie back to the browser. This way, every time the user accesses a page, the server is also sent the cookie where the preferences are stored, and can personalize the page according to the user preferences.

Implementation
^^^^^^^^^^^^^^^^

Cookies are arbitrary pieces of data chosen by the Web server and sent to the browser. The browser returns them unchanged to the server, introducing a state (memory of previous events) into otherwise stateless HTTP transactions. Without cookies, each retrieval of a Web page or component of a Web page is an isolated event, mostly unrelated to all other views of the pages of the same site. Other than being set by a web server, cookies can also be set by a script in a language such as JavaScript, if supported and enabled by the Web browser.

.. image:: https://lh3.googleusercontent.com/-2l7PsE0aHOc/T4wpiRxNhSI/AAAAAAAABAw/RrT0lbF39_k/s700/HTTP_cookie_exchange.png

**Cookie attributes**

Besides the name–value pair, servers can also set these cookie attributes: a cookie domain, a path, expiration time or maximum age, Secure flag and HttpOnly flag. Browsers will not send cookie attributes back to the server. They will only send the cookie’s name-value pair. Cookie attributes are used by browsers to determine when to delete a cookie, block a cookie or whether to send a cookie (name-value pair) to the servers.

Session
---------

在计算机专业术语中，Session是指一个终端用户与交互系统进行通信的时间间隔，通常指从注册进入系统到注销退出系统之间所经过的时间以及如果需要的话，可能还有一定的操作空间。

具体到Web中的Session指的就是用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间。

