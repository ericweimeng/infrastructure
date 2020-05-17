# Push vs Pull Architectures

There are two modes of data transfer between the client and the server. _HTTP PUSH_ & _HTTP PULL_

### HTTP PULL [\#](https://www.educative.io/courses/web-application-software-architecture-101/JE1yEz4XyZJ#http-pull) <a id="http-pull"></a>

For every response, there has to be a request first. The client sends the request & the server responds with the data. This is the default mode of HTTP communication, called the HTTP PULL mechanism.

The client pulls the data from the server whenever it requires. And it keeps doing it over and over to fetch the updated data.

An important thing to note here is that every request to the server and the response to it consumes bandwidth. Every hit on the server costs the business money & adds more load on the server.

What if there is no updated data available on the server, every time the client sends a request?

The client doesn’t know that, so naturally, it would keep sending the requests to the server over and over. This is not ideal & a waste of resources. Excessive pulls by the clients have the potential to bring down the server.

### HTTP PUSH [\#](https://www.educative.io/courses/web-application-software-architecture-101/JE1yEz4XyZJ#http-push) <a id="http-push"></a>

To tackle this, we have the HTTP PUSH based mechanism. In this mechanism, the client sends the request for particular information to the server, just for the first time, & after that the server keeps pushing the new updates to the client whenever they are available.

The client doesn’t have to worry about sending requests to the server, for data, every now & then. This saves a lot of network bandwidth & cuts down the load on the server by notches.

This is also known as a _Callback_. Client phones the server for information. The server responds, Hey!! I don’t have the information right now but I’ll call you back whenever it is available.

A very common example of this is user notifications. We have them in almost every web application today. We get notified whenever an event happens on the backend.

Clients use _AJAX \(Asynchronous JavaScript & XML\)_ to send requests to the server in the HTTP Pull based mechanism.

There are multiple technologies involved in the _HTTP Push_ based mechanism such as:

* _Ajax Long polling_
* _Web Sockets_
* _HTML5 Event Source_
* _Message Queues_
* _Streaming over HTTP_

There are two ways of pulling/fetching data from the server.

The first is sending an _HTTP GET_ request to the server manually by triggering an event, like by clicking a button or any other element on the web page.

The other is fetching data dynamically at regular intervals by using _AJAX_ without any human intervention.

#### AJAX – Asynchronous JavaScript & XML [\#](https://www.educative.io/courses/web-application-software-architecture-101/N01XnA6Ak1m#ajax-asynchronous-javascript-xml) <a id="ajax-asynchronous-javascript-xml"></a>

> _AJAX_ stands for asynchronous _JavaScript_ & _XML_. The name says it all, it is used for adding asynchronous behaviour to the web page.

![](https://www.educative.io/api/collection/6064040858091520/6411938009448448/page/4541869587431424/image/5892486276841472)

As we can see in the illustration above, instead of requesting the data manually every time with the click of a button. AJAX enables us to fetch the updated data from the server by automatically sending the requests over and over at stipulated intervals.

Upon receiving the updates, a particular section of the web page is updated dynamically by the _callback_ method. We see this behaviour all the time on news & sports websites, where the updated event information is dynamically displayed on the page without the need to reload it.

AJAX uses an _XMLHttpRequest_ object for sending the requests to the server which is built-in the browser and uses JavaScript to update the _HTML DOM_.

AJAX is commonly used with the _Jquery_ framework to implement the asynchronous behaviour on the UI.

This dynamic technique of requesting information from the server after regular intervals is known as _Polling_.



### Time To Live \(TTL\) [\#](https://www.educative.io/courses/web-application-software-architecture-101/YM9l8lxn530#time-to-live-ttl) <a id="time-to-live-ttl"></a>

In the regular client-server communication, which is _HTTP PULL_, there is a _Time to Live \(TTL\)_ for every request. It could be 30 secs to 60 secs, varies from browser to browser.

If the client doesn’t receive a response from the server within the TTL, the browser kills the connection & the client has to re-send the request hoping it would receive the data from the server before the TTL ends this time.

Open connections consume resources & there is a limit to the number of open connections a server can handle at one point in time. If the connections don’t close & new ones are being introduced, over time, the server will run out of memory. Hence, the TTL is used in client-server communication.

_But what if we are certain that the response will take more time than the TTL set by the browser?_

### Persistent Connection [\#](https://www.educative.io/courses/web-application-software-architecture-101/YM9l8lxn530#persistent-connection) <a id="persistent-connection"></a>

In this case, we need a _Persistent Connection_ between the client and the server.

> A persistent connection is a network connection between the client & the server that remains open for further requests & the responses, as opposed to being closed after a single communication.

It facilitates HTTP Push-based communication between the client and the server.

![](https://www.educative.io/api/collection/6064040858091520/6411938009448448/page/5285870967980032/image/5102209878458368)

### Heartbeat Interceptors [\#](https://www.educative.io/courses/web-application-software-architecture-101/YM9l8lxn530#heartbeat-interceptors) <a id="heartbeat-interceptors"></a>

_Now you might be wondering how is a persistent connection possible if the browser kills the open connections to the server every X seconds?_

The connection between the client and the server stays open with the help of _Heartbeat Interceptors_.

> These are just blank request responses between the client and the server to prevent the browser from killing the connection.

_Isn’t this resource-intensive?_

### Resource Intensive [\#](https://www.educative.io/courses/web-application-software-architecture-101/YM9l8lxn530#resource-intensive) <a id="resource-intensive"></a>

Yes, it is. Persistent connections consume a lot of resources in comparison to the HTTP Pull behaviour. But there are use cases where establishing a persistent connection is vital to the feature of an application.

For instance, a browser-based multiplayer game has a pretty large amount of request-response activity within a certain time in comparison to a regular web application.

It would be apt to establish a persistent connection between the client and the server from a user experience standpoint.

Long opened connections can be implemented by multiple techniques such as _Ajax Long Polling_, _Web Sockets_, _Server-Sent Events_ etc.

Let’s have a look into each of them.

### Web Sockets [\#](https://www.educative.io/courses/web-application-software-architecture-101/xlx9BQWpVLl#web-sockets) <a id="web-sockets"></a>

A _Web Socket_ connection is ideally preferred when we need a persistent _bi-directional low latency_ data flow from the client to server & back.

Typical use-cases of these are messaging, chat applications, real-time social streams & browser-based massive multiplayer games which have quite a number of read writes in comparison to a regular web app.

With Web Sockets, we can keep the client-server connection open as long as we want.

**Have bi-directional data?** Go ahead with Web Sockets. One more thing, Web Sockets tech doesn’t work over _HTTP_. It runs over _TCP_. The server & the client should both support web sockets or else it won’t work.

[The WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) & [Introducing WebSockets – Bringing Sockets to the Web](https://www.html5rocks.com/en/tutorials/websockets/basics/) are good resources for further reading on web sockets

### AJAX – Long Polling [\#](https://www.educative.io/courses/web-application-software-architecture-101/xlx9BQWpVLl#ajax-long-polling) <a id="ajax-long-polling"></a>

_Long Polling_ lies somewhere between _Ajax_ & _Web Sockets_. In this technique instead of immediately returning the response, the server holds the response until it finds an update to be sent to the client.

The connection in long polling stays open a bit longer in comparison to polling. The server doesn’t return an empty response. If the connection breaks, the client has to re-establish the connection to the server.

The upside of using this technique is that there are quite a smaller number of requests sent from the client to the server, in comparison to the regular polling mechanism. This reduces a lot of network bandwidth consumption.

Long polling can be used in simple asynchronous data fetch use cases when you do not want to poll the server every now & then.

### HTML5 Event Source API & Server Sent Events [\#](https://www.educative.io/courses/web-application-software-architecture-101/xlx9BQWpVLl#html5-event-source-api-server-sent-events) <a id="html5-event-source-api-server-sent-events"></a>

The _Server-Sent Events_ implementation takes a bit of a different approach. Instead of the client polling for data, the server automatically pushes the data to the client whenever the updates are available. The incoming messages from the server are treated as _Events_.

Via this approach, the servers can initiate data transmission towards the client once the client has established the connection with an initial request.

This helps in getting rid of a huge number of blank request-response cycles cutting down the bandwidth consumption by notches.

To implement server-sent events, the backend language should support the technology & on the UI _HTML5 Event source API_ is used to receive the data in-coming from the backend.

An important thing to note here is that once the client establishes a connection with the server, the data flow is in one direction only, that is from the server to the client.

SSE is ideal for scenarios such as a real-time feed like that of Twitter, displaying stock quotes on the UI, real-time notifications etc.

[This is a good resource for further reading on SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

### Streaming Over HTTP [\#](https://www.educative.io/courses/web-application-software-architecture-101/xlx9BQWpVLl#streaming-over-http) <a id="streaming-over-http"></a>

_Streaming Over HTTP_ is ideal for cases where we need to stream large data over HTTP by breaking it into smaller chunks. This is possible with _HTML5_ & a _JavaScript Stream API_.

![](https://www.educative.io/api/collection/6064040858091520/6411938009448448/page/5272067177971712/image/5628092083077120)

The technique is primarily used for streaming multimedia content, like large images, videos etc, over HTTP.

Due to this, we can watch a partially downloaded video as it continues to download, by playing the downloaded chunks on the client.

To stream data, both the client & the server agree to conform to some streaming settings. This helps them figure when the stream begins & ends over an HTTP request-response model.

[You can go through this resource for further reading on Stream API](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API/Concepts)

### Summary [\#](https://www.educative.io/courses/web-application-software-architecture-101/xlx9BQWpVLl#summary) <a id="summary"></a>

So, now we have an understanding of what _HTTP Pull_ & _Push_ is. We went through different technologies which help us establish a persistent connection between the client and the server.

Every tech has a specific use case, Ajax is used to dynamically update the web page by polling the server at regular intervals.

Long polling has a connection open time slightly longer than the polling mechanism.

Web Sockets have bi-directional data flow, whereas Server sent events facilitate data flow from the server to the client.

Streaming over HTTP facilitates streaming of large objects like multi-media files.

What tech would fit best for our use cases depends on the kind of application we intend to build.

Alright, let’s quickly gain an insight into the pros & cons of the client and the server-side rendering.

