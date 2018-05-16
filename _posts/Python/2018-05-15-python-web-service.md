---
layout: "post"
title: "Python Notes: Web Service"
date: "2018-05-15 21:37"
tags: Python
---

## Standard Python libraries

* `http.client` implements the low-level HTTP Protocol. You won't interact with it directly.
* `urllib.request`: standard API for accessing HTTP and FTP servers.
	* `urllib.request.urlopen(url)` takes a `url` string and returns a file-like object so that methods dealing with stream objects can be used.
	* `urllib.parse.urlencode(dic)` takes a dictionary and transforms it into a *URL-encoded* string. The string can be used as a payload in POST request.
	* It's very inefficient.

Note: HTTP servers don't deal with abstractions so data you get is in `bytes` format.


## Third Party

`httplib2`: The one to use.

* `http = httplib2.HTTP(dir)` `HTTP` object: the primary interface to `httplib2`. Pass a directory (caching) name to it (even it doesn't exist). You only need **ONE** `HTTP` object unless you're absolutely knows why.
* `httplib2.debuglevel` is 1 means printing all the data exchange between the server and local machine. After altering this value, create a **new** `HTTP` object.
* `http.request(url, type, payload)` issues an HTTP request for given `url`.
	* It returns two values: `response` contains all headers the server returned in a dictionary; `content` contains the actual data server returned in a `bytes` object.
	* The second parameter is the `type` of HTTP request, default is GET.
	* The third parameter is the `payload` to send to the server.
	* Optional `headers` parameter takes a dictionary with specified HTTP headers.
* `response.status` is the HTTP status code.
* `response.fromcache`tells if the response is from cache, not the server.
* `http.add_credentials(username, password, domain)` stores authentication information for a certain `domain` in `HTTP` object.
