# Mybatis-Plus 集成（Spring boot项目）

**mybaits-plus是对mybatis的加强**

## mybatis-plus的好处

1. 对mybatis的无缝升级，无需进行额外设置
2. 提供CURD等很多基础sql，可以让rd集中精力于业务和复杂sql
3. 提供诸多的配置，如自动将驼峰转下划线
4. 多种主键生成策略（注解配置），通过注解可以配置字段的细节
5. 使用生成文件可以生成controller，service，mapper，xml等文件
6. 相关技术贴丰富，问题容易得到解答

## mybatis-plus的坏处

1. 简单实用上手难度低，用好还需要深入学习

## 一、集成与配置

### 1. pom文件引入

```text
<dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>druid</artifactId>
     <version>1.1.10</version>
</dependency>
<dependency>
     <groupId>com.baomidou</groupId>
     <artifactId>mybatis-plus</artifactId>
     <version>2.1.9</version>
</dependency>
     <dependency>
     <groupId>com.baomidou</groupId>
     <artifactId>mybatis-plus-boot-starter</artifactId>
     <version>2.1.9</version>
</dependency>
```

### 2. service的配置

```text
// **Service 需要继承 IService 
public interface **Service extends IService<Entity>

// **ServiceImpl 需要继承ServiceImpl
public class **ServiceImpl extends ServiceImpl<**Mapper,entity> implements **Service
```

### 3. mapeper的配置

```text
public interface **Mapper extends BaseMapper<entity>
```

### 4. application中的配置

```text
#mybatis plus 设置
mybatis-plus.mapper-locations=classpath:/mapper/*Mapper.xml
#实体扫描，多个package用逗号或者分号分隔
mybatis-plus.typeAliasesPackage=com.azuray.decodeme.entity.vo
##主键类型  0:"数据库ID自增", 1:"用户输入ID",2:"全局唯一ID (数字类型唯一ID)", 3:"全局唯一ID UUID";
#mybatis-plus.global-config.id-type=0
##字段策略 0:"忽略判断",1:"非 NULL 判断"),2:"非空判断"
#mybatis-plus.global-config.field-strategy=2
##驼峰下划线转换
mybatis-plus.global-config.db-column-underline=true
##刷新mapper 调试神器
#mybatis-plus.global-config.refresh-mapper=true
##数据库大写下划线转换
##mybatis-plus.global-config.capital-mode=true
##序列接口实现类配置
##逻辑删除配置
#mybatis-plus.global-config.logic-delete-value=0
#mybatis-plus.global-config.logic-not-delete-value=1
##自定义填充策略接口实现
##自定义SQL注入器
#mybatis-plus.configuration.map-underscore-to-camel-case=true
#mybatis-plus.configuration.cache-enabled=false
```

## 二、利用代码生成service,mapper,controller等

```text
package com.azuray.decodeme.common;

import java.sql.SQLException;

import com.baomidou.mybatisplus.enums.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DbType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

/**
 * 此类用于生成数据库中的文件
 * @author yb
 */
public class MyBatisPlusGenerator {

    public static void main(String[] args) throws SQLException {

        //1. 全局配置
        GlobalConfig config = new GlobalConfig();
        config.setActiveRecord(true) // 是否支持AR模式
                .setAuthor("mybatis-plus") // 作者
                // TODO 生成路径
                .setOutputDir("/Users/yangbao/public")
                .setFileOverride(true)  // 文件覆盖
                .setIdType(IdType.AUTO) // 主键策略
                .setServiceName("%sService")  // 设置生成的service接口的名字的首字母是否为I
                // IEmployeeService
                .setBaseResultMap(true)//生成基本的resultMap
                .setBaseColumnList(true)
                .setEnableCache(false);//生成基本的SQL片段

        //2. 数据源配置
        DataSourceConfig dsConfig = new DataSourceConfig();
        dsConfig.setDbType(DbType.MYSQL)  // 设置数据库类型
                .setDriverName("com.mysql.jdbc.Driver")
                // TODO 数据库连接
                .setUrl("jdbc:mysql://127.0.01:3306/test")
                // TODO 用户名
                .setUsername("root")
                // TODO 密码
                .setPassword("1234567");

        //3. 策略配置globalConfiguration中
        StrategyConfig stConfig = new StrategyConfig();
        stConfig.setCapitalMode(true) //全局大写命名
                .setDbColumnUnderline(true)  // 指定表名 字段名是否使用下划线
                .setNaming(NamingStrategy.underline_to_camel) // 数据库表映射到实体的命名策略
                //.setTablePrefix("tbl_") // 表的前缀
                //TODO 要生成的表
                .setInclude("user_inf")
                .setEntityLombokModel(true)
                .setRestControllerStyle(true);

        //4. 包名策略配置
        PackageConfig pkConfig = new PackageConfig();
        pkConfig.setParent("com.azuray.decodeme")
                .setMapper("mapper")//dao
                .setService("service")//servcie
                .setController("controller")//controller
                .setEntity("entity.vo")
                .setXml("mapper");//mapper.xml

        //5. 整合配置
        AutoGenerator ag = new AutoGenerator();
        ag.setGlobalConfig(config)
                .setDataSource(dsConfig)
                .setStrategy(stConfig)
                .setPackageInfo(pkConfig);

        //6. 执行
        ag.execute();
    }
}
```

### 分页设置

```text
暂略
```

**mybaits-plus%E6%98%AF%E5%AF%B9mybatis%E7%9A%84%E5%8A%A0%E5%BC%BA**%0A%23%23%23%23%20mybatis-plus%E7%9A%84%E5%A5%BD%E5%A4%84%0A1.%20%E5%AF%B9mybatis%E7%9A%84%E6%97%A0%E7%BC%9D%E5%8D%87%E7%BA%A7%EF%BC%8C%E6%97%A0%E9%9C%80%E8%BF%9B%E8%A1%8C%E9%A2%9D%E5%A4%96%E8%AE%BE%E7%BD%AE%0A2.%20%E6%8F%90%E4%BE%9BCURD%E7%AD%89%E5%BE%88%E5%A4%9A%E5%9F%BA%E7%A1%80sql%EF%BC%8C%E5%8F%AF%E4%BB%A5%E8%AE%A9rd%E9%9B%86%E4%B8%AD%E7%B2%BE%E5%8A%9B%E4%BA%8E%E4%B8%9A%E5%8A%A1%E5%92%8C%E5%A4%8D%E6%9D%82sql%0A3.%20%E6%8F%90%E4%BE%9B%E8%AF%B8%E5%A4%9A%E7%9A%84%E9%85%8D%E7%BD%AE%EF%BC%8C%E5%A6%82%E8%87%AA%E5%8A%A8%E5%B0%86%E9%A9%BC%E5%B3%B0%E8%BD%AC%E4%B8%8B%E5%88%92%E7%BA%BF%0A4.%20%E5%A4%9A%E7%A7%8D%E4%B8%BB%E9%94%AE%E7%94%9F%E6%88%90%E7%AD%96%E7%95%A5%EF%BC%88%E6%B3%A8%E8%A7%A3%E9%85%8D%E7%BD%AE%EF%BC%89%EF%BC%8C%E9%80%9A%E8%BF%87%E6%B3%A8%E8%A7%A3%E5%8F%AF%E4%BB%A5%E9%85%8D%E7%BD%AE%E5%AD%97%E6%AE%B5%E7%9A%84%E7%BB%86%E8%8A%82%0A5.%20%E4%BD%BF%E7%94%A8%E7%94%9F%E6%88%90%E6%96%87%E4%BB%B6%E5%8F%AF%E4%BB%A5%E7%94%9F%E6%88%90controller%EF%BC%8Cservice%EF%BC%8Cmapper%EF%BC%8Cxml%E7%AD%89%E6%96%87%E4%BB%B6%0A6.%20%E7%9B%B8%E5%85%B3%E6%8A%80%E6%9C%AF%E8%B4%B4%E4%B8%B0%E5%AF%8C%EF%BC%8C%E9%97%AE%E9%A2%98%E5%AE%B9%E6%98%93%E5%BE%97%E5%88%B0%E8%A7%A3%E7%AD%94%0A%0A%23%23%23%23%20mybatis-plus%E7%9A%84%E5%9D%8F%E5%A4%84%0A1.%20%E7%AE%80%E5%8D%95%E5%AE%9E%E7%94%A8%E4%B8%8A%E6%89%8B%E9%9A%BE%E5%BA%A6%E4%BD%8E%EF%BC%8C%E7%94%A8%E5%A5%BD%E8%BF%98%E9%9C%80%E8%A6%81%E6%B7%B1%E5%85%A5%E5%AD%A6%E4%B9%A0%0A%0A%23%23%23%23%20%E4%B8%80%E3%80%81%E9%9B%86%E6%88%90%E4%B8%8E%E9%85%8D%E7%BD%AE%0A%23%23%23%23%23%201.%20pom%E6%96%87%E4%BB%B6%E5%BC%95%E5%85%A5%0A%60%60%60xml%0A%3Cdependency%3E%0A%20%20%20%20%20%3CgroupId%3Ecom.alibaba%3C%2FgroupId%3E%0A%20%20%20%20%20%3CartifactId%3Edruid%3C%2FartifactId%3E%0A%20%20%20%20%20%3Cversion%3E1.1.10%3C%2Fversion%3E%0A%3C%2Fdependency%3E%0A%3Cdependency%3E%0A%20%20%20%20%20%3CgroupId%3Ecom.baomidou%3C%2FgroupId%3E%0A%20%20%20%20%20%3CartifactId%3Emybatis-plus%3C%2FartifactId%3E%0A%20%20%20%20%20%3Cversion%3E2.1.9%3C%2Fversion%3E%0A%3C%2Fdependency%3E%0A%20%20%20%20%20%3Cdependency%3E%0A%20%20%20%20%20%3CgroupId%3Ecom.baomidou%3C%2FgroupId%3E%0A%20%20%20%20%20%3CartifactId%3Emybatis-plus-boot-starter%3C%2FartifactId%3E%0A%20%20%20%20%20%3Cversion%3E2.1.9%3C%2Fversion%3E%0A%3C%2Fdependency%3E%0A%60%60%60%0A%23%23%23%23%23%202.%20service%E7%9A%84%E9%85%8D%E7%BD%AE%0A%60%60%60java%0A%2F%2F%20**Service%20%E9%9C%80%E8%A6%81%E7%BB%A7%E6%89%BF%20IService%20%0Apublic%20interface%20**Service%20extends%20IService%3CEntity%3E%0A%0A%2F%2F%20**ServiceImpl%20%E9%9C%80%E8%A6%81%E7%BB%A7%E6%89%BFServiceImpl%0Apublic%20class%20**ServiceImpl%20extends%20ServiceImpl%3C**Mapper%2Centity%3E%20implements%20**Service%0A%60%60%60%0A%23%23%23%23%23%203.%20mapeper%E7%9A%84%E9%85%8D%E7%BD%AE%0A%60%60%60java%0Apublic%20interface%20**Mapper%20extends%20BaseMapper%3Centity%3E%0A%60%60%60%0A%0A%23%23%23%23%23%204.%20application%E4%B8%AD%E7%9A%84%E9%85%8D%E7%BD%AE%0A%60%60%60properties%0A%23mybatis%20plus%20%E8%AE%BE%E7%BD%AE%0Amybatis-plus.mapper-locations%3Dclasspath%3A%2Fmapper%2F\*Mapper.xml%0A%23%E5%AE%9E%E4%BD%93%E6%89%AB%E6%8F%8F%EF%BC%8C%E5%A4%9A%E4%B8%AApackage%E7%94%A8%E9%80%97%E5%8F%B7%E6%88%96%E8%80%85%E5%88%86%E5%8F%B7%E5%88%86%E9%9A%94%0Amybatis-plus.typeAliasesPackage%3Dcom.azuray.decodeme.entity.vo%0A%23%23%E4%B8%BB%E9%94%AE%E7%B1%BB%E5%9E%8B%20%200%3A%22%E6%95%B0%E6%8D%AE%E5%BA%93ID%E8%87%AA%E5%A2%9E%22%2C%201%3A%22%E7%94%A8%E6%88%B7%E8%BE%93%E5%85%A5ID%22%2C2%3A%22%E5%85%A8%E5%B1%80%E5%94%AF%E4%B8%80ID%20\(%E6%95%B0%E5%AD%97%E7%B1%BB%E5%9E%8B%E5%94%AF%E4%B8%80ID\)%22%2C%203%3A%22%E5%85%A8%E5%B1%80%E5%94%AF%E4%B8%80ID%20UUID%22%3B%0A%23mybatis-plus.global-config.id-type%3D0%0A%23%23%E5%AD%97%E6%AE%B5%E7%AD%96%E7%95%A5%200%3A%22%E5%BF%BD%E7%95%A5%E5%88%A4%E6%96%AD%22%2C1%3A%22%E9%9D%9E%20NULL%20%E5%88%A4%E6%96%AD%22\)%2C2%3A%22%E9%9D%9E%E7%A9%BA%E5%88%A4%E6%96%AD%22%0A%23mybatis-plus.global-config.field-strategy%3D2%0A%23%23%E9%A9%BC%E5%B3%B0%E4%B8%8B%E5%88%92%E7%BA%BF%E8%BD%AC%E6%8D%A2%0Amybatis-plus.global-config.db-column-underline%3Dtrue%0A%23%23%E5%88%B7%E6%96%B0mapper%20%E8%B0%83%E8%AF%95%E7%A5%9E%E5%99%A8%0A%23mybatis-plus.global-config.refresh-mapper%3Dtrue%0A%23%23%E6%95%B0%E6%8D%AE%E5%BA%93%E5%A4%A7%E5%86%99%E4%B8%8B%E5%88%92%E7%BA%BF%E8%BD%AC%E6%8D%A2%0A%23%23mybatis-plus.global-config.capital-mode%3Dtrue%0A%23%23%E5%BA%8F%E5%88%97%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E7%B1%BB%E9%85%8D%E7%BD%AE%0A%23%23%E9%80%BB%E8%BE%91%E5%88%A0%E9%99%A4%E9%85%8D%E7%BD%AE%0A%23mybatis-plus.global-config.logic-delete-value%3D0%0A%23mybatis-plus.global-config.logic-not-delete-value%3D1%0A%23%23%E8%87%AA%E5%AE%9A%E4%B9%89%E5%A1%AB%E5%85%85%E7%AD%96%E7%95%A5%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%0A%23%23%E8%87%AA%E5%AE%9A%E4%B9%89SQL%E6%B3%A8%E5%85%A5%E5%99%A8%0A%23mybatis-plus.configuration.map-underscore-to-camel-case%3Dtrue%0A%23mybatis-plus.configuration.cache-enabled%3Dfalse%0A%60%60%60%0A%0A%23%23%23%23%20%E4%BA%8C%E3%80%81%E5%88%A9%E7%94%A8%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90service%2Cmapper%2Ccontroller%E7%AD%89%0A%60%60%60java%0Apackage%20com.azuray.decodeme.common%3B%0A%0Aimport%20java.sql.SQLException%3B%0A%0Aimport%20com.baomidou.mybatisplus.enums.IdType%3B%0Aimport%20com.baomidou.mybatisplus.generator.AutoGenerator%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.DataSourceConfig%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.GlobalConfig%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.PackageConfig%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.StrategyConfig%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.rules.DbType%3B%0Aimport%20com.baomidou.mybatisplus.generator.config.rules.NamingStrategy%3B%0A%0A%2F**%0A%20_%20%E6%AD%A4%E7%B1%BB%E7%94%A8%E4%BA%8E%E7%94%9F%E6%88%90%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6%0A%20_%20%40author%20yb%0A%20\*%2F%0Apublic%20class%20MyBatisPlusGenerator%20%7B%0A%0A%20%20%20%20public%20static%20void%20main\(String%5B%5D%20args\)%20throws%20SQLException%20%7B%0A%0A%20%20%20%20%20%20%20%20%2F%2F1.%20%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE%0A%20%20%20%20%20%20%20%20GlobalConfig%20config%20%3D%20new%20GlobalConfig\(\)%3B%0A%20%20%20%20%20%20%20%20config.setActiveRecord\(true\)%20%2F%2F%20%E6%98%AF%E5%90%A6%E6%94%AF%E6%8C%81AR%E6%A8%A1%E5%BC%8F%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setAuthor\(%22mybatis-plus%22\)%20%2F%2F%20%E4%BD%9C%E8%80%85%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F%20TODO%20%E7%94%9F%E6%88%90%E8%B7%AF%E5%BE%84%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setOutputDir\(%22%2FUsers%2Fyangbao%2Fpublic%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setFileOverride\(true\)%20%20%2F%2F%20%E6%96%87%E4%BB%B6%E8%A6%86%E7%9B%96%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setIdType\(IdType.AUTO\)%20%2F%2F%20%E4%B8%BB%E9%94%AE%E7%AD%96%E7%95%A5%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setServiceName\(%22%25sService%22\)%20%20%2F%2F%20%E8%AE%BE%E7%BD%AE%E7%94%9F%E6%88%90%E7%9A%84service%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%90%8D%E5%AD%97%E7%9A%84%E9%A6%96%E5%AD%97%E6%AF%8D%E6%98%AF%E5%90%A6%E4%B8%BAI%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F%20IEmployeeService%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setBaseResultMap\(true\)%2F%2F%E7%94%9F%E6%88%90%E5%9F%BA%E6%9C%AC%E7%9A%84resultMap%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setBaseColumnList\(true\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setEnableCache\(false\)%3B%2F%2F%E7%94%9F%E6%88%90%E5%9F%BA%E6%9C%AC%E7%9A%84SQL%E7%89%87%E6%AE%B5%0A%0A%20%20%20%20%20%20%20%20%2F%2F2.%20%E6%95%B0%E6%8D%AE%E6%BA%90%E9%85%8D%E7%BD%AE%0A%20%20%20%20%20%20%20%20DataSourceConfig%20dsConfig%20%3D%20new%20DataSourceConfig\(\)%3B%0A%20%20%20%20%20%20%20%20dsConfig.setDbType\(DbType.MYSQL\)%20%20%2F%2F%20%E8%AE%BE%E7%BD%AE%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B1%BB%E5%9E%8B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setDriverName\(%22com.mysql.jdbc.Driver%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F%20TODO%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setUrl\(%22jdbc%3Amysql%3A%2F%2F127.0.01%3A3306%2Ftest%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F%20TODO%20%E7%94%A8%E6%88%B7%E5%90%8D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setUsername\(%22root%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F%20TODO%20%E5%AF%86%E7%A0%81%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setPassword\(%221234567%22\)%3B%0A%0A%20%20%20%20%20%20%20%20%2F%2F3.%20%E7%AD%96%E7%95%A5%E9%85%8D%E7%BD%AEglobalConfiguration%E4%B8%AD%0A%20%20%20%20%20%20%20%20StrategyConfig%20stConfig%20%3D%20new%20StrategyConfig\(\)%3B%0A%20%20%20%20%20%20%20%20stConfig.setCapitalMode\(true\)%20%2F%2F%E5%85%A8%E5%B1%80%E5%A4%A7%E5%86%99%E5%91%BD%E5%90%8D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setDbColumnUnderline\(true\)%20%20%2F%2F%20%E6%8C%87%E5%AE%9A%E8%A1%A8%E5%90%8D%20%E5%AD%97%E6%AE%B5%E5%90%8D%E6%98%AF%E5%90%A6%E4%BD%BF%E7%94%A8%E4%B8%8B%E5%88%92%E7%BA%BF%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setNaming\(NamingStrategy.underline_to\_camel\)%20%2F%2F%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8%E6%98%A0%E5%B0%84%E5%88%B0%E5%AE%9E%E4%BD%93%E7%9A%84%E5%91%BD%E5%90%8D%E7%AD%96%E7%95%A5%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2F.setTablePrefix\(%22tbl_%22\)%20%2F%2F%20%E8%A1%A8%E7%9A%84%E5%89%8D%E7%BC%80%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%2F%2FTODO%20%E8%A6%81%E7%94%9F%E6%88%90%E7%9A%84%E8%A1%A8%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setInclude\(%22user\_inf%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setEntityLombokModel\(true\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setRestControllerStyle\(true\)%3B%0A%0A%20%20%20%20%20%20%20%20%2F%2F4.%20%E5%8C%85%E5%90%8D%E7%AD%96%E7%95%A5%E9%85%8D%E7%BD%AE%0A%20%20%20%20%20%20%20%20PackageConfig%20pkConfig%20%3D%20new%20PackageConfig\(\)%3B%0A%20%20%20%20%20%20%20%20pkConfig.setParent\(%22com.azuray.decodeme%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setMapper\(%22mapper%22\)%2F%2Fdao%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setService\(%22service%22\)%2F%2Fservcie%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setController\(%22controller%22\)%2F%2Fcontroller%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setEntity\(%22entity.vo%22\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setXml\(%22mapper%22\)%3B%2F%2Fmapper.xml%0A%0A%20%20%20%20%20%20%20%20%2F%2F5.%20%E6%95%B4%E5%90%88%E9%85%8D%E7%BD%AE%0A%20%20%20%20%20%20%20%20AutoGenerator%20ag%20%3D%20new%20AutoGenerator\(\)%3B%0A%20%20%20%20%20%20%20%20ag.setGlobalConfig\(config\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setDataSource\(dsConfig\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setStrategy\(stConfig\)%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20.setPackageInfo\(pkConfig\)%3B%0A%0A%20%20%20%20%20%20%20%20%2F%2F6.%20%E6%89%A7%E8%A1%8C%0A%20%20%20%20%20%20%20%20ag.execute\(\)%3B%0A%20%20%20%20%7D%0A%7D%0A%60%60%60%0A%23%23%23%23%23%20%E5%88%86%E9%A1%B5%E8%AE%BE%E7%BD%AE%0A%60%60%60java%0A%E6%9A%82%E7%95%A5%0A%60%60%60

