# Mybatis-plus 3.0+ 集成spring boot出现SqlSessionFactory空指针问题
#### 出现的问题 DataSource空指针

```
Error: GlobalConfigUtils setMetaData Fail !  Cause:java.lang.NullPointerException
```

全部报错为：

```
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userInfoController' defined in file [/Users/yangbao/projects/decodeme/target/classes/com/azuray/decodeme/controller/UserInfoController.class]: Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userInfoService': Unsatisfied dependency expressed through field 'baseMapper'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userInfoMapper' defined in file [/Users/yangbao/projects/decodeme/target/classes/com/azuray/decodeme/mapper/UserInfoMapper.class]: Unsatisfied dependency expressed through bean property 'sqlSessionFactory'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/spring/boot/starter/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is com.baomidou.mybatisplus.exceptions.MybatisPlusException: Error: GlobalConfigUtils setMetaData Fail !  Cause:java.lang.NullPointerException
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:729) ~[spring-beans-5.0.5.RELEASE.jar:5.0.5.RELEASE]
```

#### 问题出现的前提情况

1. 项目拆分
2. 将mp从2.0+升级到3.0+

#### 问题出现的原因

由日志可知是出现了空指针，由mp报错，说没有扫描到GlobalConfig的配置

#### 解决办法

1. 参考网上的解决办法，在properties/yml中设置global-config相关的属性，一条即可，如

```
mybatis-plus.global-config.db-config.db-type=mysql
```

但我设置无效，原因是出现这个错误并不是因为没有设置这些，而是DataSource里面的url等配置为空，并没有扫描到对应的url配置。

1. **spring-boot可以在mybatis的配置类中设置DataSource，如:**

```
@Configuration
public class MybatisPlusConfig {

    @Bean(name = "dataSource")
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl("");
        dataSource.setUsername("");
        dataSource.setPassword("");
        dataSource.setDbType("mysql");
        return dataSource;
    }
```

#### 引申问题

* 当使用推荐的h2时，可以不用单独在java中配置DataSource
* 在报错之前也有出现 {dataSource-1} init error 错误，这说明错误出现在DataSource的初始化

%23%23%23%23%20%E5%87%BA%E7%8E%B0%E7%9A%84%E9%97%AE%E9%A2%98%20DataSource%E7%A9%BA%E6%8C%87%E9%92%88%0A%60%60%60xml%0AError%3A%20GlobalConfigUtils%20setMetaData%20Fail%20!%20%20Cause%3Ajava.lang.NullPointerException%0A%60%60%60%0A%E5%85%A8%E9%83%A8%E6%8A%A5%E9%94%99%E4%B8%BA%EF%BC%9A%0A%60%60%60xml%0Aorg.springframework.beans.factory.UnsatisfiedDependencyException%3A%20Error%20creating%20bean%20with%20name%20'userInfoController'%20defined%20in%20file%20%5B%2FUsers%2Fyangbao%2Fprojects%2Fdecodeme%2Ftarget%2Fclasses%2Fcom%2Fazuray%2Fdecodeme%2Fcontroller%2FUserInfoController.class%5D%3A%20Unsatisfied%20dependency%20expressed%20through%20constructor%20parameter%200%3B%20nested%20exception%20is%20org.springframework.beans.factory.UnsatisfiedDependencyException%3A%20Error%20creating%20bean%20with%20name%20'userInfoService'%3A%20Unsatisfied%20dependency%20expressed%20through%20field%20'baseMapper'%3B%20nested%20exception%20is%20org.springframework.beans.factory.UnsatisfiedDependencyException%3A%20Error%20creating%20bean%20with%20name%20'userInfoMapper'%20defined%20in%20file%20%5B%2FUsers%2Fyangbao%2Fprojects%2Fdecodeme%2Ftarget%2Fclasses%2Fcom%2Fazuray%2Fdecodeme%2Fmapper%2FUserInfoMapper.class%5D%3A%20Unsatisfied%20dependency%20expressed%20through%20bean%20property%20'sqlSessionFactory'%3B%20nested%20exception%20is%20org.springframework.beans.factory.BeanCreationException%3A%20Error%20creating%20bean%20with%20name%20'sqlSessionFactory'%20defined%20in%20class%20path%20resource%20%5Bcom%2Fbaomidou%2Fmybatisplus%2Fspring%2Fboot%2Fstarter%2FMybatisPlusAutoConfiguration.class%5D%3A%20Bean%20instantiation%20via%20factory%20method%20failed%3B%20nested%20exception%20is%20org.springframework.beans.BeanInstantiationException%3A%20Failed%20to%20instantiate%20%5Borg.apache.ibatis.session.SqlSessionFactory%5D%3A%20Factory%20method%20'sqlSessionFactory'%20threw%20exception%3B%20nested%20exception%20is%20com.baomidou.mybatisplus.exceptions.MybatisPlusException%3A%20Error%3A%20GlobalConfigUtils%20setMetaData%20Fail%20!%20%20Cause%3Ajava.lang.NullPointerException%0A%09at%20org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java%3A729)%20~%5Bspring-beans-5.0.5.RELEASE.jar%3A5.0.5.RELEASE%5D%0A%60%60%60%0A%23%23%23%23%20%E9%97%AE%E9%A2%98%E5%87%BA%E7%8E%B0%E7%9A%84%E5%89%8D%E6%8F%90%E6%83%85%E5%86%B5%0A1.%20%E9%A1%B9%E7%9B%AE%E6%8B%86%E5%88%86%0A2.%20%E5%B0%86mp%E4%BB%8E2.0%2B%E5%8D%87%E7%BA%A7%E5%88%B03.0%2B%0A%0A%23%23%23%23%20%E9%97%AE%E9%A2%98%E5%87%BA%E7%8E%B0%E7%9A%84%E5%8E%9F%E5%9B%A0%0A%E7%94%B1%E6%97%A5%E5%BF%97%E5%8F%AF%E7%9F%A5%E6%98%AF%E5%87%BA%E7%8E%B0%E4%BA%86%E7%A9%BA%E6%8C%87%E9%92%88%EF%BC%8C%E7%94%B1mp%E6%8A%A5%E9%94%99%EF%BC%8C%E8%AF%B4%E6%B2%A1%E6%9C%89%E6%89%AB%E6%8F%8F%E5%88%B0GlobalConfig%E7%9A%84%E9%85%8D%E7%BD%AE%0A%0A%23%23%23%23%20%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%0A1.%20%E5%8F%82%E8%80%83%E7%BD%91%E4%B8%8A%E7%9A%84%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%EF%BC%8C%E5%9C%A8properties%2Fyml%E4%B8%AD%E8%AE%BE%E7%BD%AEglobal-config%E7%9B%B8%E5%85%B3%E7%9A%84%E5%B1%9E%E6%80%A7%EF%BC%8C%E4%B8%80%E6%9D%A1%E5%8D%B3%E5%8F%AF%EF%BC%8C%E5%A6%82%0A%60%60%60properties%0Amybatis-plus.global-config.db-config.db-type%3Dmysql%0A%60%60%60%0A%26nbsp%3B%26nbsp%3B%26nbsp%3B%3Cfont%20color%3D%22%23dd0000%22%3E%E4%BD%86%E6%88%91%E8%AE%BE%E7%BD%AE%E6%97%A0%E6%95%88%EF%BC%8C%E5%8E%9F%E5%9B%A0%E6%98%AF%E5%87%BA%E7%8E%B0%E8%BF%99%E4%B8%AA%E9%94%99%E8%AF%AF%E5%B9%B6%E4%B8%8D%E6%98%AF%E5%9B%A0%E4%B8%BA%E6%B2%A1%E6%9C%89%E8%AE%BE%E7%BD%AE%E8%BF%99%E4%BA%9B%EF%BC%8C%E8%80%8C%E6%98%AFDataSource%E9%87%8C%E9%9D%A2%E7%9A%84url%E7%AD%89%E9%85%8D%E7%BD%AE%E4%B8%BA%E7%A9%BA%EF%BC%8C%E5%B9%B6%E6%B2%A1%E6%9C%89%E6%89%AB%E6%8F%8F%E5%88%B0%E5%AF%B9%E5%BA%94%E7%9A%84url%E9%85%8D%E7%BD%AE%E3%80%82%3C%2Ffont%3E%0A%0A2.%20**spring-boot%E5%8F%AF%E4%BB%A5%E5%9C%A8mybatis%E7%9A%84%E9%85%8D%E7%BD%AE%E7%B1%BB%E4%B8%AD%E8%AE%BE%E7%BD%AEDataSource%EF%BC%8C%E5%A6%82%3A**%0A%0A%60%60%60java%0A%40Configuration%0Apublic%20class%20MybatisPlusConfig%20%7B%0A%0A%0A%20%20%20%20%40Bean(name%20%3D%20%22dataSource%22)%0A%20%20%20%20%40ConfigurationProperties(prefix%20%3D%20%22spring.datasource%22)%0A%20%20%20%20public%20DataSource%20dataSource()%20%7B%0A%20%20%20%20%20%20%20%20DruidDataSource%20dataSource%20%3D%20new%20DruidDataSource()%3B%0A%20%20%20%20%20%20%20%20dataSource.setUrl(%22%22)%3B%0A%20%20%20%20%20%20%20%20dataSource.setUsername(%22%22)%3B%0A%20%20%20%20%20%20%20%20dataSource.setPassword(%22%22)%3B%0A%20%20%20%20%20%20%20%20dataSource.setDbType(%22mysql%22)%3B%0A%20%20%20%20%20%20%20%20return%20dataSource%3B%0A%20%20%20%20%7D%0A%60%60%60%0A%23%23%23%23%20%E5%BC%95%E7%94%B3%E9%97%AE%E9%A2%98%0A-%20%E5%BD%93%E4%BD%BF%E7%94%A8%E6%8E%A8%E8%8D%90%E7%9A%84h2%E6%97%B6%EF%BC%8C%E5%8F%AF%E4%BB%A5%E4%B8%8D%E7%94%A8%E5%8D%95%E7%8B%AC%E5%9C%A8java%E4%B8%AD%E9%85%8D%E7%BD%AEDataSource%0A-%20%E5%9C%A8%E6%8A%A5%E9%94%99%E4%B9%8B%E5%89%8D%E4%B9%9F%E6%9C%89%E5%87%BA%E7%8E%B0%20%3Cfont%20color%3D%22%23dd0000%22%3E%7BdataSource-1%7D%20init%20error%3C%2Ffont%3E%20%E9%94%99%E8%AF%AF%EF%BC%8C%E8%BF%99%E8%AF%B4%E6%98%8E%E9%94%99%E8%AF%AF%E5%87%BA%E7%8E%B0%E5%9C%A8DataSource%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96