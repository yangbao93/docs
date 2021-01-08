# 背景
由于项目有单测覆盖率的要求，需要但本地就进行好单测的书写和覆盖。尽可能保证多的覆盖率。

# 情况
利用IDEA 的 run with coverage 了单测，结果出现: No coverage in 'all classes in scope'的问题
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e0523ce975444d96afeef29640d5cfa0~tplv-k3u1fbpfcp-watermark.image)

# 原因和解决办法
## 原因
由于test文件的路径(com.a.b.name)和被测试的service的路径(com.a.b.service.name)不同，引起的。
## 解决办法
将test文件放在一样的路径下 即 test下的com.a.b.service下
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9a6072c279724c3498a48485e2a84220~tplv-k3u1fbpfcp-watermark.image)