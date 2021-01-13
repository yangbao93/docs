# ActiveMQ学习
# ImportNew

（点击上方公众号，可快速关注）

> 来源：cyfonly ，  

> www.cnblogs.com/cyfonly/p/6380860.html  

过了个春节，回到公司的成小胖变成了成大胖。但是你们千万别以为他那个大肚子里面装的都是肥肉，里面的墨水也多了不少嘞，毕竟成小胖利用春节的半个月时间专心学习并研究了 ActiveMQ，嘿嘿……

这不，为了检验下自己的学习成果，上班的第一天成小胖就去找架构师老王交流 ActiveMQ 相关的知识，还顺便向老王讨了个红包，可把成小胖给高兴坏了。

“ **来，根据你的了解说下 ActiveMQ 是什么** 。”

“这个简单，ActiveMQ 是一个 MOM，具体来说是一个实现了 JMS 规范的系统间远程通信的消息代理。它……”

“ **等等，先解释下什么是 MOM** 。”

“好。MOM 就是面向消息中间件（Message-oriented middleware），是用于以分布式应用或系统中的异步、松耦合、可靠、可扩展和安全通信的一类软件。MOM 的总体思想是它作为消息发送器和消息接收器之间的消息中介,这种中介提供了一个全新水平的松耦合。”

“ **JMS呢** ？”

成小胖是个追求极致的人，为了解释得更通俗易懂，索性搬来一块白板边画边说。

“JMS 叫做 Java 消息服务（Java Message Service）,是 Java 平台上有关面向 MOM 的技术规范，旨在通过提供标准的产生、发送、接收和处理消息的 API 简化企业应用的开发，类似于 JDBC 和关系型数据库通信方式的抽象。”

<a href='640.webp'>640.webp</a>

“ **嗯，很好。下面的这些概念你也需要特别理解下** ”：

* Provider：纯 Java 语言编写的 JMS 接口实现（比如 ActiveMQ 就是）

* Domains：消息传递方式，包括点对点（P2P）、发布/订阅（Pub/Sub）两种

* Connection factory：客户端使用连接工厂来创建与 JMS provider 的连接

* Destination：消息被寻址、发送以及接收的对象

“ **你来说说这其中 P2P 和 Pub/Sub 的区别吧** ”，老王给成小胖抛出了一个问题。

成小胖可不是吃素的，毕竟要是吃素的话他也吃不到这么胖……这些基本概念对他来说都是小事一桩：

P2P （点对点）消息域使用 queue 作为 Destination，消息可以被同步或异步的发送和接收，每个消息只会给一个 Consumer 传送一次。

Consumer 可以使用 MessageConsumer.receive() 同步地接收消息，也可以通过使用MessageConsumer.setMessageListener() 注册一个 MessageListener 实现异步接收。

多个 Consumer 可以注册到同一个 queue 上，但一个消息只能被一个 Consumer 所接收，然后由该 Consumer 来确认消息。并且在这种情况下，Provider 对所有注册的 Consumer 以轮询的方式发送消息。

<a href='640.webp'>640.webp</a>

Pub/Sub（发布/订阅，Publish/Subscribe）消息域使用 topic 作为 Destination，发布者向 topic 发送消息，订阅者注册接收来自 topic 的消息。发送到 topic 的任何消息都将自动传递给所有订阅者。接收方式（同步和异步）与 P2P 域相同。

除非显式指定，否则 topic 不会为订阅者保留消息。当然，这可以通过持久化（Durable）订阅来实现消息的保存。这种情况下，当订阅者与 Provider 断开时，Provider 会为它存储消息。当持久化订阅者重新连接时，将会受到所有的断连期间未消费的消息。

<a href='640.webp'>640.webp</a>

“嗯，总结的很不错，上面的这些知识是学习 ActiveMQ 的理论基础，是必须要掌握的。”

“既然 JMS 是一个通用的规范，那么使用它创建应用程序肯定也有一个通用的步骤吧？”老王追问道。

“有的有的。要不您来说说这个通用步骤？就当我考考您，哈哈！”成小胖故作聪明，自以为老王作为架构师不会关注这些太具体的实现细节。

然而老王平日里亲力亲为，至今还常常撸代码，怎么会被这种小 case 所难倒？于是老王分分钟给出答案：

* 获取连接工厂

* 使用连接工厂创建连接

* 启动连接

* 从连接创建会话

* 获取 Destination

* 创建 Producer，或

1. 创建 Producer

2. 创建 message

* 创建 Consumer，或发送或接收message发送或接收 message

1. 创建 Consumer

2. 注册消息监听器（可选）

* 发送或接收 message

* 关闭资源（connection, session, producer, consumer 等)

“66666，厉害啊我的王哥！”成小胖的小聪明被老王击得粉碎！

“你嘴皮子耍够了吧，还是多动动手吧。现在你手写上面步骤对应的代码实现吧”，老王给了成小胖一个眼神，让他自己慢慢体会……

成小胖也不是省油的灯，马上擦干净白板，现场撸了起来（是撸代码，撸代码，撸代码，重要的事情说三遍）：

> public class JMSDemo {  

> ConnectionFactory connectionFactory;  

> Connection connection;  

> Session session;  

> Destination destination;  

> MessageProducer producer;  

> MessageConsumer consumer;  

> Message message;  

> boolean useTransaction = false;  

> try {  

> Context ctx = new InitialContext();  

> connectionFactory = (ConnectionFactory) ctx.lookup("ConnectionFactoryName");  

> //使用ActiveMQ时：connectionFactory = new ActiveMQConnectionFactory(user, password, getOptimizeBrokerUrl(broker));  

> connection = connectionFactory.createConnection();  

> connection.start();  

> session = connection.createSession(useTransaction, Session.AUTO_ACKNOWLEDGE);  

> destination = session.createQueue("TEST.QUEUE");  

> //生产者发送消息  

> producer = session.createProducer(destination);  

> message = session.createTextMessage("this is a test");  

> //消费者同步接收  

> consumer = session.createConsumer(destination);  

> message = (TextMessage) consumer.receive(1000);  

> System.out.println("Received message: " + message);  

> //消费者异步接收  

> consumer.setMessageListener(new MessageListener() {  

> @Override  

> public void onMessage(Message message) {  

> if (message != null) {  

> doMessageEvent(message);  

> }  

> }  

> });  

> } catch (JMSException e) {  

> ...  

> } finally {  

> producer.close();  

> session.close();  

> connection.close();  

> }  

> }  

老王满意的点点头：“还算不赖哈~ JMS 通用的规范咱们都聊完了，下面就来聊点 ActiveMQ 更具体点的东西咯。”

“好啊好啊。 **要不我先基于自己的学习讲讲 ActiveMQ 的存储，您看看我哪里讲的不对或者遗漏的，可好** ？”成小胖发挥了他一贯的积极主动的作风，当然内心里还是想得到老王的赞许。

“行，那就开始吧。”

ActiveMQ 在 queue 中存储 Message 时，采用先进先出顺序（FIFO）存储。同一时间一个消息被分派给单个消费者，且只有当 Message 被消费并确认时，它才能从存储中删除。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

对于持久化订阅者来说，每个消费者获得 Message 的副本。为了节省存储空间，Provider 仅存储消息的一个副本。持久化订阅者维护了指向下一个 Message 的指针，并将其副本分派给消费者。以这种方式实现消息存储，因为每个持久化订阅者可能以不同的速率消费 Message，或者它们可能不是全部同时运行。此外，因每个 Message 可能存在多个消费者，所以在它被成功地传递给所有持久化订阅者之前，不能从存储中删除。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

“ **很好，上面这段知识非常重要。其实我们可以通过表格来更清晰地展示** ”，老王补充道，并在白板上画了以下表格：

<a href='640.webp'>640.webp</a>

成小胖虽然对以上特性做过实践对比，但是并没有想到去画一个表格出来使对比更加清晰易懂。特别是当他看到老王随时就画出这个表格时便惊叹不已，大声喊道：“老王你太牛了，真是爱死你了！”

周围的同事听到后，都齐刷刷的往这边看过来。

此情此景，老王也不好意思了：“诶诶诶，说话注意哈，不要让人觉得我们在搞基。回归正题，你再说说 ActiveMQ 常用的存储方式吧。”

成小胖羞涩的点点头，迅速地回归原态，一五一十地说起来。

**1.KahaDB**

ActiveMQ 5.3 版本起的默认存储方式。KahaDB存储是一个基于文件的快速存储消息，设计目标是易于使用且尽可能快。它使用基于文件的消息数据库意味着没有第三方数据库的先决条件。

要启用 KahaDB 存储，需要在 activemq.xml 中进行以下配置：

> <broker brokerName="broker" persistent="true" useShutdownHook="false">  

> <persistenceAdapter>  

> <kahaDB directory="${activemq.data}/kahadb" journalMaxFileLength="16mb"/>  

> </persistenceAdapter>  

> </broker>  

**2.AMQ**

与 KahaDB 存储一样，AMQ存储使用户能够快速启动和运行，因为它不依赖于第三方数据库。AMQ 消息存储库是可靠持久性和高性能索引的事务日志组合，当消息吞吐量是应用程序的主要需求时，该存储是最佳选择。但因为它为每个索引使用两个分开的文件，并且每个 Destination 都有一个索引，所以当你打算在代理中使用数千个队列的时候，不应该使用它。

> <persistenceAdapter>  

> <amqPersistenceAdapter  

> directory="${activemq.data}/kahadb"  

> syncOnWrite="true"  

> indexPageSize="16kb"  

> indexMaxBinSize="100"  

> maxFileLength="10mb" />  

> </persistenceAdapter>  

**3.JDBC**

选择关系型数据库，通常的原因是企业已经具备了管理关系型数据的专长，但是它在性能上绝对不优于上述消息存储实现。事实是，许多企业使用关系数据库作为存储，是因为他们更愿意充分利用这些数据库资源。

> <beans>  

> <broker brokerName="test-broker" persistent="true" xmlns="http://activemq.apache.org/schema/core">  

> <persistenceAdapter>  

> <jdbcPersistenceAdapter dataSource="＃mysql-ds"/>  

> </persistenceAdapter>  

> </broker>  

> <bean id="mysql-ds" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  

> <property name="driverClassName" value="com.mysql.jdbc.Driver"/>  

> <property name="url" value="jdbc:mysql://localhost/activemq?relaxAutoCommit=true"/>  

> <property name="username" value="activemq"/>  

> <property name="password" value="activemq"/>  

> <property name="maxActive" value="200"/>  

> <property name="poolPreparedStatements" value="true"/>  

> </bean>  

> </beans>  

**4.内存存储**

内存消息存储器将所有持久消息保存在内存中。在仅存储有限数量 Message 的情况下，内存消息存储会很有用，因为 Message 通常会被快速消耗。在 activema.xml 中将 broker 元素上的 persistent 属性设置为 false 即可。

> <broker brokerName="test-broker" persistent="false" xmlns="http://activemq.apache.org/schema/core">  

> <transportConnectors>  

> <transportConnector uri="tcp://localhost:61635"/>  

> </transportConnectors>  

> </broker>  

老王听完后露出赞许的笑容：“连配置都能写的这么详细，看来你确实是做了不少功课，给你点个赞。”老王终究不会吝啬自己的赞美，他也明白这些赞美对成小胖意味着什么。

成小胖得到了老王的赞赏，心里也是吃了蜜一般。

没等成小胖说话，老王拿起笔走到白板前，说：“下面就根据我在工作中的经历，给你讲讲 ActiveMQ 的部署模式。”

**1.单例模式**

这个就不啰嗦了，略过。

**2.无共享主从模式**

这是最简单的 Provider 高可用性的方案，主从节点分别存储 Message。从节点需要配置为连接到主节点，并且需要特殊配置其状态。

所有消息命令（消息，确认，订阅，事务等）都从主节点复制到从节点，这种复制发生在主节点对其接收的任何命令生效之前。并且，当主节点收到持久消息，会等待从节点完成消息的处理（通常是持久化到存储），然后再自己完成消息的处理（如持久化到存储）后，再返回对 Producer 的回执。

从节点不启动任何传输，也不能接受任何客户端或网络连接，除非主节点失效。当主节点失效后，从节点自动成为主节点，并且开启传输并接受连接。这是，使用 failover 传输的客户端就会连接到该新主节点。

Broker 连接配置如下：

> failover://(tcp://masterhost:61616,tcp://slavehost:61616)?randomize=false  

但是，这种部署模式有一些限制，

* 主节点只会在从节点连接到主节点时复制其活动状态，因此当从节点没有连接上主节点之前，任何主节点处理的 Message 或者消息确认都会在主节点失效后丢失。不过你可以通过在主节点设置 waitForSlave 来避免，这样就强制主节点在没有任何一个从节点连接上的情况下接受连接。

* 就是主节点只能有一个从节点，并且从节点不允许再有其他从节点。

* 把正在运行的单例配置成无共享主从，或者配置新的从节点时，你都要停止当前服务，修改配置后再重启才能生效。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

在可以接受一些故障停机时间的情况下，可以使用该模式。

从节点配置：

> <services>  

> <masterConnector remoteURI="tcp://remotehost:62001" userName="Rob" password="Davies"/>  

> </services>  

此外，可以配置 shutdownOnMasterFailure 项，表示主节点失效后安全关闭，保证没有消息丢失，允许管理员维护一个新的从节点。

**3.共享存储主从模式**

允许多个代理共享存储，但任意时刻只有一个是活动的。这种情况下，当主节点失效时，无需人工干预来维护应用的完整性。另外一个好处就是没有从节点数的限制。

有两种细分模式：

**（1）基于数据库**

它会获取一个表上的排它锁，以确保没有其他 ActiveMQ 代理可以同时访问数据库。其他未获得锁的代理则处于轮询状态，就会被当做是从节点，不会开启传输也不会接受连接。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

**（2）基于文件系统**

需要获取分布式共享文件锁，linux 系统下推荐用 GFS2。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

看到这些干货，成小胖欣喜若狂一边听一边记，等老王讲完后他还没记完。而老王则趁机喝了杯铁观音润润嗓子。

在记录完老王所讲的部署模式后，成小胖也不好意思再让老王继续讲下去了，毕竟他知道老王常年加班腰间盘突出，不能长时间站着。

“王哥您坐着休息下，我再给您讲讲我所理解的 ActiveMQ 的网络连接，中不中？”

“中。没事儿，我身体好着呢~”老王知道成小胖担心他的腰，但他还是那个倔脾气。成小胖也不敢多耽误时间，立马开讲。

**1.代理网络**

支持将 ActiveMQ 消息代理链接到不同拓扑，这就是被人们熟知的代理网络。

ActiveMQ 网络使用存储和转发的概念，其中消息总是存储在本地代理中，然后通过网络转发到另一个代理。

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

当连接建立后，远程代理将把包含其所有持久和活动消费者目的地的信息传递给本地代理，本地代理根据信息决定远程代理感兴趣的 Message 并将它发送给远程代理。

如果希望网络是双向的，您可以使用网络连接器将远程代理配置为指向本地代理，或将网络连接器配置为双工，以便双向发送消息。

> <networkConnectors>  

> <networkConnector uri="static://(tcp://backoffice:61617)"  

> name="bridge"  

> duplex="true"  

> conduitSubscriptions="true"  

> decreaseNetworkConsumerPriority="false">  

> </networkConnector>  

> </networkConnectors>  

注意，配置的顺序很重要：

1.网络连接——需要在消息存储前建立好连接，对应 networkConnectors 元素

2.消息存储——需要在传输前配置好，对应 persistenceAdapter 元素

3.消息传输——最后配置，对应 transportConnectors 元素

**2.网络发现**

**（1）动态发现**

使用多播来支持网络动态发现。配置如下：

> <networkConnectors>  

> <networkConnector uri="multicast://default"/>  

> </networkConnectors>  

其中，multicast:// 中的默认名称表示该代理所属的组。因此使用此方式时，强烈推荐你使用一个独特的组名，避免你的代理连接到其他不相关代理。

**（2）静态发现**

静态发现接受代理 URI 列表，并将尝试按列表中确定的顺序连接到远程代理。

> <networkConnectors>  

> <networkConnector uri="static:(tcp://remote-master:61617,tcp://remote-slave:61617)"/>  

> </networkConnectors>  

相关配置如下：

* initialReconnectDelay：默认值1000，表示尝试连接前的时延。

* maxReconnectDelay：默认值30000，表示连接失败后到重新建立连接之间的时延，仅在 useExponentialBackOff 启用时生效。

* useExponentialBackOff：默认值 true，如果启用，表示每次失败后增加重建连接的时延。

* backOffMultiplier：默认值2，表示启用 useExponentialBackOff 后每次的时延增量需要注意的是，网络连接将始终尝试建立到远程代理的连接。

需要注意的是，网络连接将始终尝试建立到远程代理的连接。

**（3）多连接场景** 

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

当网络负载高时，使用多连接很有意义。但是你需要确保不会重复传递消息，这可以通过过滤器来实现。

> <networkConnectors>  

> <networkConnector uri="static://(tcp://remotehost:61617)"  

> name="queues_only"  

> duplex="true"  

> <excludedDestinations>  

> <topic physicalName=">"/>  

> </excludedDestinations>  

> </networkConnector>  

> <networkConnector uri="static://(tcp://remotehost:61617)"  

> name="topics_only"  

> duplex="true"  

> <excludedDestinations>  

> <queue physicalName=">"/>  

> </excludedDestinations>  

> </networkConnector>  

> </networkConnectors>  

讲完后成小胖如释重负，因为上面这些知识点虽然看起来很少，但他却花了很多时间看了很多英文资料，同时反复实践才理解透的呢。

在成小胖讲的这段时间，老王一直坐在转椅上，这会儿他的腰也舒服了很多。老王站起来拍了拍成小胖的肩膀：“这个知识点虽然理解起来有点晦涩，但是你解释得还是挺不错的。通过今天的交流，可以看出你对 ActiveMQ 的基础知识有了不错的掌握，今后呢还是要多加深入实践，这样才能使用它提供高质量的服务。”

老王的手机突然响了，是要去开会了：“今天就到这儿吧，我有个会议要参加。”

“好，谢谢王哥的耐心指导。希望你多注意身体。”

老王鬼魅的一笑：“嗯，放心吧，隔壁有对年轻人刚结婚，听Ta们说最近想要个小baby，所以我肯定会保养好自己的身体的。”

成小胖：“……”

“哈哈，开玩笑的！”

“……”

看完本文有收获？请转发分享给更多人

**关注「ImportNew」，提升Java技能**

![](ActiveMQ%E5%AD%A6%E4%B9%A0/image_1.png)

https://mmbiz.qpic.cn/mmbiz_png/eZzl4LXykQyqFic84bKUbKc42iaSicpzYAv3ibg3WJfu7pDctCSeVL8AdR7XoU7Fv5gAv9dlSRE8pnKRjpdFMF40DA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1