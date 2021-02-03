# spring boot 开发web service

## spring boot 开发web service

[Edit](http://maxiang.io/#/?provider=evernote&amp;guid=31c0ed4a-0fc4-48d6-8907-7ab2d7b2fd0f&amp;notebook=%E6%8A%80%E6%9C%AF%E7%9F%A5%E8%AF%86)

## spring boot 开发web service

#### _WSDL：Web Services Description Language （web 服务描述语言）_

#### _Soap：Simple Object Access Protocol （简单对象访问协议）_

### BigDecimal的使用

```text
bigdecimal 是高精度数据其使用可以通过    BigDecimal bd = new BigDecimal(String);    bg.add(new BigDecimal());//这样写bd永远是初始值，不会进行任何改变    bd = bd.add(new BigDecimal());//这样才是正确的写法
```

### mongoDB的使用

> **mongoDB的服务器地址为：59.110.19.71**  
> **账号密码为：root/dzpj@123**  
> _SecureCRTPortable连接到mongo数据库_  
> _使用如下命令进行操作_

```text
cd /usr/local/mongo/bin //进入mongo的位置./mongo //启动mongo//mongo的多条件查询db.generalPaymentFile.find({"payment.coCode":"1234"})
```

mongo数据库一般不需要设置授权，但这样存在风险。可以通过授权方式进行安全加密：

```text
use testdb.createUser( {   user: "nontaxbill",   pwd: "dzpj123",   roles: [      { role: "readWrite", db: "test" }   ] } )ps -ef | grep mongokill -9 pid./mongod --auth &这是进入test数据库时需要 db.auth('nontaxbill','dzpj123')//不进行授权会报错，授权后可以进行操作
```

在数据库授权之后spring boot中进行配置的更改。

```text
//原配置spring.data.mongodb.uri=mongodb://localhost:27017/test//更改为spring.data.mongodb.uri=mongodb://name:pass@localhost:27017/test
```

### FastDfs的使用

> **ClientGlobal的使用**
>
> _在线配置_  
> \` 不是很懂需要仔细研究

```text
//设置编码格式  ClientGlobal.setG_charset("UTF-8");  //设置连接超时时间（秒）  ClientGlobal.setG_connect_timeout(2);  //设置下载超时时间（秒）  ClientGlobal.setG_network_timeout(30);  //  ClientGlobal.setG_anti_steal_token(false);  //设置端口  ClientGlobal.setG_tracker_http_port(22122);  //不懂  InetSocketAddress[] trackerServers = new InetSocketAddress[1];  trackerServers[0] = new InetSocketAddress("192.168.52.99", 22122);  ClientGlobal.setG_tracker_group(new TrackerGroup(trackerServers));`
```

&gt;

> _本地文件配置_

```text
ClientGlobal.init("文件路径")；//例如"d://fastdfs.conf"//文件类容如下connect_timeout = 2network_timeout = 30charset = UTF-8tracker_server = 192.168.52.99:22122
```

> _进行链接：_

```text
TrackerClienttrackerClient = new TrackerClient();TrackerServer  trackerServer = trackerClient.getConnection();StorageClient  storageClient = new StorageClient(trackerServer, storageServer);
```

> 上传/下载

```text
storageClient.upload();storageClient.download();
```

> bean的配置

```text
1.文件存储类型的配置：(在文件上传时会出现文件解析错误)    application.properties: fdfs_configPath=d:/EinvoiceConf/Invoiceclient/fdfs_client.conf，在该文件下修改为：tracker_server 192.168.52.99:221222spring-mvc.xml配置bean：                <bean id="multipartResolver"      class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >  </bean>
```

#### Spring boot如何发布webService

思维导图的使用，便于梳理清晰的路线

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/spring%20boot%20开发web%20service/非税票据.png)

%23spring%20boot%20%u5F00%u53D1web%20service%0A%0A%23%23%23%23_WSDL%uFF1AWeb%20Services%20Description%20Language%20%uFF08web%20%u670D%u52A1%u63CF%u8FF0%u8BED%u8A00%uFF09_%0A%23%23%23%23_Soap%uFF1ASimple%20Object%20Access%20Protocol%20%uFF08%u7B80%u5355%u5BF9%u8C61%u8BBF%u95EE%u534F%u8BAE%uFF09_%0A%0A%23%23%23BigDecimal%u7684%u4F7F%u7528%0A%09bigdecimal%20%u662F%u9AD8%u7CBE%u5EA6%u6570%u636E%0A%09%u5176%u4F7F%u7528%u53EF%u4EE5%u901A%u8FC7%0A%09%09BigDecimal%20bd%20%3D%20new%20BigDecimal%28String%29%3B%0A%09%09bg.add%28new%20BigDecimal%28%29%29%3B//%u8FD9%u6837%u5199bd%u6C38%u8FDC%u662F%u521D%u59CB%u503C%uFF0C%u4E0D%u4F1A%u8FDB%u884C%u4EFB%u4F55%u6539%u53D8%0A%09%09bd%20%3D%20bd.add%28new%20BigDecimal%28%29%29%3B//%u8FD9%u6837%u624D%u662F%u6B63%u786E%u7684%u5199%u6CD5%0A%23%23%23mongoDB%u7684%u4F7F%u7528%0A%3E**mongoDB%u7684%u670D%u52A1%u5668%u5730%u5740%u4E3A%uFF1A59.110.19.71**%0A%3E**%u8D26%u53F7%u5BC6%u7801%u4E3A%uFF1Aroot/dzpj@123**%0A%09_SecureCRTPortable%u8FDE%u63A5%u5230mongo%u6570%u636E%u5E93_%0A%09_%u4F7F%u7528%u5982%u4E0B%u547D%u4EE4%u8FDB%u884C%u64CD%u4F5C_%0A%09%0A%0A%20%20%20%20cd%20/usr/local/mongo/bin%20//%u8FDB%u5165mongo%u7684%u4F4D%u7F6E%0A%20%20%20%20./mongo%20//%u542F%u52A8mongo%0A%20%20%20%20//mongo%u7684%u591A%u6761%u4EF6%u67E5%u8BE2%0A%20%20%20%20db.generalPaymentFile.find%28%7B%22payment.coCode%22%3A%221234%22%7D%29%0A%0Amongo%u6570%u636E%u5E93%u4E00%u822C%u4E0D%u9700%u8981%u8BBE%u7F6E%u6388%u6743%uFF0C%u4F46%u8FD9%u6837%u5B58%u5728%u98CE%u9669%u3002%u53EF%u4EE5%u901A%u8FC7%u6388%u6743%u65B9%u5F0F%u8FDB%u884C%u5B89%u5168%u52A0%u5BC6%uFF1A%0A%0A%20%20%20%20use%20test%0A%20%20%20%20db.createUser%28%0A%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20user%3A%20%22nontaxbill%22%2C%0A%20%20%20%20%20%20%20pwd%3A%20%22dzpj123%22%2C%0A%20%20%20%20%20%20%20roles%3A%20%5B%0A%20%20%20%20%20%20%20%20%20%20%7B%20role%3A%20%22readWrite%22%2C%20db%3A%20%22test%22%20%7D%0A%20%20%20%20%20%20%20%5D%0A%20%20%20%20%20%7D%0A%20%20%20%20%20%29%0A%20%20%20%20ps%20-ef%20%7C%20grep%20mongo%0A%20%20%20%20kill%20-9%20pid%0A%20%20%20%20./mongod%20--auth%20%26%0A%20%20%20%20%u8FD9%u662F%u8FDB%u5165test%u6570%u636E%u5E93%u65F6%u9700%u8981%0A%20%20%20%20%20db.auth%28%27nontaxbill%27%2C%27dzpj123%27%29%0A%20%20%20%20//%u4E0D%u8FDB%u884C%u6388%u6743%u4F1A%u62A5%u9519%uFF0C%u6388%u6743%u540E%u53EF%u4EE5%u8FDB%u884C%u64CD%u4F5C%0A%0A%u5728%u6570%u636E%u5E93%u6388%u6743%u4E4B%u540Espring%20boot%u4E2D%u8FDB%u884C%u914D%u7F6E%u7684%u66F4%u6539%u3002%0A%0A%20%20%20%20//%u539F%u914D%u7F6E%0A%20%20%20%20spring.data.mongodb.uri%3Dmongodb%3A//localhost%3A27017/test%0A%20%20%20%20//%u66F4%u6539%u4E3A%0A%20%20%20%20spring.data.mongodb.uri%3Dmongodb%3A//name%3Apass@localhost%3A27017/test%0A%20%20%20%20%0A%20%20%20%20%0A%0A%23%23%23FastDfs%u7684%u4F7F%u7528%0A%3E**ClientGlobal%u7684%u4F7F%u7528**%0A%3E%0A%09%3E_%u5728%u7EBF%u914D%u7F6E_%20%0A%09%3E%60%20%u4E0D%u662F%u5F88%u61C2%u9700%u8981%u4ED4%u7EC6%u7814%u7A76%0A%09%09%20%20%0A%09%20%20//%u8BBE%u7F6E%u7F16%u7801%u683C%u5F0F%0A%09%20%20ClientGlobal.setG\_charset%28%22UTF-8%22%29%3B%0A%20%20%20%20%20%20//%u8BBE%u7F6E%u8FDE%u63A5%u8D85%u65F6%u65F6%u95F4%uFF08%u79D2%uFF09%0A%20%20%20%20%20%20ClientGlobal.setG\_connect\_timeout%282%29%3B%0A%20%20%20%20%20%20//%u8BBE%u7F6E%u4E0B%u8F7D%u8D85%u65F6%u65F6%u95F4%uFF08%u79D2%uFF09%0A%20%20%20%20%20%20ClientGlobal.setG\_network\_timeout%2830%29%3B%0A%09%20%20//%0A%20%20%20%20%20%20ClientGlobal.setG\_anti\_steal\_token%28false%29%3B%0A%09%20%20//%u8BBE%u7F6E%u7AEF%u53E3%0A%20%20%20%20%20%20ClientGlobal.setG\_tracker\_http\_port%2822122%29%3B%0A%20%20%20%20%20%20//%u4E0D%u61C2%0A%20%20%20%20%20%20InetSocketAddress%5B%5D%20trackerServers%20%3D%20new%20InetSocketAddress%5B1%5D%3B%0A%20%20%20%20%20%20trackerServers%5B0%5D%20%3D%20new%20InetSocketAddress%28%22192.168.52.99%22%2C%2022122%29%3B%0A%20%20%20%20%20%20ClientGlobal.setG\_tracker\_group%28new%20TrackerGroup%28trackerServers%29%29%3B%60%0A%20%20%3E%0A%09%3E_%u672C%u5730%u6587%u4EF6%u914D%u7F6E_%20%0A%09%0A%0A%20%20%20%20ClientGlobal.init%28%22%u6587%u4EF6%u8DEF%u5F84%22%29%uFF1B//%u4F8B%u5982%22d%3A//fastdfs.conf%22%0A%20%20%20%20//%u6587%u4EF6%u7C7B%u5BB9%u5982%u4E0B%0A%20%20%20%20connect\_timeout%20%3D%202%0A%20%20%20%20network\_timeout%20%3D%2030%0A%09charset%20%3D%20UTF-8%0A%09tracker\_server%20%3D%20192.168.52.99%3A22122%0A%0A%3E_%u8FDB%u884C%u94FE%u63A5%uFF1A_%0A%09%09%0A%09TrackerClienttrackerClient%20%3D%20new%20TrackerClient%28%29%3B%0A%20%20%20%20TrackerServer%20%20trackerServer%20%3D%20trackerClient.getConnection%28%29%3B%0A%09StorageClient%20%20storageClient%20%3D%20new%20StorageClient%28trackerServer%2C%20storageServer%29%3B%0A%0A%3E%u4E0A%u4F20/%u4E0B%u8F7D%0A%0A%09storageClient.upload%28%29%3B%0A%09storageClient.download%28%29%3B%0A%0A%3Ebean%u7684%u914D%u7F6E%0A%0A%20%20%20%201.%u6587%u4EF6%u5B58%u50A8%u7C7B%u578B%u7684%u914D%u7F6E%uFF1A%28%u5728%u6587%u4EF6%u4E0A%u4F20%u65F6%u4F1A%u51FA%u73B0%u6587%u4EF6%u89E3%u6790%u9519%u8BEF%29%20%20%20%20%0A%20%20%20%20application.properties%3A%20fdfs\_configPath%3Dd%3A/EinvoiceConf/Invoiceclient/fdfs\_client.conf%uFF0C%0A%20%20%20%20%u5728%u8BE5%u6587%u4EF6%u4E0B%u4FEE%u6539%u4E3A%uFF1Atracker\_server%20192.168.52.99%3A221222%0A%20%20%20%20spring-mvc.xml%u914D%u7F6Ebean%uFF1A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%3Cbean%20id%3D%22multipartResolver%22%20%20%20%20%20%20class%3D%22org.springframework.web.multipart.commons.CommonsMultipartResolver%22%20%3E%20%20%3C/bean%3E%0A%0A%23%23%23%23Spring%20boot%u5982%u4F55%u53D1%u5E03webService%0A%u601D%u7EF4%u5BFC%u56FE%u7684%u4F7F%u7528%uFF0C%u4FBF%u4E8E%u68B3%u7406%u6E05%u6670%u7684%u8DEF%u7EBF%0A%0A%21%5B%u5229%u7528%u767E%u5EA6%u8111%u56FE%u8FDB%u884C%u7ED8%u5236%u7684%u601D%u7EF4%u5BFC%u56FE%5D%28./%u975E%u7A0E%u7968%u636E.png%29%0A

