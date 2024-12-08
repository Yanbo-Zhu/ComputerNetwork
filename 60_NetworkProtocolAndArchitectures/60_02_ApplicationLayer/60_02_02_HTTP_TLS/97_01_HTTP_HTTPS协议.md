
# 1 URL 


![](image/Pasted%20image%2020241031221639.png)



> stateless: server maintains no information about past client requests 

# 2 HTTP use TCP 


![](../../60_01_Intro/image/Pasted%20image%2020241021073237.png)



![](../../60_01_Intro/image/Pasted%20image%2020241021073429.png)



# 3 三次握手 



![](../../60_01_Intro/image/Pasted%20image%2020241021073527.png)

![](../../60_01_Intro/image/Pasted%20image%2020241021073534.png)

# 4 HTTP Request 


![](image/Pasted%20image%2020241031223127.png)


![](image/Pasted%20image%2020241031223136.png)


## 4.1 HTTP Request message format

![](../../60_01_Intro/image/Pasted%20image%2020241021073735.png)



![](../../60_01_Intro/image/Pasted%20image%2020241021073742.png)


## 4.2 HTTP Request Method 


![](image/Pasted%20image%2020241208203219.png)


POST method:
• web page often includes form input
• user input sent from client to server in entity body of HTTP POST request message

GET method (for sending data to server):
• include user data in URL field of HTTP GET request message (following a '?'):
www. some site . com/animalsearch?monkeys &banana

HEAD method:
requests headers (only) that would be returned if specified URL were requested with an HTTP GET method.

PUT method:
• uploads new file (object) to server
• completely replaces file that exists at specified URL with content in entity body of POST HTTP request message


- `GET`：请求资源数据，通常只读。
- `POST`：提交数据，创建资源或执行某些处理。
- `HEAD`：类似 GET，但只获取响应头。
- `PUT`：更新或创建资源，发送完整的资源信息。


- **GET**:
    
    - 用于从服务器获取数据。
    - 请求的数据通过 URL 传递（查询参数或路径）。
    - 通常是幂等的（多次执行同一个请求，服务器返回的结果应该相同）。
    - 不会对服务器上的数据造成修改，仅用于请求数据。
    - 响应中会包含完整的数据（比如 HTML 页面、JSON 等）。
    
    **例子**: 请求获取用户信息  
    `GET /users/123`
    
- **POST**:
    
    - 用于向服务器提交数据。
    - 数据通常在请求体中传递（不像 GET 使用 URL 参数）。
    - 一般用于创建或提交新的资源或处理表单数据。
    - 非幂等（同样的 POST 请求可以创建多个相同的资源）。
    - 响应可能会返回操作结果、生成的资源 ID，或其他状态信息。
    
    **例子**: 提交一个新用户  
    `POST /users`  
    请求体: `{ "name": "John", "age": 30 }`
    
- **HEAD**:
    
    - 类似于 GET，但只返回 HTTP 响应头而不返回响应体。
    - 用于检查资源的元信息，比如验证资源是否存在，检查内容类型、大小等。
    - 常用于在下载或获取数据前检查资源的状态。
    
    **例子**: 检查用户信息是否存在  
    `HEAD /users/123`
    
- **PUT**:
    
    - 用于更新服务器上的资源（或者如果资源不存在，创建资源）。
    - 数据通常在请求体中传递，表示完整的资源。
    - 幂等的（多次执行同一个 PUT 请求，结果不会改变服务器的状态）。
    - 通常用于替换现有资源的全部内容。
    
    **例子**: 更新用户信息  
    `PUT /users/123`  
    请求体: `{ "name": "John", "age": 31 }`



# 5 Header Field 


![](image/Pasted%20image%2020241208203522.png)




# 6 Repsone Code 

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#informational_responses) (`100` – `199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200` – `299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300` – `399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400` – `499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500` – `599`)

![](image/Pasted%20image%2020241208203205.png)

![](image/Pasted%20image%2020241031223214.png)

# 7 Cookies 

为什么需要 Cookies: HTTP GET/response interaction is stateless
> Web sites and client browser use cookies to maintain some state between transactions


Client-side state maintenance
- Client stores small (?) state on behalf of server
- Client sends state in future requests to the server
Can provide authentication


four components:
1) cookie header line of HTTP response message
2) cookie header line in next HTTP request message
3) cookie file kept on user’s host, managed by user’s browser
4) back-end database at Web site

![](image/Pasted%20image%2020241021103143.png)


What cookies can be used for: Cookies 的用途 
▪ authorization
▪ shopping carts
▪ recommendations
▪ user session state (Web e-mail)



![](image/Pasted%20image%2020241031224142.png)




# 8 Web Caches (Proxy Servers)

browser sends all HTTP requests to cache
- f object in cache: cache returns object to client
- else cache requests object from origin server, caches received object, then returns object to client

Why Web caching?
- reduce response time for client request, cache is closer to client
- reduce traffic on an institution’s access link
- Internet is dense with caches


Conditional GET through Cache 
Cachce 中有  up-to-date cached version 就不需要再去  server 端找最新的数据了 

![](image/Pasted%20image%2020241021103343.png)



# 9 HTTP/HTTPS协议

![](image/Pasted%20image%2020240216165526.png)

![](image/Pasted%20image%2020240216170154.png)


# 10 HTTP is Stateless

![](image/Pasted%20image%2020241031223605.png)


Stateless protocol (e.g., GET, PUT, DELETE)
- Each request-response exchange treated independently
- Servers not required to retain state

Still, requests are not idempotent
- Consider GET then PUT then GET vs. GET then DELETE then GET

This is good as it improves scalability on the server-side
- Don’t have to retain info across requests
- Can handle higher rate of requests
- Order of requests doesn’t matter


This is also bad as some applications need persistent state
- Need to uniquely identify user or store temporary info
- e.g., shopping cart, user preferences/profiles, usage tracking,
