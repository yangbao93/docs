# SpringBoot | 第八章：统一异常、数据校验处理
## SpringBoot | 第八章：统一异常、数据校验处理

oKong [ImportNew]()  
（点击上方公众号，可快速关注）

> 来源：oKong ，  

> blog.lqdev.cn/2018/07/20/springboot/chapter-eight/  

**前言**

在web应用中，请求处理时，出现异常是非常常见的。所以当应用出现各类异常时，进行异常的捕获或者二次处理(比如sql异常正常是不能外抛)是非常必要的，比如在开发对外api服务时，约定了响应的参数格式，如respCode、respMsg，调用方根据错误码进行自己的业务逻辑。本章节就重点讲解下统一异常和数据校验处理。

springboot中，默认在发送异常时，会跳转值/error请求进行错误的展现，根据不同的Content-Type展现不同的错误结果，如json请求时，直接返回json格式参数。

浏览器访问异常时：

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

使用postman访问时：

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

**统一异常处理**

显然，默认的异常页是对用户或者调用者而言都是不友好的，所以一般上我们都会进行实现自己业务的异常提示信息。

**创建全局的统一异常处理类**

利用@ControllerAdvice和@ExceptionHandler定义一个统一异常处理类

* @ControllerAdvice：控制器增强，使@ExceptionHandler、@InitBinder、@ModelAttribute注解的方法应用到所有的 @RequestMapping注解的方法。

* @ExceptionHandler：异常处理器，此注解的作用是当出现其定义的异常时进行处理的方法

创建异常类：CommonExceptionHandler

> @ControllerAdvice  

> public class CommonExceptionHandler {  

>   

> /**  

> *  拦截Exception类的异常  

> * @param e  

> * @return  

> */  

> @ExceptionHandler(Exception.class)  

> @ResponseBody  

> public Map<String,Object> exceptionHandler(Exception e){  

> Map<String,Object> result = new HashMap<String,Object>();  

> result.put("respCode", "9999");  

> result.put("respMsg", e.getMessage());  

> //正常开发中，可创建一个统一响应实体，如CommonResp  

> return result;  

> }  

> }  

多余不同异常(如自定义异常)，需要进行不同的异常处理时，可编写多个exceptionHandler方法，注解ExceptionHandler指定处理的异常类，如

> /**  

> * 拦截 CommonException 的异常  

> * @param ex  

> * @return  

> */  

> @ExceptionHandler(CommonException.class)  

> @ResponseBody  

> public Map<String,Object> exceptionHandler(CommonException ex){  

> log.info("CommonException：{}({})",ex.getMsg(), ex.getCode());  

> Map<String,Object> result = new HashMap<String,Object>();  

> result.put("respCode", ex.getCode());  

> result.put("respMsg", ex.getMsg());  

> return result;  

> }  

由于加入了@ResponseBody，所以返回的是json格式，

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

说明异常已经被拦截了。

可拦截不同的异常，进行不同的异常提示，比如NoHandlerFoundException、HttpMediaTypeNotSupportedException、AsyncRequestTimeoutException等等，这里就不列举了，读者可自己加入后实际操作下。

对于返回页面时，返回ModelAndView即可，如

> @ExceptionHandler(value = Exception.class)  

> public ModelAndView defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {  

> ModelAndView mav = new ModelAndView();  

> mav.addObject("exception", e);  

> mav.addObject("url", req.getRequestURL());  

> mav.setViewName(DEFAULT_ERROR_VIEW);  

> return mav;  

> }  

由于工作中都是才有前后端分离开发模式，所以一般上都没有直接返回资源页的需求了，一般上都是返回固定的响应格式，如respCode、respMsg、data，前端通过判断respCode的值进行业务判断，是弹窗还是跳转页面。

**数据校验**

在web开发时，对于请求参数，一般上都需要进行参数合法性校验的，原先的写法时一个个字段一个个去判断，这种方式太不通用了，所以java的JSR 303: Bean Validation规范就是解决这个问题的。

JSR 303只是个规范，并没有具体的实现，目前通常都是才有hibernate-validator进行统一参数校验。

JSR303定义的校验类型

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.jpg)

Hibernate Validator 附加的 constraint

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.jpg)

创建实体类

> @Data  

> @NoArgsConstructor  

> @AllArgsConstructor  

> public class DemoReq {  

>   

> @NotBlank(message="code不能为空")  

> String code;  

>   

> @Length(max=10,message="name长度不能超过10")  

> String name;  

> }  

然后在控制层方法里，加入@Valid即可，这样在访问前，会对请求参数进行检验。

> @GetMapping("/demo/valid")  

> public String demoValid(@Valid DemoReq req) {  

> return req.getCode() + "," + req.getName();  

> }  

启动，后访问http://127.0.0.1:8080/demo/valid

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

加上正确参数后，http://127.0.0.1:8080/demo/valid?code=3&name=s

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

这样数据的统一校验就完成了，对于其他注解的使用，大家可自行谷歌下，基本上都很简单的，对于已有的注解无法满足校验需要时，也可进行自定义注解的开发，一下简单讲解下，自定义注解的编写

不使用@valid的情况下，也可利用编程的方式编写一个工具类，进行实体参数校验

> public class ValidatorUtil {  

> private static Validator validator = ((HibernateValidatorConfiguration) Validation  

> .byProvider(HibernateValidator.class).configure()).failFast(true).buildValidatorFactory().getValidator();  

>   

> /**  

> * 实体校验  

> *  

> * @param obj  

> * @throws CommonException  

> */  

> public static <T> void validate(T obj) throws CommonException {  

> Set<ConstraintViolation<T>> constraintViolations = validator.validate(obj, new Class[0]);  

> if (constraintViolations.size() > 0) {  

> ConstraintViolation<T> validateInfo = (ConstraintViolation<T>) constraintViolations.iterator().next();  

> // validateInfo.getMessage() 校验不通过时的信息，即message对应的值  

> throw new CommonException("0001", validateInfo.getMessage());  

> }  

> }  

> }  

使用

> @GetMapping("/demo/valid")  

> public String demoValid(@Valid DemoReq req) {  

> //手动校验  

> ValidatorUtil.validate(req);  

> return req.getCode() + "," + req.getName();  

> }  

**自定义校验注解**

自定义注解，主要时实现ConstraintValidator的处理类即可，这里已编写一个校验常量的注解为例：参数值只能为特定的值。

自定义注解

> @Documented  

> //指定注解的处理类  

> @Constraint(validatedBy = {ConstantValidatorHandler.class })  

> @Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER })  

> @Retention(RUNTIME)  

> public @interface Constant {  

>   

> String message() default "{constraint.default.const.message}";  

>   

> Class<?>[] groups() default {};  

>   

> Class<? extends Payload>[] payload() default {};  

>   

> String value();  

> }  

注解处理类

> public class ConstantValidatorHandler implements ConstraintValidator<Constant, String> {  

>   

> private String constant;  

>   

> @Override  

> public void initialize(Constant constraintAnnotation) {  

> //获取设置的字段值  

> this.constant = constraintAnnotation.value();  

> }  

>   

> @Override  

> public boolean isValid(String value, ConstraintValidatorContext context) {  

> //判断参数是否等于设置的字段值，返回结果  

> return constant.equals(value);  

> }  

> }  

使用

> @Constant(message = "verson只能为1.0",value="1.0")  

> String version;  

运行：

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)
此时，自定义注解已生效。大家可根据实际需求进行开发。

大家看到在校验不通过时，返回的异常信息是不友好的，此时可利用统一异常处理，对校验异常进行特殊处理，特别说明下，对于异常处理类,共有以下几种情况(被@RequestBody和@RequestParam注解的请求实体，校验异常类是不同的)

> @ExceptionHandler(MethodArgumentNotValidException.class)  

> public Map<String,Object> handleBindException(MethodArgumentNotValidException ex) {  

> FieldError fieldError = ex.getBindingResult().getFieldError();  

> log.info("参数校验异常:{}({})", fieldError.getDefaultMessage(),fieldError.getField());  

> Map<String,Object> result = new HashMap<String,Object>();  

> result.put("respCode", "01002");  

> result.put("respMsg", fieldError.getDefaultMessage());  

> return result;  

> }  

>   

>   

> @ExceptionHandler(BindException.class)  

> public Map<String,Object> handleBindException(BindException ex) {  

> //校验 除了 requestbody 注解方式的参数校验 对应的 bindingresult 为 BeanPropertyBindingResult  

> FieldError fieldError = ex.getBindingResult().getFieldError();  

> log.info("必填校验异常:{}({})", fieldError.getDefaultMessage(),fieldError.getField());  

> Map<String,Object> result = new HashMap<String,Object>();  

> result.put("respCode", "01002");  

> result.put("respMsg", fieldError.getDefaultMessage());  

> return result;  

> }  

启动后，提示就友好了

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.png)

所以统一异常还是很有必要的。

**总结**

本章节主要是阐述了统一异常处理和数据的合法性校验，同时简单实现了一个自定义的注解类，大家在碰见已有注解无法解决时，可通过自定义的形式进行，当然对于通用而已，利用@Pattern(正则表达式)基本上都是可以实现的。

**最后**

目前互联网上很多大佬都有springboot系列教程，如有雷同，请多多包涵了。本文是作者在电脑前一字一句敲的，每一步都是实践的。若文中有所错误之处，还望提出，谢谢。

**系列**

* [SpringBoot | 第一章：第一个 SpringBoot 应用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481504&amp;idx=2&amp;sn=e0cf1bf8b7400a19236a1d0ac5eda2b2&amp;chksm=bd2509df8a5280c98d09b564aa19d0f75014837d842a5d13386389ee514903a4e46cc7104628&amp;scene=21#wechat_redirect)

* [SpringBoot | 第二章：lombok 介绍及简单使用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481512&amp;idx=3&amp;sn=8012746719f148d89a57a281ec7e5dda&amp;chksm=bd2509d78a5280c1111bc16859324143324e7932a6609fbff2a5e8604eb284ba0556360a69d0&amp;scene=21#wechat_redirect)

* [SpringBoot | 第三章：springboot 配置详解](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481517&amp;idx=2&amp;sn=e545ff871587a33c13c8d5cbe1276e18&amp;chksm=bd2509d28a5280c428462a3c91b56189ecfc503ec10cb1bcd96bd0f3047eebbf5695aea71e1c&amp;scene=21#wechat_redirect)

* [SpringBoot | 第四章：日志管理](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481528&amp;idx=3&amp;sn=7673a32c148b6f06dc1205e1b83280f9&amp;chksm=bd2509c78a5280d1eb847ef70ced5fabc8cff71b4691164be05407a3101de05598c3c6cfbb64&amp;scene=21#wechat_redirect)

* [SpringBoot | 第五章：多环境配置](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481532&amp;idx=2&amp;sn=2b03db4e828d5220702771c028048c4f&amp;chksm=bd2509c38a5280d5478d153766bb762b16c5552ca1dd047d62783259a2cac95d54d8b25fb7d6&amp;scene=21#wechat_redirect)

* [SpringBoot | 第六章：常用注解介绍及简单使用](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481539&amp;idx=3&amp;sn=efac2766e0252006a2ea9cdd97cf3b97&amp;chksm=bd2509bc8a5280aa3d6206cef52fd9487b8d914f453185985571251ec8973ba1fbe4fd2a91dd&amp;scene=21#wechat_redirect)

* [SpringBoot | 第七章：过滤器、监听器、拦截器](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481544&amp;idx=3&amp;sn=44046a4afbe1056799a312228d570aaf&amp;chksm=bd2509b78a5280a10eb6f54e0861ae1c5becf3b5ab8044020690082985787ff387b7cfebe738&amp;scene=21#wechat_redirect)

【关于投稿】

如果大家有原创好文投稿，请直接给公号发送留言。

① 留言格式：
【投稿】+《 文章标题》+ 文章链接

② 示例：
【投稿】《不要自称是程序员，我十多年的 IT 职场总结》：http://blog.jobbole.com/94148/

③ 最后请附上您的个人简介哈~

看完本文有收获？请转发分享给更多人

**关注「ImportNew」，提升Java技能**

![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/640.jpg)

[文章转载自公众号](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481550&amp;idx=2&amp;sn=d54365e5936724285fb61b9129ddfc11&amp;chksm=bd2509b18a5280a72e1babe26271592a15bb1afa99925a643bf4d2692a294b5fe7208bb7c957&amp;mpshare=1&amp;scene=1&amp;srcid=0812Uw711lryEusXu6ADflST##)
![](SpringBoot%20%7C%20%E7%AC%AC%E5%85%AB%E7%AB%A0%EF%BC%9A%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E3%80%81%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%A4%84%E7%90%86/0.jpg)
[一枚趔趄的猿](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481550&amp;idx=2&amp;sn=d54365e5936724285fb61b9129ddfc11&amp;chksm=bd2509b18a5280a72e1babe26271592a15bb1afa99925a643bf4d2692a294b5fe7208bb7c957&amp;mpshare=1&amp;scene=1&amp;srcid=0812Uw711lryEusXu6ADflST##)

http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651481550&idx=2&sn=d54365e5936724285fb61b9129ddfc11&chksm=bd2509b18a5280a72e1babe26271592a15bb1afa99925a643bf4d2692a294b5fe7208bb7c957&mpshare=1&scene=1&srcid=0812Uw711lryEusXu6ADflST#rd