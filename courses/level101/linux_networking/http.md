# HTTP

Till this point we have only got the IP address of [linkedin.com](https://www.linkedin.com/). The HTML page of [linkedin.com](https://www.linkedin.com/) is served by HTTP protocol which the browser renders. Browser sends a HTTP request to the IP of the server determined above.
Request has a verb GET, PUT, POST followed by a path and query parameters and lines of key-value pair which gives information about the client and capabilities of the client like contents it can accept and a body (usually in POST or PUT).

```bash
# Eg. run the following in your container and have a look at the headers 
curl linkedin.com -v
```
```bash
* Connected to linkedin.com (108.174.10.10) port 80 (#0)
> GET / HTTP/1.1
> Host: linkedin.com
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 09 Nov 2020 10:39:43 GMT
< X-Li-Pop: prod-esv5
< X-LI-Proto: http/1.1
< Location: https://www.linkedin.com/
< Content-Length: 0
< 
* Connection #0 to host linkedin.com left intact
* Closing connection 0
```

Here, in the first line `GET` is the verb, `/` is the path and `1.1` is the HTTP protocol version. Then there are key-value pairs which give client capabilities and some details to the server. The server responds back with HTTP version, [Status Code and Status message](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). Status codes `2xx` means success, `3xx` denotes redirection, `4xx` denotes client-side errors and `5xx` server-side errors.

We will now jump in to see the difference between HTTP/1.0 and HTTP/1.1. 

```bash
#On the terminal type
telnet  www.linkedin.com 80
#Copy and paste the following with an empty new line at last in the telnet STDIN
GET / HTTP/1.1
HOST:linkedin.com
USER-AGENT: curl

```

This would get server response and waits for next input as the underlying connection to [www.linkedin.com](https://www.linkedin.com/) can be reused for further queries. While going through TCP, we can understand the benefits of this. But in HTTP/1.0, this connection will be immediately closed after the response meaning new connection has to be opened for each query. HTTP/1.1 can have only one inflight request in an open connection but connection can be reused for multiple requests one after another. One of the benefits of HTTP/2.0 over HTTP/1.1 is we can have multiple inflight requests on the same connection. We are restricting our scope to generic HTTP and not jumping to the intricacies of each protocol version but they should be straight forward to understand post the course.

HTTP is called **stateless protocol**. This section we will try to understand what stateless means. Say we logged in to [linkedin.com](https://www.linkedin.com/), each request to [linkedin.com](https://www.linkedin.com/) from the client will have no context of the user and it makes no sense to prompt user to login for each page/resource. This problem of HTTP is solved by *COOKIE*. A user is created a session when a user logs in. This session identifier is sent to the browser via *SET-COOKIE* header. The browser stores the COOKIE till the expiry set by the server and sends the cookie for each request from hereon for [linkedin.com](https://www.linkedin.com/). More details on cookies are available [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies). Cookies are a critical piece of information like password and since HTTP is a plain text protocol, any man-in-the-middle can capture either password or cookies and can breach the privacy of the user. Similarly as discussed during DNS, a spoofed IP of [linkedin.com](https://www.linkedin.com/) can cause a phishing attack on users where an user can give LinkedIn’s password to login on the malicious site. To solve both problems, HTTPS came in place and HTTPS has to be mandated.

HTTPS has to provide server identification and encryption of data between client and server. The server administrator has to generate a private-public key pair and certificate request. This certificate request has to be signed by a certificate authority which converts the certificate request to a certificate. The server administrator has to update the certificate and private key to the webserver. The certificate has details about the server (like domain name for which it serves, expiry date), public key of the server. The private key is a secret to the server and losing the private key loses the trust the server provides. When clients connect, the client sends a HELLO. The server sends its certificate to the client. The client checks the validity of the cert by seeing if it is within its expiry time, if it is signed by a trusted authority and the hostname in the cert is the same as the server. This validation makes sure the server is the right server and there is no phishing. Once that is validated, the client negotiates a symmetrical key and cipher with the server by encrypting the negotiation with the public key of the server. Nobody else other than the server who has the private key can understand this data. Once negotiation is complete, that symmetric key and algorithm is used for further encryption which can be decrypted only by client and server from thereon as they only know the symmetric key and algorithm. The switch to symmetric algorithm from asymmetric encryption algorithm is to not strain the resources of client devices as symmetric encryption is generally less resource intensive than asymmetric. 

```bash
# Try the following on your terminal to see the cert details like Subject Name (domain name), Issuer details, Expiry date
curl https://www.linkedin.com -v 
```
```bash
* Connected to www.linkedin.com (13.107.42.14) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
} [230 bytes data]
* TLSv1.2 (IN), TLS handshake, Server hello (2):
{ [90 bytes data]
* TLSv1.2 (IN), TLS handshake, Certificate (11):
{ [3171 bytes data]
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
{ [365 bytes data]
* TLSv1.2 (IN), TLS handshake, Server finished (14):
{ [4 bytes data]
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
} [102 bytes data]
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
} [1 bytes data]
* TLSv1.2 (OUT), TLS handshake, Finished (20):
} [16 bytes data]
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
{ [1 bytes data]
* TLSv1.2 (IN), TLS handshake, Finished (20):
{ [16 bytes data]
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: C=US; ST=California; L=Sunnyvale; O=LinkedIn Corporation; CN=www.linkedin.com
*  start date: Oct  2 00:00:00 2020 GMT
*  expire date: Apr  2 12:00:00 2021 GMT
*  subjectAltName: host "www.linkedin.com" matched cert's "www.linkedin.com"
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert SHA2 Secure Server CA
*  SSL certificate verify ok.
* Using HTTP2, server supports multi-use
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* Using Stream ID: 1 (easy handle 0x7fb055808200)
* Connection state changed (MAX_CONCURRENT_STREAMS == 100)!
  0 82117    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* Connection #0 to host www.linkedin.com left intact
HTTP/2 200 
cache-control: no-cache, no-store
pragma: no-cache
content-length: 82117
content-type: text/html; charset=utf-8
expires: Thu, 01 Jan 1970 00:00:00 GMT
set-cookie: JSESSIONID=ajax:2747059799136291014; SameSite=None; Path=/; Domain=.www.linkedin.com; Secure
set-cookie: lang=v=2&lang=en-us; SameSite=None; Path=/; Domain=linkedin.com; Secure
set-cookie: bcookie="v=2&70bd59e3-5a51-406c-8e0d-dd70befa8890"; domain=.linkedin.com; Path=/; Secure; Expires=Wed, 09-Nov-2022 22:27:42 GMT; SameSite=None
set-cookie: bscookie="v=1&202011091050107ae9b7ac-fe97-40fc-830d-d7a9ccf80659AQGib5iXwarbY8CCBP94Q39THkgUlx6J"; domain=.www.linkedin.com; Path=/; Secure; Expires=Wed, 09-Nov-2022 22:27:42 GMT; HttpOnly; SameSite=None
set-cookie: lissc=1; domain=.linkedin.com; Path=/; Secure; Expires=Tue, 09-Nov-2021 10:50:10 GMT; SameSite=None
set-cookie: lidc="b=VGST04:s=V:r=V:g=2201:u=1:i=1604919010:t=1605005410:v=1:sig=AQHe-KzU8i_5Iy6MwnFEsgRct3c9Lh5R"; Expires=Tue, 10 Nov 2020 10:50:10 GMT; domain=.linkedin.com; Path=/; SameSite=None; Secure
x-fs-txn-id: 2b8d5409ba70
x-fs-uuid: 61bbf94956d14516302567fc882b0000
expect-ct: max-age=86400, report-uri="https://www.linkedin.com/platform-telemetry/ct"
x-xss-protection: 1; mode=block
content-security-policy-report-only: default-src 'none'; connect-src 'self' www.linkedin.com www.google-analytics.com https://dpm.demdex.net/id lnkd.demdex.net blob: https://linkedin.sc.omtrdc.net/b/ss/ static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; script-src 'sha256-THuVhwbXPeTR0HszASqMOnIyxqEgvGyBwSPBKBF/iMc=' 'sha256-PyCXNcEkzRWqbiNr087fizmiBBrq9O6GGD8eV3P09Ik=' 'sha256-2SQ55Erm3CPCb+k03EpNxU9bdV3XL9TnVTriDs7INZ4=' 'sha256-S/KSPe186K/1B0JEjbIXcCdpB97krdzX05S+dHnQjUs=' platform.linkedin.com platform-akam.linkedin.com platform-ecst.linkedin.com platform-azur.linkedin.com static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; img-src data: blob: *; font-src data: *; style-src 'self' 'unsafe-inline' static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; media-src dms.licdn.com; child-src blob: *; frame-src 'self' lnkd.demdex.net linkedin.cdn.qualaroo.com; manifest-src 'self'; report-uri https://www.linkedin.com/platform-telemetry/csp?f=g
content-security-policy: default-src *; connect-src 'self' https://media-src.linkedin.com/media/ www.linkedin.com s.c.lnkd.licdn.com m.c.lnkd.licdn.com s.c.exp1.licdn.com s.c.exp2.licdn.com m.c.exp1.licdn.com m.c.exp2.licdn.com wss://*.linkedin.com dms.licdn.com https://dpm.demdex.net/id lnkd.demdex.net blob: https://accounts.google.com/gsi/status https://linkedin.sc.omtrdc.net/b/ss/ www.google-analytics.com static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com media.licdn.com media-exp1.licdn.com media-exp2.licdn.com media-exp3.licdn.com; img-src data: blob: *; font-src data: *; style-src 'unsafe-inline' 'self' static-src.linkedin.com *.licdn.com; script-src 'report-sample' 'unsafe-inline' 'unsafe-eval' 'self' spdy.linkedin.com static-src.linkedin.com *.ads.linkedin.com *.licdn.com static.chartbeat.com www.google-analytics.com ssl.google-analytics.com bcvipva02.rightnowtech.com www.bizographics.com sjs.bizographics.com js.bizographics.com d.la4-c1-was.salesforceliveagent.com slideshare.www.linkedin.com https://snap.licdn.com/li.lms-analytics/ platform.linkedin.com platform-akam.linkedin.com platform-ecst.linkedin.com platform-azur.linkedin.com; object-src 'none'; media-src blob: *; child-src blob: lnkd-communities: voyager: *; frame-ancestors 'self'; report-uri https://www.linkedin.com/platform-telemetry/csp?f=l
x-frame-options: sameorigin
x-content-type-options: nosniff
strict-transport-security: max-age=2592000
x-li-fabric: prod-lva1
x-li-pop: afd-prod-lva1
x-li-proto: http/2
x-li-uuid: Ybv5SVbRRRYwJWf8iCsAAA==
x-msedge-ref: Ref A: CFB9AC1D2B0645DDB161CEE4A4909AEF Ref B: BOM02EDGE0712 Ref C: 2020-11-09T10:50:10Z
date: Mon, 09 Nov 2020 10:50:10 GMT

* Closing connection 0
```

Here, my system has a list of certificate authorities it trusts in this file `/etc/ssl/cert.pem`. cURL validates the certificate is for [www.linkedin.com](https://www.linkedin.com/) by seeing the CN section of the subject part of the certificate. It also makes sure the certificate is not expired by seeing the expire date. It also validates the signature on the certificate by using the public key of issuer Digicert in `/etc/ssl/cert.pem`. Once this is done, using the public key of [www.linkedin.com](https://www.linkedin.com/) it negotiates cipher `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384` with a symmetric key. Subsequent data transfer including first HTTP request uses the same cipher and symmetric key.


