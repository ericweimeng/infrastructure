---
description: 'https://geekflare.com/curl-command-usage-with-example/'
---

# 11 cURL Command Usages

## Verify if you can connect to the URL

```text
curl sampleurl.com -I
```

### Success:

Return nothing if w/o -i/-I or with -i/-I:

```text
HTTP/1.1 302 Found
location: /bon
content-type: text/html; charset=utf-8
cache-control: private, no-cache, no-store, must-revalidate
content-length: 0
Date: Sat, 31 Jul 2021 19:21:18 GMT
Connection: keep-alive
```

### Failure:

Errors like:

```text
curl: (6) Could not resolve host: idontknow.com
```

## Save URL/URI output to file

If you have to save the URL or URI contents to a specific file, you can use the following syntax

```text
curl https://yoururl.com > yoururl.html
```

## Show request and response header

If you are having issues and would like to validate, you are getting the expected request and response header.

```text
curl -v yoururl.com
```

## Download at a limit rate

If you are working on optimization and would like to see how much time does it take to download at a particular speed, you can:-

```text
curl –-limit-rate 2000B
```

## Using a proxy to connect

Very handy if you are working on the [DMZ server](https://searchsecurity.techtarget.com/definition/DMZ#:~:text=In%20computer%20networks%2C%20a%20DMZ,%2D%2D%20usually%2C%20the%20public%20internet.&text=Servers%20and%20resources%20in%20the,the%20internal%20LAN%20remains%20unreachable.) where you need to connect to the external world using a proxy.

```text
curl --proxy yourproxy:port https://yoururl.com
```

## Test URL with injecting header

You can use curl by inserting a header with your data to test or troubleshoot the particular issue. Let’s see the following example to request with Content-Type.

```text
curl --header 'Content-Type: application/json' http://yoururl.com
```

By doing above, you are asking curl to pass Content-Type as application/json in the request header.

## Display only response header

If you are doing some troubleshooting and quickly want to check the response header, you can use the following syntax.

```text
curl --head http://yoururl.com
```

## Connect HTTPS/SSL URL and ignore any SSL certificate error

When you try to access SSL/TLS cert secured URL and if that is having the wrong cert or CN doesn’t match, then you will get the following error.

```text
curl: (51) Unable to communicate securely with peer: requested domain name does not match the server's certificate.
```

Good news, you can instruct cURL to ignore the cert error with `--insecure` flag.

```text
curl --insecure https://yoururl.com
```

## Connect using a specific protocol \(SSL/TLS\)

Very handy to test if a particular URL can handshake over specific SSL/TLS protocol.

To connect using SSL v3

```text
curl --sslv3 https://yoururl.com
```

and for different TLS versions

```text
curl --tlsv1 https://example.com
curl --tlsv1.0 https://example.com
curl --tlsv1.1 https://example.com
curl --tlsv1.2 https://example.com
curl --tlsv1.3 https://example.com
```



## Download file from FTP Server

You can use curl to download the file as well by specifying username and password.

```text
curl -u user:password -O ftp://ftpurl/style.css
```

Copy

You can always use “**-v**” with any syntax to print in verbose mode.

## Using Host Header

The host header is useful to test the target URL over IP when the requested content is only available when host header matches. Or, if you want to test application using load balancer IP/URL.

```text
curl --header 'Host: targetapplication.com' https://192.0.0.1:8080/
```



