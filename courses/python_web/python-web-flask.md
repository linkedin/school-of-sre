# Python, Web and Flask

Back in the old days, websites were simple. They were simple static html contents. A webserver would be listening on a defined port and according to the HTTP request received, it would read files from disk and return them in response. But since then, complexity has evolved and websites are now dynamic. Depending on the request, multiple operations need to be performed like reading from database or calling other API and finally returning some response (HTML data, JSON content etc.)

Since serving web requests is no longer a simple task like reading files from disk and return contents, we need to process each http request, perform some operations programmatically and construct a response.

## Sockets

Though we have frameworks like flask, HTTP is still a protocol that works over TCP protocol. So let us setup a TCP server and send an HTTP request and inspect the request's payload. Note that this is not a tutorial on socket programming but what we are doing here is inspecting HTTP protocol at its ground level and look at what its contents look like. (Ref: [Socket Programming in Python (Guide) on RealPython](https://realpython.com/python-sockets/))

```python
import socket

HOST = '127.0.0.1'  # Standard loopback interface address (localhost)
PORT = 65432        # Port to listen on (non-privileged ports are > 1023)

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
   s.bind((HOST, PORT))
   s.listen()
   conn, addr = s.accept()
   with conn:
       print('Connected by', addr)
       while True:
           data = conn.recv(1024)
           if not data:
               break
           print(data)
```

Then we open `localhost:65432` in our web browser and following would be the output:

```bash
Connected by ('127.0.0.1', 54719)
b'GET / HTTP/1.1\r\nHost: localhost:65432\r\nConnection: keep-alive\r\nDNT: 1\r\nUpgrade-Insecure-Requests: 1\r\nUser-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36 Edg/85.0.564.44\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\nSec-Fetch-Site: none\r\nSec-Fetch-Mode: navigate\r\nSec-Fetch-User: ?1\r\nSec-Fetch-Dest: document\r\nAccept-Encoding: gzip, deflate, br\r\nAccept-Language: en-US,en;q=0.9\r\n\r\n'
```

Examine closely and the content will look like the HTTP protocol's format. ie:

```
HTTP_METHOD URI_PATH HTTP_VERSION
HEADERS_SEPARATED_BY_SEPARATOR
```

So though it's a blob of bytes, knowing [http protocol specification](https://tools.ietf.org/html/rfc2616), you can parse that string (ie: split by `\r\n`) and get meaningful information out of it.

## Flask

Flask, and other such frameworks does pretty much what we just discussed in the last section (with added more sophistication). They listen on a port on a TCP socket, receive an HTTP request, parse the data according to protocol format and make it available to you in a convenient manner.

ie: you can access headers in flask by `request.headers` which is made available to you by splitting above payload by `/r/n`, as defined in http protocol.

Another example: we register routes in flask by `@app.route("/hello")`. What flask will do is maintain a registry internally which will map `/hello` with the function you decorated with. Now whenever a request comes with the `/hello` route (second component in the first line, split by space), flask calls the registered function and returns whatever the function returned.

Same with all other web frameworks in other languages too. They all work on similar principles. What they basically do is understand the HTTP protocol, parses the HTTP request data and gives us programmers a nice interface to work with HTTP requests.

Not so much of magic, innit?
