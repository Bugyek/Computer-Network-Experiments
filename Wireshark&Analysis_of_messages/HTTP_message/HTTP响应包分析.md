# HTTP响应包分析

## 响应数据

```html
<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>©2017 Baidu <a href=http://www.baidu.com/duty/>使用百度前必读</a>  <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a> 京ICP证030173号  <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>
```

响应数据总长度2381字节。

## 应用层 

应用层为响应数据封装HTTP首部后，组成HTTP响应报文，发往传输层。

HTTP首部为

```
    状态行：
    HTTP/1.1 200 OK\r\n                                //HTTP的版本为1.1，状态码为200，解释状态码的简单短语OK表示成功
    首部行：
    Accept-Ranges: bytes\r\n                           //表示可用于定义范围的单位。在这里范围请求的单位是 bytes （字节）
    Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform\r\n    //通过指定指令来实现缓存机制,指明哪些地方可以缓存返回的数据
                                                                                      //private表明响应只能被单个用户缓存，不能作为共享缓存
                                                                                      //no-cache禁止缓存
                                                                                      //no-transform不得对资源进行转换或转变
    Connection: keep-alive\r\n           //说明当前的事务完成后服务器不关闭网络连接，即持久HTTP连接
    Content-Length: 2381\r\n                           //指明发送给接收方的消息主体的大小，只有当浏览器使用持久HTTP连接时才需要这个数据
    Content-Type: text/html\r\n                        //告诉客户端实际返回的内容的内容类型属于text/html类型
    Date: Thu, 26 Nov 2020 07:24:09 GMT\r\n            //报文创建的日期和时间。(GMT格林尼治标准时间)
    Etag: "588604c8-94d"\r\n                           //资源的特定版本的标识符,用于标示URL对象是否改变
    Last-Modified: Mon, 23 Jan 2017 13:27:36 GMT\r\n   //文档的最后改动时间
    Pragma: no-cache\r\n                               //与 Cache-Control: no-cache 效果一致。强制要求缓存服务器在返回缓存的版本之前将请求提交到源头服务器进行验证。
                                                       //它用来向后兼容只支持 HTTP/1.0 协议的缓存服务器，那时候 HTTP/1.1 协议中的 Cache-Control 还没有出来。
    Server: bfe/1.0.8.18\r\n                           //服务器名字
    Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/\r\n             //设置和页面关联的Cookie
    \r\n                                               //空行
```

HTTP首部长度为400字节，组成的HTTP响应数据总长度为2781字节。

客户端与服务端的交互过程

  >用户在点击鼠标链接某个万维网文档时，HTTP协议首先要和服务器建立tcp链接,这会用三报文握手.\
  >第一次握手万维网客户向万维网服务器发出链接请求报文段\
  >第二次握手服务器向客户发送确认报文段。至此经过一个RTT时间\
  >第三次客户把HTTP请求报文发送给服务器\
  >服务器收到HTTP请求报文后，把请求的文档作为响应报文返回给客户。至此又经过一个RTT时间
 
## 传输层

传输层为HTTP响应数据封装TCP首部，组成TCP分组数据，发往网络层。

TCP首部为

```
    Source Port: 80                                                 //源端口
    Destination Port: 47516											//目的端口
    Sequence number: 1    (relative sequence number)				//序号，表明本报文段所发送的数据的第一个字节序号
    Sequence number (raw): 1686649527
    [Next sequence number: 2782    (relative sequence number)]		
    Acknowledgment number: 78    (relative ack number)				//确认号，表明期望收到的下一个报文段的第一个字节的序号
    Acknowledgment number (raw): 2346178793
    0101 .... = Header Length: 20 bytes (5)                         //数据偏移，也就是TCP首部的长度
    Flags: 0x018 (PSH, ACK)
        000. .... .... = Reserved: Not set							//保留
        ...0 .... .... = Nonce: Not set                             //保留
        .... 0... .... = Congestion Window Reduced (CWR): Not set   //保留
        .... .0.. .... = ECN-Echo: Not set                          //保留
        .... ..0. .... = Urgent: Not set                            //紧急URG，URG=1时表示紧急指针字段有效
        .... ...1 .... = Acknowledgment: Set                        //确认ACK，ACK=1时确认号字段才有效
        .... .... 1... = Push: Set                                  //推送PSH，PSH=1的报文段会尽快交付接收进程
        .... .... .0.. = Reset: Not set                             //复位RST，RST=1时表示需要重新建立连接
        .... .... ..0. = Syn: Not set                               //同步SYN，在连接时用来同步序号
        .... .... ...0 = Fin: Not set                               //终止FIN，FYN=1时表明要求释放允许连接
    Window size value: 64240                                        //接收窗口的大小，指明现在运行对方发送的数据量
    Checksum: 0xd76a            //校验和
    Urgent pointer: 0                                               //紧急指针，仅在URG=1时有意义
```

TCP首部长度为20字节，组成的TCP分组数据总长度为2801字节。

## 网络层

网络层为TCP分组数据封装IP首部，组成IP数据报，发往数据链路层。

IP首部为

```
    0100 .... = Version: 4                                           //IP协议的版本
    .... 0101 = Header Length: 20 bytes (5)                          //首部长度
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)    //区分服务
        0000 00.. = Differentiated Services Codepoint: Default (0)
        .... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0)
    Total Length: 2821                                               //数据报总长度
    Identification: 0xfef8 (65272)                                   //标识，使得相同标识字段的数据报片能正确的重装为原来的数据报
    Flags: 0x0000                                                    //标志
        0... .... .... .... = Reserved bit: Not set                  //保留位
        .0.. .... .... .... = Don't fragment: Not set                //DF=0表示允许分片，DF=1表示不能分片
        ..0. .... .... .... = More fragments: Not set                //MF=0表示这是数据报片中的最后一片，MF=1表示后面还有分片
    Fragment offset: 0                                               //片偏移，表明该数据报片在在原分组中的相对位置
    Time to live: 128                                                //生存时间TTL，TTL=0时路由器会丢弃该数据报
    Protocol: TCP (6)                                                //此包内封装的上层协议为TCP
    Header checksum: 0x6487                                          //首部校验和
    Source: 220.181.38.149                                           //源IP地址
    Destination: 192.168.8.128                                       //目的IP地址
```

IP首部长度为20字节，组成的IP数据报总长度为2821字节。

## 数据链路层

数据链路层为IP数据报封装以太网首部，组成以太网帧，发往物理层。

以太网首部为

```
    Destination: VMware_e5:f4:d8 (00:0c:29:e5:f4:d8)                 //目的MAC地址
    Source: VMware_fe:f8:43 (00:50:56:fe:f8:43)                      //源MAC地址
    Type: IPv4 (0x0800)                                              //类型
```

以太网首部长度为14字节，组成的以太网帧总长度为2835字节。
