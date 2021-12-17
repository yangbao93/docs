# FastDfs文件上传（多文件的上传，文件的大小限制）

## FastDfs文件上传（多文件的上传，文件的大小限制）

## FastDfs文件上传（多文件的上传，文件的大小限制）

配置: 1.文件存储类型的配置：\(在文件上传时会出现文件解析错误\)

```text
application.properties:                fdfs_configPath=d:/EinvoiceConf/Invoiceclient/fdfs_client.conf，在该文件下修改为：tracker_server 192.168.52.99:221222:spring-mvc.xml配置bean：                <bean id="multipartResolver"         class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >          </bean>
```

上传文件有两种形式： 1:一次上传多个文件：MultipartFile\[\] files;

```text
Map<String, InputStream> inputFileMap = new Hashtable<String, InputStream>();            for (MultipartFile file : myfiles) {            InputStream inputStream = file.getInputStream();            inputFileMap.put(file.getOriginalFilename(), inputStream);            }
```

2:每次加载一个文件：HttpServletRequest request, HttpServletResponse response；

```text
CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();       MultipartHttpServletRequest multiRequest = (DefaultMultipartHttpServletRequest) request;         //获取multiRequest中的参数        String[] StringArray=multiRequest.getParameterMap().get(Object key);    if (multipartResolver.isMultipart(request)) {       Iterator<String> iter = multiRequest.getFileNames();       while (iter.hasNext()) {      // 一次遍历所有文件      MultipartFile file = multiRequest.getFile(iter.next().toString());      if (file != null) {        InputStream inputStream = file.getInputStream();         byte[] content = IOUtils.toByteArray(inputStream);       }     }   }
```

3.BASE64字符串（解析）转换成pdf文件

```text
byte[] bytes = Base64.decodeBase64(pdfFile.getContent());  ByteArrayInputStream inputStream = new ByteArrayInputStream(bytes);
```

4.判断文件的类型：

```text
/**   * 文件大小限制   */  private static final int FILE_SIZE_LIMIT = 1 * 1024 * 1024;  public void checkFileType(CommonsMultipartFile[] files) {     for (CommonsMultipartFile f : files) {      String fileName = f.getOriginalFilename();      if (!fileName.endsWith("pdf") && !fileName.endsWith("PDF")) {        BusiExceptionUtils.wrapBusiException("文件类型不正确!");      }      if (f.getSize() > FILE_SIZE_LIMIT) {        BusiExceptionUtils.wrapBusiException("文件大小超出限制!");      }    }  }
```

geng

[http://115.29.201.173/client\_zh.html\#](http://115.29.201.173/client_zh.html#)

