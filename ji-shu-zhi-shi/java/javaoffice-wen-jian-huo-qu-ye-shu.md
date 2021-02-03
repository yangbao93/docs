# java--Office文件获取页数

1.导入包 ppt文件需要引入一个apache的包，如下：

org.apache.poipoi-scratchpad3.2-FINAL

2.文件页数获取

InputStream is = new FileInputStream\(path\);

2.1 word文件页数

方法一：

XWPFDocument a = new XWPFDocument\(is\); pages = a.getProperties\(\).getExtendedProperties\(\).getUnderlyingProperties\(\).getPages\(\); is.close\(\);

方法二：

WordExtractor extractor = new WordExtractor\(is\); extractor....\(待尝试\)

2.2 excel文件页数 XSSFWorkbook wb = new XSSFWorkbook\(is\); pages = wb.getNumberOfSheets\(\);// 获取页数 is.close\(\);

2.3 ppt文件页数 SlideShow ss = new SlideShow\(new HSLFSlideShow\(is\)\); // ss.getSlides\(\)是获取每一页ppt pages = ss.getSlides\(\).length; is.close\(\);

2.4 pdf文件页数

```text
PdfReader reader = new PdfReader(is);
page = reader.getNumberOfPages();
is.close();
```

about:blank

