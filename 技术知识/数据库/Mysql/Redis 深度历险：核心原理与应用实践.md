# Redis 深度历险：核心原理与应用实践
## Redis 深度历险：核心原理与应用实践

钱文品 [ImportNew]()  
Redis 是如今互联网技术架构中，使用最广泛的缓存。支持 **复杂的数据结构** ，支持 **持久化** ，支持 **主从集群** ，支持 **高可用** ，支持 **较大的value存储**...

同时， Redis 也是中高级后端工程师技术面试中，面试官最喜欢问的问题之一。特别是那些优秀的、竞争激烈的大型互联网公司（比如 Twitter、新浪微博、阿里云、腾讯云、淘宝、知乎等），通常要求面试者不仅仅掌握 Redis 基础使用，更要求深层理解 Redis 内部实现的细节原理。毫不夸张地说，能把 Redis 的知识点全部吃透，你的半只脚就已经踏进心仪大公司的技术研发部。

然而，绝大部分开发者只会拿 Redis 做数据缓存，使用最简单的 get/set 方法，除此之外几乎一片茫然，对 Redis 内部实现的细节原理知之甚少。例如：

* 有同学知道 Redis 的 **分布式锁** ，但完全不清楚其内部实现机制

* 有同学知道 Redis 是 **单线程结构** ，但完全不理解 Redis 缘何单线程还可以支持高并发

* 有同学知道 Redis 支持 **主从** ，但完全不晓得内部的实现机制

* ...

于是，一位有着十年经验的互联网分布式高并发技术老兵，掌阅的后端专家 **钱文品** (老钱 Github 地址: github.com/pyloque），撰写了一本小册子，梳理了自己多年 Redis 实践经验，以帮助更多工程师更快，更深入的了解 Redis 。

![](Redis%20%E6%B7%B1%E5%BA%A6%E5%8E%86%E9%99%A9%EF%BC%9A%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/640.jpg)

老钱在掌阅维护着上千个 Redis 实例的集群，他在 Redis  **持久化，缓存，消息队列** 的各类实战经验，都毫无保留的凝聚到了这本小册子里。

小册分为五大部分：

**一、Redis应用**
* 分布式锁、延时队列、位图、HyperLogLog、布隆过滤器、限流、GeoHash、Scan

**二、Redis原理**

* IO现场模型、通信协议、持久化、管道、事务、PubSub、对象压缩、主从同步

**三、Redis集群**

* Sentinel、Codis、Cluster

**四、Reids拓展知识**

* Stream、Info命令、分布式锁、过期策略、LRU、懒删除、Jedis

**五、Redis源码剖析**

* 字符串、字典、压缩列表、快速列表、跳跃列表、紧凑列表、基数树

![](Redis%20%E6%B7%B1%E5%BA%A6%E5%8E%86%E9%99%A9%EF%BC%9A%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/640.jpg)

这本小册，是迄今为止，对 Redis 理论结合实践讲解相当通俗且全面 的资料。而且图文并茂，直观易懂，下面的动画大家感受一下。

![](Redis%20%E6%B7%B1%E5%BA%A6%E5%8E%86%E9%99%A9%EF%BC%9A%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/640.png)

![](Redis%20%E6%B7%B1%E5%BA%A6%E5%8E%86%E9%99%A9%EF%BC%9A%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/640.gif)
有没有感受到小册的诚意？

该小册，对ImportNew的粉丝 **半价** ， **限时一周** 。

目前小册有一些章节免费，欢迎 **扫码** 阅读。

![](Redis%20%E6%B7%B1%E5%BA%A6%E5%8E%86%E9%99%A9%EF%BC%9A%E6%A0%B8%E5%BF%83%E5%8E%9F%E7%90%86%E4%B8%8E%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/640.png)
**扫码** 免费阅读部分章节

 [阅读原文](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481647&amp;idx=1&amp;sn=b5971cde4eb57953fef3a9d7fc53f6a2&amp;chksm=bd250a508a52834609e66e05e057d6749a9b134020723a02b2b8a4f70dd77e6951f22618c8e4&amp;mpshare=1&amp;scene=1&amp;srcid=0904qLKki2BNogIHrGNKvLNj##)

http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651481647&idx=1&sn=b5971cde4eb57953fef3a9d7fc53f6a2&chksm=bd250a508a52834609e66e05e057d6749a9b134020723a02b2b8a4f70dd77e6951f22618c8e4&mpshare=1&scene=1&srcid=0904qLKki2BNogIHrGNKvLNj#rd