# 切面方式动态切换数据库

1. 设置扫描的注解

> /\*\*
>
> * 多数据源切换时定义的注解  
> * Created by Administrator on 2017/9/27.  
>
>   \*/  
>
>   @Retention\(RetentionPolicy.RUNTIME\)  
>
>   @Target\(ElementType.METHOD\)  
>
>   public @interface DataSource {  
>
>   DataSourceEnum value\(\);  
>
>   }

1. 设置DataSourceEnum 这里主要是靠这个的值判断使用哪个数据库

> public enum DataSourceEnum {  
> MAIN\("mainData"\), AUTH\( "authData"\), CLIENT\("clientData"\);  
> private String value;
>
> private DataSourceEnum\( String value\) {  
> this.value = value;  
> }
>
> public String getValue\(\) {  
> return value;  
> }
>
> public void setValue\(String value\) {  
> this.value = value;  
> }  
> }

1. 设置DataSourceHolder 的切换

> public class DataSourceHolder {  
> public static final ThreadLocal holder = new ThreadLocal\(\);  
> public static void putDataSource\(String datasource\) {  
> holder.set\(datasource\);  
> }
>
> public static String getDataSource\(\) {  
> return holder.get\(\);  
> }
>
> public static void clearDataSource\(\){  
> holder.remove\(\);  
> }  
> }

1. 设置DBRoutingDataSource
   1. extends AbstractRoutingDataSource

> /\*\*
>
> * 实现aop数据源切换  
> * Created by Administrator on 2017/9/27.  
>
>   \*/  
>
>   public class DBRoutingDataSource extends AbstractRoutingDataSource {  
>
>   @Override  
>
>   protected Object determineCurrentLookupKey\(\) {  
>
>   return DataSourceHolder.getDataSource\(\);  
>
>   }  
>
> @Override  
> public void setTargetDataSources\(Map targetDataSources\) {  
> super.setTargetDataSources\(targetDataSources\);  
> }
>
> @Override  
> public Object unwrap\(Class iface\) throws SQLException {  
> return null;  
> }
>
> @Override  
> public boolean isWrapperFor\(Class iface\) throws SQLException {  
> return false;  
> }  
> }

1. 设置切面进入后的操作

> /\*\*
>
> * 切面  
> * Created by Administrator on 2017/9/27.  
>
>   \*/  
>
>   @Aspect  
>
>   public class DetermineAspect {  
>
> // @Pointcut\("within\(com.yonyou.einvoice.oprtdata.service.impl.\*\)"\)  
> public void pointCut\(\) {}
>
> // @Before\("pointCut\(\)"\)  
> public void before\(JoinPoint point\) {  
> Object target = point.getTarget\(\);  
> System.out.println\(target.toString\(\)\);  
> String method = point.getSignature\(\).getName\(\);  
> System.out.println\(method\);  
> Class&lt;?&gt; classz = target.getClass\(\);  
> Class&lt;?&gt;\[\] parameterTypes =  
> \(\(MethodSignature\) point.getSignature\(\)\).getMethod\(\).getParameterTypes\(\);  
> try {  
> Method m = classz.getMethod\(method, parameterTypes\);  
> System.out.println\(m.getName\(\)\);  
> if \(m != null && m.isAnnotationPresent\(DataSource.class\)\) {  
> DataSource data = m.getAnnotation\(DataSource.class\);  
> DataSourceHolder.putDataSource\(data.value\(\).getValue\(\)\);  
> }
>
> } catch \(Exception e\) {  
> e.printStackTrace\(\);  
> }
>
> }  
> }

1. 设置数据库配置文件

```text
<?xml version="1.0" encoding="UTF-8"?>
```

${jdbc.driver}${jdbc.url}${jdbc.username}${jdbc.password}${jdbc.pool.maxActive}${jdbc.pool.init}${jdbc.pool.maxWait}${jdbc.pool.minIdle}clientEncoding=UTF-8${database.driverClass}${database.url2}${database.user2}${database.password2}${database.driverClass}${database.url3}${database.user3}${database.password3}classpath\*:/mapper/\*.xml

1. 设置注解扫描
2. 切面调用

> @DataSource\(DataSourceEnum.AUTH\)  
> @Override  
> public List getActiveCorpInfo\(List corpIdList\) {  
> // 根据corpId查询信息  
> List corpList = userRepo.getActiveCorpInfo\(corpIdList\);  
> // 设置登录时间  
> this.buildLoginTime\(corpList\);
>
> return corpList;  
> }

* 参考 [https://hacpai.com/article/1488101038281](https://hacpai.com/article/1488101038281)

about:blank

