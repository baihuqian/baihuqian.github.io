---
layout: "post"
title: "Python Notes: HTTP Requests"
date: "2018-09-07 18:50"
tags: Python
---

# Simple HTTP Request
For really simple HTTP client code, using the built-in `urllib` module is usually fine.

```python
from urllib import request, parse # Base URL being accessed

url = 'http://httpbin.org/get'
# Dictionary of query parameters (if any)
parms = {
   'name1' : 'value1',
   'name2' : 'value2'
}
# Encode the query string
querystring = parse.urlencode(parms)
# Make a GET request and read the response
u = request.urlopen(url+'?' + querystring)
resp = u.read()
```

If you need to send the query parameters in the request body using a POST method, encode them and supply them as an optional argument to `urlopen()`:

```python
from urllib import request, parse # Base URL being accessed
url = 'http://httpbin.org/post'
# Dictionary of query parameters (if any)
parms = {
   'name1' : 'value1',
   'name2' : 'value2'
}
# Encode the query string
querystring = parse.urlencode(parms)
# Make a POST request and read the response
u = request.urlopen(url, querystring.encode('ascii'))
resp = u.read()
```

If you need to supply some custom HTTP headers in the outgoing request such as a change to the user-agent field, make a dictionary containing their value and create a Request instance and pass it to `urlopen()`:

```python
from urllib import request, parse ...
# Extra headers
headers = {
    'User-agent' : 'none/ofyourbusiness',
    'Spam' : 'Eggs'
}
req = request.Request(url, querystring.encode('ascii'), headers=headers)
# Make a request and read the response
u = request.urlopen(req)
resp = u.read()
```

# `requests` Library
It is probably the go-to library to use to make REST API calls. It provides nice segregation between URL, parameters, and headers, and provides different APIs to make different HTTP requests. Note that `requests` can interpret different response types via different attributes. For example, the following example returns response in Unicode-decoded format.

```python
import requests
# Base URL being accessed
url = 'http://httpbin.org/post'
# Dictionary of query parameters (if any)
parms = {
   'name1' : 'value1',
   'name2' : 'value2'
}
# Extra headers
headers = {
    'User-agent' : 'none/ofyourbusiness',
    'Spam' : 'Eggs'
}
resp = requests.post(url, data=parms, headers=headers)
# Decoded text returned by the request
text = resp.text
```
