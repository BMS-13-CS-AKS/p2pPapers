# Notes on content delivery

## Introduction

- the web is all about communication and content distribution [1]
- shift from communication to content
- talk about different kinds of content(small)
- popular content distribution sites have large loads
- for content distribution no single server can handle such loads [1]
- talk about large content providers like youtube google etc.

- internet traffic changes quickly in [1]
  - details and overall makeup
  - its properties are highly skewed
- zipfs law???

## Different methods for creating large web sites

### Server Farms [1]
- server side improvement
- cluster of computers act like a single server
- the farm must look like one logical entity to the clients.Possible solutions:
  - DNS server return rotating list of IPs
  - front end (switch or router) spraying
    - broadcast
    - mapping by inspecting headers
  -front end method requires that the front end checks whether a server is down to make sure it is reliable

### Web Proxies
- client side improvement
- browser caches responses to specific requests
- shortens response time and load on server.
- browser must check if cached responses are still fresh but this request is still better than requesting for total page
- but browser cannot cache all the sites visited by the user
- web proxy is used to share cache
- fetches web requests and caches responses
- typical setup is that a company or isp sets up a web proxy for all its users to reduce bandwidth usage
- working
  1. client sends request to proxy
  2. proxy checks cache
  3. if not there in cache fetches page from server
  4. serves page back

The above two methods cannot be applied for populat sites that serve content on a global scale

### Content Delivery Networks







#### TODO
- [ ] content distibution in youtube and other large players
- [ ] shift from communication to content
- [ ] zipfs law in internet traffic
