---
layout: post
title: 理解Nginx里的cgi，fastcgi，php-fpm 之间的关系
description: "通俗理解Nginx里的cgi，fastcgi，php-fpm"
category: nginx
avatarimg: 
tags: [nginx, cgi, fastcgi]
duoshuo: true
---
本来是在研究docker下nginx配置php，后来就思考这玩意到底是咋运转的，捣鼓nginx运行原理，于是打开了知识盲区的海洋。 
不做搬运工了，以下链接让我认识了cgi与fastcgi：
[CGI详解（原理，配置及访问）](https://blog.csdn.net/liunian_siyu/article/details/60964966)  
[怎么通俗易懂的去理解CGI](https://blog.csdn.net/dzyweer/article/details/79782663)  
其中第一篇博客也是博主转发了，不知道几手了，如果上边两篇还是不能理解，来点通俗的：
[如何通俗地解释 CGI、FastCGI、php-fpm 之间的关系？](https://blog.csdn.net/linuxheik/article/details/52039220)  
然后牵扯到apache的mod_php了，nginx与apache的差异，apache无论遇到什么请求，就像厨师一样，一见到有顾客来就电火，哪怕是顾客点的是凉菜，加载了php解释器，浪费类内存，解释器又是什么呢，参考：  
[超赞！编译器和解释器的异同，瞬间明白了](https://blog.csdn.net/zp357252539/article/details/78660131)  


nginx里有master和work进程概念参考：
[SAPI,CGI,Fastcgi,php-fpm的一些知识(个人见解) ](https://github.com/littlespark/blog/issues/7)    

最后你想知道php的执行过程，参考
[深入理解PHP代码的执行的过程](https://blog.csdn.net/risingsun001/article/details/22888861)  
[概念了解：CGI，FastCGI，PHP-CGI与PHP-FPM](https://my.oschina.net/junn/blog/280799)  

其他参考：
 
从 FPM 接收到请求，到处理完毕，其具体的流程如下：

FPM 的 master 进程接收到请求
master 进程根据配置指派特定的 worker 进程进行请求处理，如果没有可用进程，返回错误，这也是我们配合 Nginx 遇到502错误比较多的原因。
worker 进程处理请求，如果超时，返回504错误
请求处理结束，返回结果
FPM 从接收到处理请求的流程就是这样了，那么 Nginx 又是如何发送请求给 fpm 的呢？这就需要从 Nginx 层面来说明了。

我们知道，Nginx 不仅仅是一个 Web 服务器，也是一个功能强大的 Proxy 服务器，除了进行 http 请求的代理，也可以进行许多其他协议请求的代理，  
包括本文与 fpm 相关的 fastcgi 协议。为了能够使 Nginx 理解 fastcgi 协议，Nginx 提供了 fastcgi 模块来将 http 请求映射为对应的 fastcgi 请求。
[深入理解PHP之：Nginx 与 FPM 的工作机制](https://zhuanlan.zhihu.com/p/20694204)

