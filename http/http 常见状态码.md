# 101Switch Protocol

升级协议，如从 http 到 ws

# 200 Ok

资源请求成功，会返回内容

# 201Created

资源创建成功，多用于 post 请求

# 204 No Content

请求成功，但不会返回 `body`，一般用于 OPTIONS / DELETE 请求

# 206 Partial Content

当请求资源较大时，会进行分片传输，例如媒体数据。

在 B 站看视频时，会有很多 206 状态码的请求。

# 301Moved Permanently

永久重定向

当 http 转向 https 时，有时会使用301，例如用 http 协议请求 B站

该操作比较危险，需要谨慎操作：如果设置了 301，但是一段时间后又想取消，但是浏览器中已经有了缓存，还是会重定向。

# 302 Found

临时重定向

当 http 转向 https 时，有时会使用302

```
$ curl www.zhihu.com -vvv
*   Trying 180.153.93.114:80...
* Connected to www.zhihu.com (180.153.93.114) port 80 (#0)
> GET / HTTP/1.1
> Host: www.zhihu.com
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 302 Found
< Location: https://www.zhihu.com/
```

# 304 Not Modified

资源已被缓存，与之相关的响应头有

* ETag
* last-modified / if-modified-since

# 307 Temporary Redirect

302 会在重定向的时候改变 method: 把 POST 改成 GET，于是有了 307。

307 也是一种临时重定向，在重定向时不会改变 method

# 400 Bad Request

当服务器无法解析客户端传递的参数时，会返回 400 状态码

比如当 Content-Type: JSON 时，服务器解析 JSON 失败会返回 400

# 401 Unauthorized

当没有权限的用户请求需要带有权限的资源时，会返回 401

# 403 

我就是不想让你访问，不管你的权限凭证是否正确！

#  404

未找到该资源，可能路径没拼写对

# 405 Method Not Allowed

请求方式错误，一个接口接收的请求方式是 post，但客户端用的是 get 方法

# 413 Payload Too Large

不要给我扔这么大的 Body，我处理不过来

# 500 Internal Serveren Error

服务器内部错误，很有可能是应用层未捕获错误而导致整个服务挂掉

# 502 Bad Gateway

被用于充当代理或网管的服务器，Nginx 上常见，表示接收到远端服务器错误的响应

# 503 Service Unavailable

由于大量流量造成服务忙，稍等一下说不定就能用了

# 504 Gateway Timeout

也用于充当代理或网关的服务器。网关超时，远端服务器迟迟未响应

502 和 504 一般都用于代理服务器或网管

502 是网关错误，远端服务器出问题了。

504 是网关超时，可能是远端服务器过载，忙不过来

