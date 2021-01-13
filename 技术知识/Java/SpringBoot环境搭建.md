# SpringBoot环境搭建
1.springBoot的搭建

1.1 搭建
idea工具；file；new project；spring initializr（选好jdk版本进行下一步）；进行个人的配置，next；选择web--勾选多选项中的web；下一步即可。

1.2 参考见
[http://blog.csdn.net/mengdonghui123456/article/details/71304550](http://blog.csdn.net/mengdonghui123456/article/details/71304550) （ [IntelliJ IDEA搭建Spring Boot的小Demo](http://blog.csdn.net/mengdonghui123456/article/details/71304550) ）
[https://eclipsesv.com/2016/08/25/springboot/](https://eclipsesv.com/2016/08/25/springboot/) （使用IDEA和maven快速搭建SpringBoot项目）

2.第一个controller

@RestController
public class helloController {

@RequestMapping("/hello")
public String say() {

return new String("hello spring boot");
}

private void setRedis() {
redisTemplate.opsForValue().set("key", "value", 2, TimeUnit.MINUTES);
}
}

3.整合redis

3.1 需要的jar包：
<!-- redis jar包 -->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
<version>1.5.3.RELEASE</version>
</dependency>
</dependencies>

3.2 整合的配置
@Configuration
@PropertySource(value = "classpath:/redis.properties")
@EnableCaching
public class RedisConfig extends CachingConfigurerSupport {

@Value("${spring.redis.host}")
private String host;

@Value("${spring.redis.port}")
private int port;

@Value("${spring.redis.password}")
private String password;

@Bean
public RedisConnectionFactory redisConnectionFactory() {
JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
JedisConnectionFactory jedisConnectionFactory = new JedisConnectionFactory(jedisPoolConfig);
jedisConnectionFactory.setHostName(host);
jedisConnectionFactory.setPort(port);
jedisConnectionFactory.setPassword(password);
return jedisConnectionFactory;
}

@Bean
public RedisTemplate<Object, Object> redisTemplate() {
RedisTemplate<Object, Object> template = new RedisTemplate<Object, Object>();
template.setConnectionFactory(redisConnectionFactory());
return template;
}
}

3.3 redis的使用

```
@Autowired
private StringRedisTemplate redisTemplate;

private void setRedis() {
  redisTemplate.opsForValue().set("key", "value", 2, TimeUnit.MINUTES);
}
```

3.4 参考见
[http://www.voidcn.com/blog/Mchange/article/p-4974508.html](http://www.voidcn.com/blog/Mchange/article/p-4974508.html) （Spring boot 手动配置redis）
[https://stackoverflow.com/questions/34201135/spring-redis-read-configuration-from-application-properties-file](https://stackoverflow.com/questions/34201135/spring-redis-read-configuration-from-application-properties-file) （stackoverflow）

4.整合mysql

参考见： [http://www.imooc.com/article/15406](http://www.imooc.com/article/15406)
参考见： [https://my.oschina.net/mondayer/blog/881056](https://my.oschina.net/mondayer/blog/881056)

about:blank