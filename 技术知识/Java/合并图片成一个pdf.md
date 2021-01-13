# 合并图片成一个pdf
合并图片

/**
* 合并图片,参考旧会计档案系统的文件合并方法
*
* @param files
* @return
*/
private byte[] mergePicturesToPdf(Map<String, String> files) {
Image[] images = new Image[files.size()];
Document doc = new Document(PageSize.A4, 20, 20, 20, 20);
byte[] buffer = null;
ByteArrayOutputStream bos = null;
try {
List<String> keyList = new ArrayList<>();
for (String key : files.keySet()) {
keyList.add(key);
}
// 获取图片image数组
for (int i = 0; i < keyList.size(); i++) {
byte[] bytes = FdfsPoolClient.getInstance().download(files.get(keyList.get(i)));
images[i] = Image.getInstance(bytes);
}
bos = new ByteArrayOutputStream();
PdfWriter.getInstance(doc, bos);
// 打开文档
doc.open();
// 图片循环插入文档
for (Image image : images) {
// 设置图片比例
float heigth = image.getHeight();
float width = image.getWidth();
int percent = getPercent2(heigth, width);
image.scalePercent(percent);
// 设置图片的绝对位置
image.setAlignment(Image.MIDDLE);
// 插入图片
doc.add(image);
// 设置图片之间间隔
doc.add(new Paragraph(" "));
}
// 关闭文档
doc.close();
buffer = bos.toByteArray();
} catch (Exception e) {
throw new BusinessRuntimeException(ICommonResponse.FAIL_CODE, "图片合并pdf异常,请联系管理员！");
} finally {
if (bos != null) {
try {
bos.close();
} catch (IOException e) {
throw new BusinessRuntimeException(ICommonResponse.FAIL_CODE, "图片合并pdf异常,请联系管理员！");
}
}
}
return buffer;
}

/**
* 按照宽度压缩图片
*
* @param h 高度
* @param w 宽度
* @return
*/
private static int getPercent2(float h, float w) {
int p = 0;
float p2 = 0.0f;
p2 = 530 / w * 100;
p = Math.round(p2);
return p;
}

about:blank