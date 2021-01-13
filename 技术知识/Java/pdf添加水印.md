# pdf添加水印
/**
* PDF添加水印
*/
private ByteArrayOutputStream addWatermark(byte[] bytes, String watermarkText) {

ByteArrayInputStream fis = new ByteArrayInputStream(bytes);
ByteArrayOutputStream fos = new ByteArrayOutputStream();
//  创建一个pdf读入流
PdfReader reader = null;
try {
reader = new PdfReader(fis);
} catch (IOException e) {
logger.error("读取pdf失败.", e);
return null;
}
//  根据一个pdfreader创建一个pdfStamper.用来生成新的pdf.
PdfStamper stamper = null;
try {
stamper = new PdfStamper(reader, fos);
} catch (FileNotFoundException e) {
logger.error("输出文件加载失败失败.", e);
return null;
} catch (DocumentException e) {
logger.error("读取pdf失败.", e);
return null;
} catch (IOException e) {
logger.error("输出文件IO异常.", e);
return null;
}

BaseFont stSongLight = null;
try {

stSongLight = BaseFont.createFont("STSong-Light", "UniGB-UCS2-H", false);

// stSongLight = BaseFont.createFont("/COUR.TTF", BaseFont.IDENTITY_H, BaseFont.EMBEDDED);
// 设置字体
// stSongLight = BaseFont.createFont("STSong-Light", "UniGB-UCS2-H", BaseFont.EMBEDDED);

} catch (DocumentException e) {
logger.error("字体文件加载失败。", e);
return null;
} catch (IOException e) {
logger.error("字体文件读取失败。", e);
return null;
}
PdfGState gs = new PdfGState();
gs.setFillOpacity(0.4f);
gs.setStrokeOpacity(0.4f);
PdfContentByte over = null;
int total = reader.getNumberOfPages() + 1;
Rectangle pageRect = stamper.getReader().getPageSizeWithRotation(1);
//  计算水印X,Y坐标
float x1 = pageRect.getWidth() / 2;
float y1 = pageRect.getHeight() * 3 / 4;
for (int i = 1; i < total; i++) {
over = stamper.getOverContent(i);
//  开始写入文本
over.beginText();
over.saveState();
//  set Transparency
over.setGState(gs);
over.setColorFill(BaseColor.GRAY);
over.setFontAndSize(stSongLight, 30);
//  水印文字成45度角倾斜
over.showTextAligned(Element.ALIGN_CENTER, watermarkText, x1, y1, 0);
over.endText();
}
try {
if (stamper != null) {
stamper.close();
}
if (reader != null) {
reader.close();
}
} catch (DocumentException e) {
logger.error("stamper关闭异常。", e);
return null;
} catch (IOException e) {
logger.error("stamper关闭IO异常。", e);
return null;
} catch (Exception e) {
logger.error("stamper关闭IO异常。", e);
return null;
}
return fos;
}

需要引入包：

```
<dependency>
<groupId>com.itextpdf</groupId>
<artifactId>itext-asian</artifactId>
<version>5.2.0</version>
</dependency>
```

about:blank