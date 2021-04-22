# cookie_session_token

## 基于cookie/session的认证方案
1. cookie 工作原理
    - http是一种无状态协议，服务器单从网络上无法知道客户的身份
    - cookie实际上是一段文本信息，客户端请求服务端，如果服务器需要记录用户状态，就给客户端颁发一个cookie
    - 客户端会把cookie保存起来，当再次请求时，把cookie发送给服务器，服务器检查cookie，来辨认用户状态
    - cookie具有不可跨域名性

2. seession
    - session是一种记录客户状态的机制，不同的是cookie保存在客户端浏览器中，而session保存在服务器上

    - 客户端访问服务器时，服务器把客户端信息以某种形式记录在服务器上，这就是session
    - 客户端再次访问时，只需从该seesion中查找客户状态就可以了

3. cookie/session
    - seesion有一个缺点：如果web服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失
    - cookie数据存放在浏览器上，session数据放在服务器上
    - cookie不太安全，session相对于cookie来说更安全
    - session会在一定时间内保存在服务器上，当访问量增多，会占用服务器性能
    - 服务端session的实现对客户端的cookie有依赖关系
        - 服务端生成的session时会发给客户端一个id
        - 客户端保存id的容器就是cookie
        - 当浏览器禁用cookie时，服务端session也无法正常使用


## 基于token的认证方式
1. token是多用户处理认证的最佳方式
    - 无状态，可扩展
    - 支持移动设备
    - 跨程序调用
    - 安全

2. 基于token的验证原理
    - 用户通过用户名，密码发送请求
    - 服务端验证，并返回一个带签名的token给客户端
    - 客户端存储token，并每次访问都携带token到服务端
    - 服务端验证token


## 项目中使用token
1. 前端使用用户名密码请求首次登陆
2. 服务端验证用户名，密码
3. 验证成功后，服务端会根据用户id，用户名，定义好的密钥，过期时间生成一个token，返回给前端
4. 前端接收到token，存储在cookie或者localstorage里
5. 前端每次路由跳转，判断localstorage中有无token，没有则跳转到登陆页
6. 每次发送的请求中也要在请求头里携带token
7. 服务端验证token，成功则返回数据