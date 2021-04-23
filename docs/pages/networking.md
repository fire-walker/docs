## Commands

``` bash
# all port usage
netstat -pnltu

# specific port usage
lsof -i :9000
netstat -ltnp | grep -w ':80'
```

## Endpoint Info

Useful when trying to verify if headers are acting as they're supposed to when building an API

``` bash
curl -is http://google.com
```

```
HTTP/2 301 
location: https://www.google.com/
content-type: text/html; charset=UTF-8
date: Fri, 23 Apr 2021 15:36:48 GMT
expires: Sun, 23 May 2021 15:36:48 GMT
cache-control: public, max-age=2592000
server: gws
content-length: 220
x-xss-protection: 0
x-frame-options: SAMEORIGIN
```