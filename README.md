# rude
输入url到浏览器显示页面的过程

接收url -> 分配http线程 -> dns查询得到IP -> 发送TCP/IP请求 -> 接收服务端信息 -> 解析html，构建dom树 -> 解析css，构建css规则树 -> 合并dom树和css规则树，生成render树 -> 布局render树 -> 绘制render树 -> GPU显示

1，解析URL：

    url解析：
    
        1，protocol，协议头，例如http，ftp，https
        
        2，host，主机域名或IP地址
        
        3，port，端口号
        
        4，path，目录路径
        
        5，query，查询参数
        
        6，fragment，#后的哈希值
        
2，每次网络请求都需要开辟独立的线程

3，DNS查询得到IP：

    优先级：浏览器缓存 -> 本机缓存 -> host -> 向dns域名服务器查询
   
4，TCP/IP请求

   http本质是TCP/IP请求，浏览器对同一域名下的tcp连接有个数限制
   
    三次握手：
   
       1，客户端给服务器发送确认是当前服务器
       
       2，服务器给客户端回应，确认是要访问的服务器
       
       3，客户端回应，我是客户端
       
    四次挥手：
    
       1，发起者：关闭主动传输信息通道，只能接受信息
       
       2，接收者：收到通道关闭的信息
       
       3，接收者：也关闭主动传输信息通道
       
       4，发起者：接收到信息，关闭通道，通信结束
     
   get和post区别
   
      get和post本质上都是tcp/IP
      
      get会产生一个数据包，post产生两个
      
      get请求，浏览器会把 header 和 data 一起发送
      
      post请求，浏览器先发送 header ，服务器响应 100continue ，浏览器在发送 data （更安全） 
      
5，http报文结构

    通用头部：
    
      Request Url：请求的web服务器地址
      
      Request Methods: 请求方式
      
      Status Code: 状态码
      
      Remote Address: 请求的远程服务器地址（IP地址）
      
      Referrer Policy： 用于过滤 Referrer 报头内容，目前是一个候选标准，不过已经有部分浏览器支持该标准
      
    请求头部：
      
      Accept: 接收类型，表示浏览器支持的MIME类型（对标服务端返回的Content-Type）
      
      Accept-Encoding：浏览器支持的压缩类型,如gzip等,超出类型不能接收
      
      Content-Type：客户端发送出去实体内容的类型
      
      Cache-Control: 指定请求和响应遵循的缓存机制，如no-cache
      
      If-Modified-Since：对应服务端的Last-Modified，用来匹配看文件是否变动，只能精确到1s之内，http1.0中
      
      Expires：缓存控制，在这个时间内不会请求，直接使用缓存，http1.0，而且是服务端时间
      
      Max-age：代表资源在本地缓存多少秒，有效时间内不会请求，而是使用缓存，http1.1中
      
      If-None-Match：对应服务端的ETag，用来匹配文件内容是否改变（非常精确），http1.1中
      
      Cookie：有cookie并且同域访问时会自动带上
      
      Connection：当浏览器与服务器通信时对于长连接如何进行处理,如keep-alive
      
      Host：请求的服务器URL
      
      Origin：最初的请求是从哪里发起的（只会精确到端口）,Origin比Referer更尊重隐私
      
      Referer：该页面的来源URL(适用于所有类型的请求，会精确到详细页面地址，csrf拦截常用到这个字段)
      
      User-Agent：用户客户端的一些必要信息，如UA头部等
