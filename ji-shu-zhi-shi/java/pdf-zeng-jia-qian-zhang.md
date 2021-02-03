# pdf增加签章

public String exportSign\(String fileId, Long corpId, String info, String startDate, String endDate\){ try { //获取pdf文件内容 byte\[\] fileContent = FdfsPoolClient.getInstance\(\).download\(fileId\); //获取签证证书文件 RecFile recFile = new RecFile\(\); recFile.setCorpId\(corpId\); byte\[\] fileConfig = getSinconfig\(recFile\); / __ \*/ if \(StringUtils.isBlank\(info\)\) { logger.error\("签章原因不能为空！请检查"\); ExceptionUtils.wrapBusiException\("签章原因不能为空！请检查"\); } drawSignet.drawSignet\(info, startDate, endDate, 270, 70\); if \(fileContent == null \|\| fileContent.length == 0\) { return fileId; } PdfReader pdfReader = null; KeyStore ks = null; String alias = null; PrivateKey key = null; Certificate\[\] chain = null; ByteArrayOutputStream fos = null; PdfStamper stp = null; PdfSignatureAppearance sap = null; Image signatureFieldImage = null; PrivateKeySignature es = null; StringBuffer stringBuffer = new StringBuffer\(System.getProperty\("user.dir"\)\); try { pdfReader = new PdfReader\(fileContent\); ks = KeyStore.getInstance\(PKCS12\); // 到时把.p12文件以及密码存储到redis里面 // ks.load\(new FileInputStream\("D:\erecord\dlt.p12"\), ksPassword.toCharArray\(\)\); // System.out.println\(x\);

ks = KeyStore.getInstance\(PKCS12\); ByteArrayInputStream inputStream = new ByteArrayInputStream\(fileConfig\); ks.load\(inputStream, SignConfigGenerate.KEY\_PASSWD\); key = \(PrivateKey\) ks.getKey\(SignConfigGenerate.ROOT\_ALIAS, SignConfigGenerate.KEY\_PASSWD\); chain = ks.getCertificateChain\(SignConfigGenerate.ROOT\_ALIAS\); fos = new ByteArrayOutputStream\(fileContent.length\); stp = PdfStamper.createSignature\(pdfReader, fos, '\0', null, true\); sap = stp.getSignatureAppearance\(\); sap.setReason\(info\); sap.setLocation\("companyName"\); stringBuffer.append\(File.separator\); stringBuffer.append\("seal.png"\); signatureFieldImage = Image.getInstance\(stringBuffer.toString\(\)\); sap.setSignatureGraphic\(signatureFieldImage\); sap.setRenderingMode\(PdfSignatureAppearance.RenderingMode.GRAPHIC\); // comment next line to have an invisible signature float width = getPdfWidth\(fileContent\); float height = getPdfHeight\(fileContent\); float centerX = width / 2; float centerY = height / 2; sap.setVisibleSignature\( new Rectangle\(centerX - 135f, centerY - 35, centerX + 135f, centerY + 35\), 1, null\); sap.setCertificationLevel\(PdfSignatureAppearance.NOT\_CERTIFIED\); es = new PrivateKeySignature\(key, SHA1, null\); BouncyCastleDigest bouncyCastleDigest = new BouncyCastleDigest\(\); MakeSignature.signDetached\(sap, bouncyCastleDigest, es, chain, null, null, null, 0, MakeSignature.CryptoStandard.CMS\); stp.close\(\); } catch \(Exception e\) { logger.error\(e.getMessage\(\)\); ExceptionUtils.marshException\(e\); } finally { // 删除临时文件seal.png deleteTempFile\(new File\(stringBuffer.toString\(\)\)\); } String upload = FdfsPoolClient.getInstance\(\).upload\(fos.toByteArray\(\), "temp123456789"\); return upload; } catch \(Exception e\) { ExceptionUtils.marshException\(e\); } return null; } 相关方法

private float getPdfWidth\(byte\[\] sourcePdf\) throws Exception { PdfReader reader = new PdfReader\(sourcePdf\); Rectangle mediabox = reader.getPageSize\(1\); return mediabox.getRight\(\); }

private float getPdfHeight\(byte\[\] sourcePdf\) throws Exception { PdfReader reader = new PdfReader\(sourcePdf\); Rectangle mediabox = reader.getPageSize\(1\); return mediabox.getTop\(\); } public void deleteTempFile\(File file\) { if \(file != null && file.exists\(\)\) { if \(!file.delete\(\)\) { logger.error\("非pdf转pdf文件时，服务器上临时文件删除失败！"\); } }} }

签章图片的生成： package com.yonyou.cloudrecord.sin.service.impl;

import com.yonyou.cloudrecord.common.utils.ExceptionUtils;

import org.apache.commons.lang3.StringUtils; import org.slf4j.Logger; import org.slf4j.LoggerFactory; import org.springframework.stereotype.Component;

import java.awt.\*; import java.awt.geom.Rectangle2D; import java.awt.image.BufferedImage; import java.io.File;

import javax.imageio.ImageIO;

@Component public class DrawSignet {

private final Logger logger = LoggerFactory.getLogger\(getClass\(\)\);

private static final float BASICSTROKEWIDTH = 2; // 画笔的粗度

/_\*_ 

* @param content 改签张对外的用途
* @param startDate 有效期起始日期 例如：2017-03-01
* @param endDate 有效期截止日期 例如：2017-03-14
* @param canvasWidth 400
* @param canvasHeight 200
* @return

  \*/

  public BufferedImage drawSignet\(String content, String startDate, String endDate, int canvasWidth,

  int canvasHeight\) {

  if \(content == null \|\| StringUtils.isEmpty\(content.trim\(\)\)\) {

  logger.error\("签章原因不能为空！请检查"\);

  ExceptionUtils.wrapBusiException\("签章原因不能为空！请检查"\);

  }

  BufferedImage bi = new BufferedImage\(canvasWidth, canvasHeight, BufferedImage.TYPE\_INT\_ARGB\);

  Graphics2D g2d = bi.createGraphics\(\);

  // 增加下面代码使得背景透明

  bi = g2d.getDeviceConfiguration\(\).createCompatibleImage\(canvasWidth, canvasHeight,

  Transparency.TRANSLUCENT\);

  g2d.dispose\(\);

  g2d = bi.createGraphics\(\);

  // 背景透明代码结束

  // 设置画笔

  // g2d.setPaint\(Color.WHITE\);

  // g2d.fillRect\(0, 0, canvasWidth, canvasWidth\);

/_**\***_ draw rectangle **\***/ // float alpha = 0.1f; // 透明度 // g2d.setComposite\(AlphaComposite.getInstance\(AlphaComposite.SRC\_ATOP, // alpha\)\); g2d.setColor\(new Color\(255, 0, 0\)\); g2d.setStroke\(new BasicStroke\(BASICSTROKEWIDTH\)\);// 设置画笔的粗度 Shape rectangle = new Rectangle2D.Double\(0, 0, canvasWidth, canvasHeight\); g2d.draw\(rectangle\); /_**\*\***_/

/_**\***_ draw line _**\***_/ // g2d.drawLine\(10, canvasHeight / 2, canvasWidth - 10, canvasHeight / // 2\);// /_**\***_ END _**\*\***_/

// /**\*** draw string **\*\***/ int fontSize = 13; Font f = new Font\("宋体", Font.PLAIN, fontSize\); g2d.setFont\(f\); g2d.drawString\(" 此件与原件相符，仅用于", 0, \(canvasHeight / 2\) -16\); // g2d.setPaint\(Color.GRAY\); // g2d.drawLine\(40, \(canvasHeight / 2\) - 30, canvasWidth - 40, // \(canvasHeight / 2\) - 30\); // g2d.drawLine\(40, \(canvasHeight / 2\) - 30, 40, \(canvasHeight / 2\) - // 5\); // g2d.drawLine\(40, \(canvasHeight / 2\) - 5, canvasWidth - 40, // \(canvasHeight / 2\) - 5\); // g2d.drawLine\(canvasWidth - 40, \(canvasHeight / 2\) - 30, canvasWidth - // 40, \(canvasHeight / 2\) - 5\); // g2d.setPaint\(Color.red\); /_\* // 把content分成两行，第一行8个字符，第二行放剩余的全部 if \(content.length\(\) &lt;= 8\) { g2d.drawString\(content, 142, \(canvasHeight / 2\) - 12\); } else { g2d.drawString\(content.substring\(0, 8\), 142, \(canvasHeight / 2\) - 12\); g2d.drawString\(content.substring\(8, content.length\(\)\), 0, \(canvasHeight / 2\) + 2\); }_/ StringBuffer stringBuffer = new StringBuffer\(" "\); stringBuffer.append\(content\); stringBuffer.append\(","\); g2d.drawString\(stringBuffer.toString\(\), 0, \(canvasHeight / 2\) -2\); g2d.drawString\(" 其它无效。", 0, \(canvasHeight / 2\) + 12\); g2d.drawString\(" 有效期：" + startDate + "至" + endDate, 0, \(canvasHeight / 2\) + 26\); // String\[\] stringArray1 = processDate\(startDate\); // String\[\] stringArray2 = processDate\(endDate\); // g2d.drawString\( // " 有效期：" + stringArray1\[0\] + "年" + stringArray1\[1\] + "月" + stringArray1\[2\] + "号至" // + stringArray2\[0\] + "年" + stringArray2\[1\] + "月" + stringArray2\[2\] + "号", // 0, \(canvasHeight / 2\) + 26\); g2d.dispose\(\);// 销毁资源 // "D:\erecord\seal.png"; stringBuffer = new StringBuffer\(System.getProperty\("user.dir"\)\); stringBuffer.append\(File.separator\); stringBuffer.append\("seal.png"\); try { ImageIO.write\(bi, "PNG", new File\(stringBuffer.toString\(\)\)\); } catch \(Exception e\) { logger.error\(e.getMessage\(\)\); ExceptionUtils.marshException\(e\); } return bi; }

private static String\[\] processDate\(String date\) { if \(date == null \|\| StringUtils.isEmpty\(date.trim\(\)\)\) { return null; } return date.split\("-"\); }

// /_\* //_  改变png图片的透明度 //  _//_  @param srcImageFile //  _源图像地址 //_  @param descImageDir //  _输出图片的路径和名称 //_  @param alpha //  _输出图片的透明度1-10 //_ / // private void setAlpha\(String srcImageFile, String descImageDir, int alpha\) { // // try { // // 读取图片 // FileInputStream stream = new FileInputStream\(new File\(srcImageFile\)\);// 指定要读取的图片 // // // 定义一个字节数组输出流，用于转换数组 // ByteArrayOutputStream os = new ByteArrayOutputStream\(\); // // byte\[\] data = new byte\[1024\];// 定义一个1K大小的数组 // while \(stream.read\(data\) != -1\) { // os.write\(data\); // } // // ImageIcon imageIcon = new ImageIcon\(os.toByteArray\(\)\); // BufferedImage bufferedImage = new BufferedImage\(imageIcon.getIconWidth\(\), // imageIcon.getIconHeight\(\), // BufferedImage.TYPE\_4BYTE\_ABGR\); // Graphics2D g2D = \(Graphics2D\) bufferedImage.getGraphics\(\); // g2D.drawImage\(imageIcon.getImage\(\), 0, 0, imageIcon.getImageObserver\(\)\); // // // 判读透明度是否越界 // if \(alpha &lt; 0\) { // alpha = 0; // } else if \(alpha &gt; 10\) { // alpha = 10; // } // // // 循环每一个像素点，改变像素点的Alpha值 // for \(int j1 = bufferedImage.getMinY\(\); j1 &lt; bufferedImage.getHeight\(\); j1++\) { // for \(int j2 = bufferedImage.getMinX\(\); j2 &lt; bufferedImage.getWidth\(\); j2++\) { // int rgb = bufferedImage.getRGB\(j2, j1\); // rgb = \(\(alpha \* 255 / 10\) &lt;&lt; 24\) \| \(rgb & 0x00FFFFFF\); // bufferedImage.setRGB\(j2, j1, rgb\); // } // } // g2D.drawImage\(bufferedImage, 0, 0, imageIcon.getImageObserver\(\)\); // // // 生成图片为PNG // // bufferedImage.se // ImageIO.write\(bufferedImage, "png", new File\(descImageDir\)\); // // } catch \(Exception e\) { // e.printStackTrace\(\); // } // // }

// public static void main\(String\[\] args\) { // int canvasWidth = 400; // int canvasHeight = 200; // String savepath = "D:\seal.png"; // String content = "新华书店对外借阅"; // BufferedImage image = DrawSignet.drawSignet\(content, "2017-03-01", // "2017-03-14", canvasWidth, canvasHeight\); // try { // ImageIO.write\(image, "PNG", new File\(savepath\)\); // } catch \(IOException e\) { // e.printStackTrace\(\); // } // } }

about:blank

