# Proxy

Quite often you may have seen some notification in your PC to add and configure the proxy servers but what exactly proxy servers are and how it works? Typically proxy servers are some bit of code or intermediary piece of hardware/software that sits between a client and another server. It may reside on the user’s local computer or anywhere in between the clients and the destination servers. A proxy server receives requests from the client and transmits it to the origin servers, then it forwards the received response from the server to the originator client. In some cases, when the server receives the request the IP address is not associated with the client but it is of the proxy server. This happens when the proxy server hides the identity of the client. 

In general, when people use the term proxy they refer to ‘forward proxy’. ‘Forward proxy’ is designed to help users and it acts on behalf of \(substitute for\) the client in the interaction between client and server. It forwards the user’s requests and acts as a personal representative of the user. In system, design especially in complex systems, proxies are very useful, particularly ‘reverse proxies’ are useful. ‘Reverse proxies’ are the opposite of ‘forward proxy’. A reverse proxy acts on behalf of a server and it is designed to help servers. 

In the ‘forward proxy’ server won’t know that the request and response are routed through the proxy, and in a reverse proxy, the client won’t know that the request and response are traveling through a proxy. A ‘reverse proxy’ can be assigned a lot of tasks to help the main server and it can act as a gatekeeper, a screener, a load-balancer, and an all-around assistant. 

Typically proxies are used to handle requests, filter requests or log requests, or sometimes transform requests \(by adding/removing headers, encrypting/decrypting, or compression\). It helps in coordinating requests from multiple servers and it can be used to optimize request traffic from a system-wide perspective. 

**Forward Proxy:**   
 

![Forward Proxy](https://media.geeksforgeeks.org/wp-content/uploads/20200824215915/ForwardProxySystemDesign.png)

**Reverse Proxy:**   
 

![Reverse Proxy](https://media.geeksforgeeks.org/wp-content/uploads/20200824220014/ReverseProxySystemDesign.png)

