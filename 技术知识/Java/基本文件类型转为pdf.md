# 基本文件类型转为pdf

* windows下office文件转pdf

> package com.yonyou.cloudrecord.transfile.service.impl;
>
> import com.jacob.activeX.ActiveXComponent;  
> import [com.jacob.com](http://com.jacob.com/).Dispatch;  
> import com.yonyou.cloudrecord.common.exception.BusinessRuntimeException;  
> import com.yonyou.cloudrecord.common.response.ICommonResponse;  
> import com.yonyou.cloudrecord.transfile.entity.FileType;
>
> import org.slf4j.Logger;  
> import org.slf4j.LoggerFactory;  
> import org.springframework.stereotype.Component;
>
> import java.io.File;
>
> /\*\*
>
> * Windows操作系统下，使用jaco 在 win32 下用 Java 来操作 COM 组件 时把office\(word,ppt,excel\)转换成pdf 此种方法转换成的pdf失真率很低  
> * 但是，由于使用时需要和一个dll文件 配合使用，导致在linux系统下不能通用  
> * * Created by yangbao on 2017/5/10.  
> * \*/  
>
>   @Component  
>
>   public class WindowsOfficeToPdfService extends BaseFileToPdfService {  
>
> private static final Logger logger = LoggerFactory.getLogger\(WindowsOfficeToPdfService.class\);  
> private static final int wdFormatPDF = 17;  
> private static final int xlTypePDF = 0;  
> private static final int ppSaveAsPDF = 32;
>
> @Override  
> public void fileToPdf\(File tempSourceFile, File tempPdfFile\) throws BusinessRuntimeException {
>
> logger.debug\("windows下office转pdf开始"\);  
> // 获取文件名后缀  
> String fileName = tempSourceFile.getName\(\).toLowerCase\(\);  
> String type =  
> fileName.substring\(fileName.lastIndexOf\("."\) + 1, fileName.length\(\)\).toLowerCase\(\);  
> // 判断类型选择调用的转换方法  
> if \(type.equals\(FileType.DOC\) \|\| type.equals\(FileType.DOCX\)\) {  
> this.wordToPdf\(tempSourceFile.getAbsolutePath\(\), tempPdfFile.getAbsolutePath\(\)\);  
> }  
> if \(type.equals\(FileType.PPT\) \|\| type.equals\(FileType.PPTX\)\) {  
> this.pptToPdf\(tempSourceFile.getAbsolutePath\(\), tempPdfFile.getAbsolutePath\(\)\);  
> }  
> if \(type.equals\(FileType.XLS\) \|\| type.equals\(FileType.XLSX\)\) {  
> this.excelToPdf\(tempSourceFile.getAbsolutePath\(\), tempPdfFile.getAbsolutePath\(\)\);  
> }  
> logger.debug\("windows下文件转pdf完成"\);  
> }
>
> /\*\*
>
> * ppt文件转pdf服务  
>
>   \*/  
>
>   private void pptToPdf\(String pptFile, String pdfFile\) {  
>
>   try {  
>
>   ActiveXComponent app = new ActiveXComponent\("PowerPoint.Application"\);  
>
>   // app.setProperty\("Visible",false\);  
>
>   Dispatch ppts = app.getProperty\("Presentations"\).toDispatch\(\);  
>
>   Dispatch ppt = Dispatch.call\(ppts, "Open", pptFile, true, true, false\).toDispatch\(\);  
>
>   Dispatch.call\(ppt, "SaveAs", pdfFile, ppSaveAsPDF\);  
>
>   Dispatch.call\(ppt, "Close"\);  
>
>   app.invoke\("Quit"\);  
>
>   } catch \(Exception e\) {  
>
>   logger.error\("ppt转pdf失败", e.getMessage\(\)\);  
>
>   throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "ppt转pdf失败"\);  
>
>   }  
>
>   }  
>
> /\*\*
>
> * excel文件转pdf服务  
>
>   \*/  
>
>   private void excelToPdf\(String excelFile, String pdfFile\) {  
>
>   try {  
>
>   // 打开excel应用程序  
>
>   ActiveXComponent app = new ActiveXComponent\("Excel.Application"\);  
>
>   // 设置excel不可见  
>
>   app.setProperty\("Visible", false\);  
>
>   // 打开每一个excel  
>
>   Dispatch excels = app.getProperty\("Workbooks"\).toDispatch\(\);  
>
>   Dispatch excel = Dispatch.call\(excels, "Open", excelFile, false, true\).toDispatch\(\);  
>
>   Dispatch.call\(excel, "ExportAsFixedFormat", xlTypePDF, pdfFile\);  
>
>   Dispatch.call\(excel, "Close", false\);  
>
>   app.invoke\("Quit"\);  
>
>   } catch \(Exception e\) {  
>
>   logger.error\("excel转pdf失败", e.getMessage\(\)\);  
>
>   throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "excel转pdf失败"\);  
>
>   }  
>
>   }  
>
> /\*\*
>
> * word文件转pdf服务  
>
>   \*/  
>
>   private void wordToPdf\(String wordFile, String pdfFile\) {  
>
>   try {  
>
>   // 打开word应用程序  
>
>   ActiveXComponent app = new ActiveXComponent\("Word.Application"\);  
>
>   // 设置word不可见  
>
>   app.setProperty\("Visible", false\);  
>
>   // 调用word所有打开的文档，返回documents对象  
>
>   Dispatch documents = app.getProperty\("Documents"\).toDispatch\(\);  
>
>   // 调用documents对象的open方法，打开每一个document对象  
>
>   Dispatch doc = Dispatch.call\(documents, "Open", wordFile, false, true\).toDispatch\(\);  
>
>   // 调用document的saveas方法，将文档保存为pdf  
>
>   Dispatch.call\(doc, "ExportAsFixedFormat", pdfFile, wdFormatPDF\);  
>
>   // 关闭文档  
>
>   Dispatch.call\(doc, "Close", false\);  
>
>   // 关闭word应用程序  
>
>   app.invoke\("Quit", 0\);  
>
>   } catch \(Exception e\) {  
>
>   logger.error\("word转pdf失败", e.getMessage\(\)\);  
>
>   throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "word转pdf失败"\);  
>
>   }  
>
> }
>
> }

* Linux下office转pdf

> package com.yonyou.cloudrecord.transfile.service.impl;
>
> import com.artofsolving.jodconverter.DocumentConverter;  
> import com.artofsolving.jodconverter.openoffice.connection.OpenOfficeConnection;  
> import com.artofsolving.jodconverter.openoffice.connection.SocketOpenOfficeConnection;  
> import com.artofsolving.jodconverter.openoffice.converter.OpenOfficeDocumentConverter;  
> import com.yonyou.cloudrecord.common.exception.BusinessRuntimeException;  
> import com.yonyou.cloudrecord.common.response.ICommonResponse;
>
> import org.slf4j.Logger;  
> import org.slf4j.LoggerFactory;  
> import org.springframework.stereotype.Component;
>
> import java.io.File;
>
> /\*\*
>
> * Linux下office文件转pdf  
> * linux环境下，由于不能和windows环境一样，处理dll文件， 所以通过jacob调用com的方法行不通 此处使用openoffice的方法在linux环境下，把office文件  
> * \(word、ppt、excel\)转换成pdf文件  
> * Created by yangbao on 2017/5/10.  
>
>   \*/  
>
>   @Component  
>
>   public class LinuxOfficeToPdfService extends BaseFileToPdfService {  
>
> private Logger logger = LoggerFactory.getLogger\(LinuxOfficeToPdfService.class\);
>
> private static final String OPENOFFICEHOME = "openofficehome";  
> private static final String COMMAND = "command";  
> private static final String HOST = "host";  
> private static final String PORT = "port";
>
> @Override  
> public void fileToPdf\(File tempSourceFile, File tempPdfFile\) throws BusinessRuntimeException {  
> logger.debug\("Linux下office转pdf开始"\);  
> // 调用openoffice转换  
> this.officeToPdf\(tempSourceFile.getAbsolutePath\(\), tempPdfFile.getAbsolutePath\(\)\);  
> logger.debug\("Linux下office转pdf完成"\);  
> }
>
> /\*\*
>
> * Linux下将office转为pdf  
>
>   \*/  
>
>   private void officeToPdf\(String tempOfficeFile, String tempPdfFile\) {  
>
> try {  
> // 这里是OpenOffice的安装目录  
> String OpenOffice\_HOME = OPENOFFICEHOME;  
> if \(OpenOffice\_HOME.charAt\(OpenOffice\_HOME.length\(\) - 1\) != '\'\) {  
> OpenOffice\_HOME += "\";  
> }
>
> // 启动OpenOffice的服务  
> String command = OpenOffice\_HOME + COMMAND;  
> Process pro = Runtime.getRuntime\(\).exec\(command\);  
> // connect to an OpenOffice.org instance running on port 8100  
> OpenOfficeConnection connection =  
> new SocketOpenOfficeConnection\(HOST, Integer.parseInt\(PORT\)\);  
> connection.connect\(\);
>
> // convert  
> DocumentConverter converter = new OpenOfficeDocumentConverter\(connection\);  
> converter.convert\(new File\(tempOfficeFile\), new File\(tempPdfFile\)\);
>
> // close the connection  
> connection.disconnect\(\);  
> // 关闭OpenOffice服务的进程  
> pro.destroy\(\);  
> } catch \(Exception e\) {  
> logger.error\("Linux下office转pdf失败", e.getMessage\(\)\);  
> throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "Linux下office转pdf失败"\);  
> }
>
> }  
> }

* txt文件转为pdf

> package com.yonyou.cloudrecord.transfile.service.impl;
>
> import com.itextpdf.text.Document;  
> import com.itextpdf.text.Element;  
> import com.itextpdf.text.Font;  
> import com.itextpdf.text.PageSize;  
> import com.itextpdf.text.Paragraph;  
> import com.itextpdf.text.pdf.BaseFont;  
> import com.itextpdf.text.pdf.PdfWriter;  
> import com.yonyou.cloudrecord.common.exception.BusinessRuntimeException;  
> import com.yonyou.cloudrecord.common.response.ICommonResponse;  
> import com.yonyou.cloudrecord.transfile.utils.Charsetutil;
>
> import org.slf4j.Logger;  
> import org.slf4j.LoggerFactory;  
> import org.springframework.stereotype.Component;
>
> import java.io.BufferedReader;  
> import java.io.File;  
> import java.io.FileInputStream;  
> import java.io.FileOutputStream;  
> import java.io.IOException;  
> import java.io.InputStreamReader;
>
> /\*\*
>
> * Txt转Pdf， 注意中文乱码，与操作系统无关  
> * * Created by yangbao on 2017/5/10.  
> * \*/  
>
>   @Component  
>
>   public class TxtFileToPdfService extends BaseFileToPdfService {  
>
> private static final Logger logger = LoggerFactory.getLogger\(TxtFileToPdfService.class\);
>
> @Override  
> public void fileToPdf\(File tempSourceFile, File tempPdfFile\) throws BusinessRuntimeException {  
> logger.debug\("txt文件转pdf开始"\);  
> // txt转pdf  
> this.txtFileToPdf\(tempSourceFile, tempPdfFile\);  
> logger.debug\("txt文件转pdf完成"\);  
> }
>
> /\*\*
>
> * txt文件转pdf文件服务  
>
>   \*/  
>
>   private void txtFileToPdf\(File txtFile, File pdfFile\) {  
>
>   Document document = new Document\(PageSize.A4\);  
>
>   BufferedReader reader = null;  
>
>   try {  
>
>   // 初始化pdfwriter  
>
>   PdfWriter.getInstance\(document, new FileOutputStream\(pdfFile\)\);  
>
>   document.open\(\);  
>
>   // 初始化字体  
>
>   BaseFont bf = BaseFont.createFont\("STSong-Light", "UniGB-UCS2-H", BaseFont.NOT\_EMBEDDED\);  
>
>   Font font = new Font\(bf, 12, Font.NORMAL\);  
>
>   // reader 设置GB2312才能正确显示中文，utf-8不可以  
>
>   String charset = Charsetutil.GetCharset\(txtFile\);  
>
>   reader = new BufferedReader\(new InputStreamReader\(new FileInputStream\(txtFile\), charset\)\);  
>
>   String strLine = null;  
>
>   while \(\(strLine = reader.readLine\(\)\) != null\) {  
>
>   Paragraph p = new Paragraph\(strLine, font\);  
>
>   p.setAlignment\(Element.ALIGN\_JUSTIFIED\);  
>
>   document.add\(p\);  
>
>   }  
>
>   document.close\(\);  
>
>   } catch \(Exception e\) {  
>
>   logger.error\("txt转pdf失败", e.getMessage\(\)\);  
>
>   throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "txt文件转pdf失败"\);  
>
>   } finally {  
>
>   if \(reader != null\) {  
>
>   try {  
>
>   reader.close\(\);  
>
>   } catch \(IOException e\) {  
>
>   logger.error\("txt转pdf文件流关闭失败", e.getMessage\(\)\);  
>
>   throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "txt文件转pdf失败"\);  
>
>   }  
>
>   }  
>
>   }  
>
>   }  
>
>   }

图片转为pdf /\*\*

* 合并图片,参考旧会计档案系统的文件合并方法

  \*

* @param files
* @return

  \*/

  private byte\[\] mergePicturesToPdf\(Map files\) {

  Image\[\] images = new Image\[files.size\(\)\];

  Document doc = new Document\(PageSize.A4, 20, 20, 20, 20\);

  byte\[\] buffer = null;

  ByteArrayOutputStream bos = null;

  try {

  List keyList = new ArrayList&lt;&gt;\(\);

  for \(String key : files.keySet\(\)\) {

  keyList.add\(key\);

  }

  // 获取图片image数组

  for \(int i = 0; i &lt; keyList.size\(\); i++\) {

  byte\[\] bytes = FdfsPoolClient.getInstance\(\).download\(files.get\(keyList.get\(i\)\)\);

  images\[i\] = Image.getInstance\(bytes\);

  }

  bos = new ByteArrayOutputStream\(\);

  PdfWriter.getInstance\(doc, bos\);

  // 打开文档

  doc.open\(\);

  // 图片循环插入文档

  for \(Image image : images\) {

  // 设置图片比例

  float heigth = image.getHeight\(\);

  float width = image.getWidth\(\);

  // int percent = getPercent2\(heigth, width\);

  int percent = getPercent\(heigth, width\);

  image.scalePercent\(percent\);

  // 设置图片的绝对位置

  image.setAlignment\(Image.MIDDLE\);

  // 插入图片

  doc.add\(image\);

  // 设置图片之间间隔

  doc.add\(new Paragraph\(" "\)\);

  }

  // 关闭文档

  doc.close\(\);

  buffer = bos.toByteArray\(\);

  } catch \(Exception e\) {

  throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "图片合并pdf异常,请联系管理员！"\);

  } finally {

  if \(bos != null\) {

  try {

  bos.close\(\);

  } catch \(IOException e\) {

  throw new BusinessRuntimeException\(ICommonResponse.FAIL\_CODE, "图片合并pdf异常,请联系管理员！"\);

  }

  }

  }

  return buffer;

  }

```text
/**
* 第一种解决方案 在不改变图片形状的同时，判断，如果h>w，则按h压缩，否则在w>h或w=h的情况下，按宽度压缩
*
* @param h
* @param w
* @return
*/

public static int getPercent(float h, float w) {
  int p = 0;
  float p2 = 0.0f;
  if (h > w) {
    p2 = 742 / h * 100;
  } else {
    p2 = 530 / w * 100;
  }
  p = Math.round(p2);
  return p;
}
```

about:blank

