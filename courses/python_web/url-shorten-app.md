# The URL Shortening App

Let's build a very simple URL shortening app using flask and try to incorporate all aspects of the development process including the reliability aspects. We will not be building the UI and we will come up with a minimal set of API that will be enough for the app to function well.

## Design

We don't jump directly to coding. First thing we do is gather requirements. Come up with an approach. Have the approach/design reviewed by peers. Evolve, iterate, document the decisions and tradeoffs. And then finally implement. While we will not do the full blown design document here, we will raise certain questions here that are important to the design.

### 1. High Level Operations and API Endpoints

Since it's a URL shortening app, we will need an API for generating the shorten link given an original link. And an API/Endpoint which will accept the shorten link and redirect to original URL. We are not including the user aspect of the app to keep things minimal. These two API should make app functional and usable by anyone.

### 2. How to shorten?

Given a url, we will need to generate a shortened version of it. One approach could be using random characters for each link. Another thing that can be done is to use some sort of hashing algorithm. The benefit here is we will reuse the same hash for the same link. ie: if lot of people are shortening `https://www.linkedin.com` they all will have the same value, compared to multiple entries in DB if chosen random characters.

What about hash collisions? Even in random characters approach, though there is a less probability, hash collisions can happen. And we need to be mindful of them. In that case we might want to prepend/append the string with some random value to avoid conflict.

Also, choice of hash algorithm matters. We will need to analyze algorithms. Their CPU requirements and their characteristics. Choose one that suits the most.

### 3. Is URL Valid?

Given a URL to shorten, how do we verify if the URL is valid? Do we even verify or validate? One basic check that can be done is see if the URL matches a regex of a URL. To go even further we can try opening/visiting the URL. But there are certain gotchas here.

1. We need to define success criteria. ie: HTTP 200 means it is valid.
2. What is the URL is in private network?
3. What if URL is temporarily down?

### 4. Storage

Finally, storage. Where will we store the data that we will generate over time? There are multiple database solutions available and we will need to choose the one that suits this app the most. Relational database like MySQL would be a fair choice but **be sure to checkout School of SRE's [SQL database section](../databases_sql/intro.md) and [NoSQL databases section](../databases_nosql/intro.md) for deeper insights into making a more informed decision.**

### 5. Other

We are not accounting for users into our app and other possible features like rate limiting, customized links etc but it will eventually come up with time. Depending on the requirements, they too might need to get incorporated.

The minimal working code is given below for reference but I'd encourage you to come up with your own.

```python
from flask import Flask, redirect, request

from hashlib import md5

app = Flask("url_shortener")

mapping = {}

@app.route("/shorten", methods=["POST"])
def shorten():
    global mapping
    payload = request.json

    if "url" not in payload:
        return "Missing URL Parameter", 400

    # TODO: check if URL is valid

    hash_ = md5()
    hash_.update(payload["url"].encode())
    digest = hash_.hexdigest()[:5]  # limiting to 5 chars. Less the limit more the chances of collission

    if digest not in mapping:
        mapping[digest] = payload["url"]
        return f"Shortened: r/{digest}\n"
    else:
        # TODO: check for hash collission
        return f"Already exists: r/{digest}\n"


@app.route("/r/<hash_>")
def redirect_(hash_):
    if hash_ not in mapping:
        return "URL Not Found", 404
    return redirect(mapping[hash_])


if __name__ == "__main__":
    app.run(debug=True)

"""
OUTPUT:


===> SHORTENING

spatel1-mn1:tmp spatel1$ curl localhost:5000/shorten -H "content-type: application/json" --data '{"url":"https://linkedin.com"}'
Shortened: r/a62a4


===> REDIRECTING, notice the response code 302 and the location header

spatel1-mn1:tmp spatel1$ curl localhost:5000/r/a62a4 -v
* Uses proxy env variable NO_PROXY == '127.0.0.1'
*   Trying ::1...
* TCP_NODELAY set
* Connection failed
* connect to ::1 port 5000 failed: Connection refused
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /r/a62a4 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.1
> Accept: */*
>
* HTTP 1.0, assume close after body
< HTTP/1.0 302 FOUND
< Content-Type: text/html; charset=utf-8
< Content-Length: 247
< Location: https://linkedin.com
< Server: Werkzeug/0.15.4 Python/3.7.7
< Date: Tue, 27 Oct 2020 09:37:12 GMT
<
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>Redirecting...</title>
<h1>Redirecting...</h1>
* Closing connection 0
<p>You should be redirected automatically to target URL: <a href="https://linkedin.com">https://linkedin.com</a>.  If not click the link.
"""
```
