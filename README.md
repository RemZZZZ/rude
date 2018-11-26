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
      
  响应头部：
  
      Access-Control-Allow-Headers: 服务器端允许的请求Headers
      
      Access-Control-Allow-Methods: 服务器端允许的请求方法
      
      Access-Control-Allow-Origin: 服务器端允许的请求Origin头部（譬如为*）
      
      Content-Type：服务端返回的实体内容的类型
      
      Date：数据从服务器发送的时间
      
      Cache-Control：告诉浏览器或其他客户，什么环境可以安全的缓存文档
      
      Last-Modified：请求资源的最后修改时间
      
      Expires：应该在什么时候认为文档已经过期,从而不再缓存它
      
      Max-age：客户端的本地资源应该缓存多少秒，开启了Cache-Control后有效
      
      ETag：请求变量的实体标签的当前值
      
      Set-Cookie：设置和页面关联的cookie，服务器通过这个头部把cookie传给客户端
      
      Keep-Alive：如果客户端有keep-alive，服务端也会有响应（如timeout=38）
      
      Server：服务器的一些相关信息
      
6，HTML解析，构建DOM

   Bytes - characters - tokens - nodes - dom
   
      conversion转换：浏览器将获得的HTML内容（Bytes）基于编码转换成字符
      
      tokenizing分词：浏览器按照HTML规范将字符转换为不同标记的 token，每个 token 都有自己独特的含义以及规则集
      
      Lexing词法分析：分词的结果是得到一堆的 token ，此时把他们转换为对象，这些对象分别定义他们的属性和规则
      
      DOM构建：因为HTML标记定义的就是不同标签之间的关系，这个关系就像是一个树形结构一样
      
7，生成CSS规则树，同dom树

8，构建render树

       回流:reflow,一般意味着元素的内容、结构、位置或尺寸发生了变化，需要重新计算样式和渲染树
       
       重绘：repaint，意味着元素发生的改变只是影响了元素的一些外观之类的时候（例如，背景色，边框颜色，文字颜色等），此时只需要应用新样式绘制这个元素      就可以了
