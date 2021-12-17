# 读取excel报错java.io.IOException: mark/reset not supported

使用java读取excel转化为workbook时时候报错:

java.io.IOException: mark/reset not supported

这个错误的原因提供的流不支持设置标记并将流重置为该标记

原代码如下:

```text
InputStream fis = new FileInputStream(new File(""));
```

WorkBook workbook = null; try { if \(POIFSFileSystem.hasPOIFSHeader\(fis\)\) { workbook = new HSSFWorkbook\(fis\); } else if \(POIXMLDocument.hasOOXMLHeader\(fis\)\) { workbook = new XSSFWorkbook\(fis\); } } catch \(IOException e\) { e.printStackTrace\(\); }

改正为如下代码即可:

InputStream inputStream = new BufferedInputStream\(new FileInputStream\(new File\(""\)\)\) about:blank

