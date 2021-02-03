# Java文件读写

file\(内存\)----输入流----&gt;【程序】----输出流----&gt;file\(内存\)

* **复制文件（文件流的方式）**

public void copyFile\(String src, String dest\){ FileInputStream in = new FileInputStream\(src\); File file = new File\(dest\); if\(!file.exists\(\)\){ file.createNewFile\(\); } FileOutputStream out = new FileOutputStream\(\); int c; byte buffer\[\] =new byte\[1024\]; while\(\(c=in.read\(buffer\)\)!=-1\){ for\(int i = 0;i&lt;c;i++\){ out.write\(buffer\[i\]\)

}

} in.close; out.close; }

* **FileInputStream读取文件**

File file = new file\(path\); FileInputStream in = new FileInputStream\(file\); byte\[\] buf = new byte\[1024\]; StringBuffer sb = new StringBuffer\(\); while\(\(in.read\(buf\)!=-1\){ sb.append\(new String\(buf\)\); buf = new byte\[1024\];//重新生成，避免和上次读取的数据重复 } return sb.toString\(\);

* **File转为byte数组**

```text
File file = new File("D:\\transfile\\createFile1.txt");

long fileSize = file.length();
if (fileSize > Integer.MAX_VALUE) {
  System.out.println("file too big...");
}
FileInputStream fi = new FileInputStream(file);
byte[] buffer = new byte[(int) fileSize];
int offset = 0;
int numRead = 0;
while (offset < buffer.length && (numRead = fi.read(buffer, offset, buffer.length - offset)) >= 0) {
  offset += numRead;
}

// 确保所有数据均被读取

if (offset != buffer.length) {

  throw new IOException("Could not completely read file " + file.getName());

}

fi.close();
```

优先参考 [http://blog.csdn.net/jiangxinyu/article/details/7885518/](http://blog.csdn.net/jiangxinyu/article/details/7885518/)

参考： [http://www.cnblogs.com/zhuocheng/archive/2011/12/12/2285290.html](http://www.cnblogs.com/zhuocheng/archive/2011/12/12/2285290.html) [http://blog.csdn.net/jiangxinyu/article/details/7885518/](http://blog.csdn.net/jiangxinyu/article/details/7885518/)

