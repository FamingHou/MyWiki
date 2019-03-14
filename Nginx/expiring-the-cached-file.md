You can also bypass/re-cache on a file by file basis using 

```console
proxy_cache_bypass $http_secret_header;
```
you can return this header to see if you got it from the cache (will return 'HIT') or from the content server (will return 'BYPASS').

```console
add_header X-Cache-Status $upstream_cache_status;
```

To expire/refresh the cached file, use curl or any rest client to make a request to the cached page. 

```console
curl http://abcdomain.com/mypage.html -s -I -H "secret-header:true"
```

This will return a fresh copy of the item and it will also replace what's in cache. 
