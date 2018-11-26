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

3，DNS查询得到IP

   优先级：
       浏览器缓存 -> 本机缓存 -> host -> 向dns域名服务器查询
