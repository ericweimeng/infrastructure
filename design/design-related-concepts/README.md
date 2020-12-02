# Design Related Concepts

## Whitelist vs Blacklist

White lists and black lists are two ways of filtering data. If you have a white list then you will filter in only data on the white list; if you have a black list you will filter out only data on that list.

For example, consider automatically rejecting incoming phone calls. You could have a black list of marketing companies, so everyone but them would be able to call you. Or you could have white list containing your friends' numbers, so only they would be able to call you.

TL;DR:

* Whitelist - only these things.
* Blacklist - everything but these things.

## \(Forward\) Proxy and Reverse Proxy

The main difference between the two is that **forward proxy is used by the client** such as a web browser whereas **reverse proxy is used by the server** such as a web server. Forward proxy can reside in the same internal network as the client, or it can be on the Internet.

### Forward Proxy

**Forward proxy can be used by the client to** [bypass firewall restrictions](https://www.linuxbabe.com/ubuntu/shadowsocks-libev-proxy-server-ubuntu-16-04-17-10) in order to visit websites that are blocked by school, government, company etc. If a website blocked an IP range from visiting the website, then a person in that IP range can use forward proxy to hide the real IP of the client so that person can visit the website and maybe leave some spam comments. However forward proxy might be detected by the website administrator. There are some paid proxy service that has numerous proxy systems around the world so that they can change your IP address every time your visit a new web page and this makes it harder for website administrators to detect.

**Forward proxy was very useful and popular in the 1990s.** Before NAT is integrated into network routers, forward proxy is the way for multiple computers in the same network to access the Internet. This type of forward proxy usually resides in the internal network.

**Forward proxy can also act as a cache server in an internal network.** If a resource is download many times, then the proxy can cache the content on the server so next time when another computer download the same content, the proxy will send the content that is previously stored on the server to the computer.

There’re many different kinds of forward proxy such as [web proxy](https://www.linuxbabe.com/ubuntu/create-your-own-web-proxy-ubuntu-16-04-glype-php-proxy), HTTP proxy, SOCKS proxy etc. Please keep mind that using forward proxy to browse the Internet usually slows down your overall Internet speed. That depends on the location between your computer and the forward proxy and how many people are using that forward proxy.

Another thing to be aware of is that there’re many free forward proxies which is built by hackers for malicious purpose. If you happen to be using one of these proxies, they will log every activity you do on the Internet.  So free in charge is actually very costly.

### Reverse Proxy

A reverse proxy is an intermediary proxy service which takes a client request, passes it on to one or more servers, and subsequently delivers the server's response to the client. A common reverse proxy configuring is to put **Nginx in front of an Apache web server**. Using this method will allow both web servers to work together enabling each to do what they do best

#### Benefits of an Nginx reverse proxy[\#](https://www.keycdn.com/support/nginx-reverse-proxy#benefits-of-an-nginx-reverse-proxy)

There are a few benefits to setting up an Nginx reverse proxy. Although not required in all cases, it can be beneficial depending upon your particular scenario/setup. The following outlines a few benefits implementing a reverse proxy.

* **Load balancing** - A reverse proxy can perform load balancing which helps distribute client requests evenly across backend servers. This process greatly helps in avoiding the scenario where a particular server becomes overloaded due to a sudden spike in requests. Load balancing also improves redundancy as if one server goes down, the reverse proxy will simply reroute requests to a different server. Read our complete article to learn more about [load balancing](https://www.keycdn.com/support/load-balancing).
* **Increased security** - A reverse proxy also acts as a line of defense for your backend servers. Configuring a reverse proxy ensures that the identity of your backend servers remains unknown. This can greatly help in protecting your servers from attacks such as [DDoS](https://www.keycdn.com/support/ddos-attack) for example.
* **Better performance** - Nginx has been known to perform better in delivering static content over Apache. Therefore with an Nginx reverse proxy, all client requests can be handled by Nginx while all requests for dynamic content can be passed on to the backend Apache server. This helps improve performance by optimizing the delivery of assets based on their type. Additionally, reverse proxies can also be used to serve cached content and perform SSL encryption to take a load off the web server\(s\).
* **Easy logging and auditing** - Since there is only one single point of access when a reverse proxy is implemented, this makes logging and auditing much simpler. Using this method, you can easily monitor what goes in and out through the reverse proxy.

### How to setup an Nginx reverse proxy[\#](https://www.keycdn.com/support/nginx-reverse-proxy#how-to-setup-an-nginx-reverse-proxy) <a id="how-to-setup-an-nginx-reverse-proxy"></a>

Now that we've covered the benefits of setting up a reverse proxy, we'll go through a simple example of how to configure an **Nginx reverse proxy in front of an Apache web server**. We'll define the IP address of the Nginx reverse proxy to be `192.x.x.1` and the backend Apache server to be `192.x.x.2`. In the following example, we assume that Apache is already installed and configured. Therefore to install Nginx the following command must be executed \(remember to use sudo if you aren't logged in as root\).

```text
apt-get update
apt-get install nginx
```

Once Nginx has been installed, the next step is to disable the default virtual host.

```text
unlink /etc/nginx/sites-enabled/default
```

Then, we need to create a file within the `/etc/nginx/sites-available` directory that contains the reverse proxy information. We can name this `reverse-proxy.conf` for example.

```text
server {
    listen 80;
    location / {
        proxy_pass http://192.x.x.2;
    }
}
```

The above snippet is quite minimalistic and straightforward. The important part here is the `proxy_pass` directive which is essentially telling any requests coming through the Nginx reverse proxy to be passed along to the Apache remote socket `192.x.x.2:80`. There are many other proxy configurations you can define within your Nginx configuration settings. To learn more about the directives available, check out the [Nginx proxy module documentation](http://nginx.org/en/docs/http/ngx_http_proxy_module.html).

Once you've added the appropriate directives to your `.conf` file, activate it by linking to `/sites-enabled/` using the following command.

```text
ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
```

Lastly, run an Nginx configuration test and restart Nginx.

```text
service nginx configtest
service nginx restart
```

## CDN \(Content delivery network\)

A content delivery network \(CDN\) is a globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon's CloudFront support dynamic content. The site's DNS resolution will tell clients which server to contact.

Serving content from CDNs can significantly improve performance in two ways:

* Users receive content at data centers close to them
* Your servers do not have to serve requests that the CDN fulfills

#### Push CDNs

Push CDNs receive new content whenever changes occur on your server. You take full responsibility for providing content, uploading directly to the CDN and rewriting URLs to point to the CDN. You can configure when content expires and when it is updated. Content is uploaded only when it is new or changed, minimizing traffic, but maximizing storage.

Sites with a small amount of traffic or sites with content that isn't often updated work well with push CDNs. Content is placed on the CDNs once, instead of being re-pulled at regular intervals.

#### Pull CDNs

Pull CDNs grab new content from your server when the first user requests the content. You leave the content on your server and rewrite URLs to point to the CDN. This results in a slower request until the content is cached on the CDN.

A [time-to-live \(TTL\)](https://en.wikipedia.org/wiki/Time_to_live) determines how long content is cached. Pull CDNs minimize storage space on the CDN, but can create redundant traffic if files expire and are pulled before they have actually changed.

Sites with heavy traffic work well with pull CDNs, as traffic is spread out more evenly with only recently-requested content remaining on the CDN.

#### Disadvantage\(s\): CDN

* CDN costs could be significant depending on traffic, although this should be weighed with additional costs you would incur not using a CDN.
* Content might be stale if it is updated before the TTL expires it.
* CDNs require changing URLs for static content to point to the CDN.

## Backpressure

{% embed url="https://medium.com/@jayphelps/backpressure-explained-the-flow-of-data-through-software-2350b3e77ce7" %}



## Caching

Load balancing helps you scale horizontally across an ever-increasing number of servers, but caching will enable you to make vastly better use of the resources you already have as well as making otherwise unattainable product requirements feasible. Caches take advantage of the locality of reference principle: recently requested data is likely to be requested again. They are used in almost every layer of computing: hardware, operating systems, web browsers, web applications, and more. A cache is like short-term memory: it has a limited amount of space, but is typically faster than the original data source and contains the most recently accessed items. Caches can exist at all levels in architecture, but are often found at the level nearest to the front end where they are implemented to return data quickly without taxing downstream levels.

#### Application server cache <a id="div-stylecolorblack-background-colore2f4c7-border-radius5px-padding5pxapplication-server-cachediv"></a>

Placing a cache directly on a request layer node enables the local storage of response data. Each time a request is made to the service, the node will quickly return local cached data if it exists. If it is not in the cache, the requesting node will query the data from disk. The cache on one request layer node could also be located both in memory \(which is very fast\) and on the node’s local disk \(faster than going to network storage\).

What happens when you expand this to many nodes? If the request layer is expanded to multiple nodes, it’s still quite possible to have each node host its own cache. However, if your load balancer randomly distributes requests across the nodes, the same request will go to different nodes, thus increasing cache misses. Two choices for overcoming this hurdle are global caches and distributed caches.

#### Content Distribution Network \(CDN\) <a id="div-stylecolorblack-background-colore2f4c7-border-radius5px-padding5pxcontent-distribution-network-cdndiv"></a>

CDNs are a kind of cache that comes into play for sites serving large amounts of static media. In a typical CDN setup, a request will first ask the CDN for a piece of static media; the CDN will serve that content if it has it locally available. If it isn’t available, the CDN will query the back-end servers for the file, cache it locally, and serve it to the requesting user.

If the system we are building isn’t yet large enough to have its own CDN, we can ease a future transition by serving the static media off a separate subdomain \(e.g. [static.yourservice.com](http://static.yourservice.com/)\) using a lightweight HTTP server like Nginx, and cut-over the DNS from your servers to a CDN later.

#### Cache Invalidation <a id="div-stylecolorblack-background-colore2f4c7-border-radius5px-padding5px-cache-invalidationdiv"></a>

While caching is fantastic, it does require some maintenance for keeping cache coherent with the source of truth \(e.g., database\). If the data is modified in the database, it should be invalidated in the cache; if not, this can cause inconsistent application behavior.

Solving this problem is known as cache invalidation; there are three main schemes that are used:

**Write-through cache:** Under this scheme, data is written into the cache and the corresponding database at the same time. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have complete data consistency between the cache and the storage. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions.

Although, write through minimizes the risk of data loss, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of higher latency for write operations.

**Write-around cache:** This technique is similar to write through cache, but data is written directly to permanent storage, bypassing the cache. This can reduce the cache being flooded with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from slower back-end storage and experience higher latency.

**Write-back cache:** Under this scheme, data is written to cache alone and completion is immediately confirmed to the client. The write to the permanent storage is done after specified intervals or under certain conditions. This results in low latency and high throughput for write-intensive applications, however, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.

#### Cache eviction policies <a id="div-stylecolorblack-background-colore2f4c7-border-radius5px-padding5px-cache-eviction-policiesdiv"></a>

Following are some of the most common cache eviction policies:

1. First In First Out \(FIFO\): The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
2. Last In First Out \(LIFO\): The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
3. Least Recently Used \(LRU\): Discards the least recently used items first.
4. Most Recently Used \(MRU\): Discards, in contrast to LRU, the most recently used items first.
5. Least Frequently Used \(LFU\): Counts how often an item is needed. Those that are used least often are discarded first.
6. Random Replacement \(RR\): Randomly selects a candidate item and discards it to make space when necessary.

## Performance vs scalability

A service is **scalable** if it results in increased **performance** in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.[1](http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)

Another way to look at performance vs scalability:

* If you have a **performance** problem, your system is slow for a single user.
* If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.

## Latency vs throughput

**Latency** is the time to perform some action or to produce some result.

**Throughput** is the number of such actions or results per unit of time.

Generally, you should aim for **maximal throughput** with **acceptable latency**.

## Availability vs consistency

#### CAP theorem

[![](https://camo.githubusercontent.com/13719354da7dcd34cd79ff5f8b6306a67bc18261/687474703a2f2f692e696d6775722e636f6d2f62674c4d4932752e706e67)](https://camo.githubusercontent.com/13719354da7dcd34cd79ff5f8b6306a67bc18261/687474703a2f2f692e696d6775722e636f6d2f62674c4d4932752e706e67)  
[Source: CAP theorem revisited](http://robertgreiner.com/2014/08/cap-theorem-revisited)

In a distributed computer system, you can only support two of the following guarantees:

* **Consistency** - Every read receives the most recent write or an error
* **Availability** - Every request receives a response, without guarantee that it contains the most recent version of the information
* **Partition Tolerance** - The system continues to operate despite arbitrary partitioning due to network failures

_Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability._

**CP - consistency and partition tolerance**

Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.

**AP - availability and partition tolerance**

Responses return the most recent version of the data available on a node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.

AP is a good choice if the business needs allow for [eventual consistency](https://github.com/donnemartin/system-design-primer#eventual-consistency) or when the system needs to continue working despite external errors.

### Consistency patterns

With multiple copies of the same data, we are faced with options on how to synchronize them so clients have a consistent view of the data. Recall the definition of consistency from the [CAP theorem](https://github.com/donnemartin/system-design-primer#cap-theorem) - Every read receives the most recent write or an error.

#### Weak consistency

After a write, reads may or may not see it. A best effort approach is taken.

This approach is seen in systems such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss.

#### Eventual consistency

After a write, reads will eventually see it \(typically within milliseconds\). Data is replicated asynchronously.

This approach is seen in systems such as DNS and email. Eventual consistency works well in highly available systems.

#### Strong consistency

After a write, reads will see it. Data is replicated synchronously.

This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that need transactions.

There are two main patterns to support high availability: **fail-over** and **replication**.

### Availability patterns

#### Fail-over

**Active-passive**

With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.

The length of downtime is determined by whether the passive server is already running in 'hot' standby or whether it needs to start up from 'cold' standby. Only the active server handles traffic.

Active-passive failover can also be referred to as master-slave failover.

**Active-active**

In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers. If the servers are internal-facing, application logic would need to know about both servers.

Active-active failover can also be referred to as master-master failover.

#### Disadvantage\(s\): failover

* Fail-over adds more hardware and additional complexity.
* There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.

#### Replication

**Master-slave and master-master**

## Domain name system

[![](https://camo.githubusercontent.com/fae27d1291ed38dd120595d692eacd2505cd3a9c/687474703a2f2f692e696d6775722e636f6d2f494f794c6a34692e6a7067)](https://camo.githubusercontent.com/fae27d1291ed38dd120595d692eacd2505cd3a9c/687474703a2f2f692e696d6775722e636f6d2f494f794c6a34692e6a7067)  
[Source: DNS security presentation](http://www.slideshare.net/srikrupa5/dns-security-presentation-issa)

A Domain Name System \(DNS\) translates a domain name such as [www.example.com](http://www.example.com/) to an IP address.

DNS is hierarchical, with a few authoritative servers at the top level. Your router or ISP provides information about which DNS server\(s\) to contact when doing a lookup. Lower level DNS servers cache mappings, which could become stale due to DNS propagation delays. DNS results can also be cached by your browser or OS for a certain period of time, determined by the [time to live \(TTL\)](https://en.wikipedia.org/wiki/Time_to_live).

* **NS record \(name server\)** - Specifies the DNS servers for your domain/subdomain.
* **MX record \(mail exchange\)** - Specifies the mail servers for accepting messages.
* **A record \(address\)** - Points a name to an IP address.
* **CNAME \(canonical\)** - Points a name to another name or `CNAME` \(example.com to [www.example.com](http://www.example.com/)\) or to an `A` record.

Services such as [CloudFlare](https://www.cloudflare.com/dns/) and [Route 53](https://aws.amazon.com/route53/) provide managed DNS services. Some DNS services can route traffic through various methods:

* [Weighted round robin](https://www.g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb)
  * Prevent traffic from going to servers under maintenance
  * Balance between varying cluster sizes
  * A/B testing
* Latency-based
* Geolocation-based

#### Disadvantage\(s\): DNS

* Accessing a DNS server introduces a slight delay, although mitigated by caching described above.
* DNS server management could be complex and is generally managed by [governments, ISPs, and large companies](http://superuser.com/questions/472695/who-controls-the-dns-servers/472729).
* DNS services have recently come under [DDoS attack](http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/), preventing users from accessing websites such as Twitter without knowing Twitter's IP address\(es\).

