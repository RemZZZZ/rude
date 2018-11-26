# rude
输入url到浏览器显示页面的过程

接收url -> 分配http线程 -> dns查询得到IP -> 发送TCP/IP请求 -> 接收服务端信息 -> 解析html，构建dom树 -> 解析css，构建css规则树 -> 合并dom树和css规则树，生成render树 -> 布局render树 -> 绘制render树 -> GPU显示

解析URL：

URL主要组成部分：

1，protocol，协议头，例如http，ftp，https
