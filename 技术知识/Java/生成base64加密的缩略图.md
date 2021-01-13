# 生成base64加密的缩略图
// 获取图片的byte[]数组
// 需要修改
byte[] content = null;
File file = new File("D:/a.pdf");
FileInputStream in = new FileInputStream(file);
ByteArrayOutputStream bos = new ByteArrayOutputStream(1024);
byte[] b = new byte[1024];
int n;
while ((n = in.read(b)) != -1) {
bos.write(b, 0, n);
}
in.close();
bos.close();
content = bos.toByteArray();
BufferedImage bImage11 =ImageIO.read(file);
InputStream buffin = new ByteArrayInputStream(content, 0, content.length);
BufferedImage bImage = ImageIO.read(buffin);
int height = bImage.getHeight() / 4;
int width = bImage.getWidth() / 4;
BufferedImage newBImage = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
newBImage.getGraphics().drawImage(bImage, 0, 0, width, height, null);
ByteArrayOutputStream out = new ByteArrayOutputStream();
ImageIO.write(newBImage, "jpg", out);
byte[] smallContent = out.toByteArray();
String data = Base64.encodeToString(smallContent);

生成base64的二维码：

```
private String setQRCode(String content) throws WriterException, IOException {
    // 二维码的图片格式
```

// HashMap<EncodeHintType, String> hints = new HashMap<>();
// 用于设置QR二维码参数
Hashtable<EncodeHintType, Object> hints = new Hashtable<EncodeHintType, Object>();
// 设置QR二维码的纠错级别——这里选择M级别
hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M);
// 设置编码方式
hints.put(EncodeHintType.CHARACTER_SET, "UTF-8");
BitMatrix bitMatrix =
new MultiFormatWriter().encode(content, BarcodeFormat.QR_CODE, 400, 400, hints);
BufferedImage srcImage =
MatrixToImageWriter.toBufferedImage(bitMatrix, new MatrixToImageConfig());
ByteArrayOutputStream out = new ByteArrayOutputStream();
ImageIO.write(srcImage, "png", out);
byte[] data = out.toByteArray();
return Base64.encodeBase64String(data);
}