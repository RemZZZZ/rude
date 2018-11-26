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
