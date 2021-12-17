# Java 结合 keytool 实现非对称签名与验证

## Java 结合 keytool 实现非对称签名与验证

[ImportNew](java-jie-he-keytool-shi-xian-fei-dui-cheng-qian-ming-yu-yan-zheng.md)  
（点击上方公众号，可快速关注）

> 来源：王洁 ，
>
> imaidata.github.io/blog/2017/07/29/java结合keytool实现非对称签名与验证/

**keytool的使用**

keytool 是 JDK 自带的一个密钥库管理工具。这里只用到了 keytool 的部分功能，包括生成密钥对、导出公钥等。keytool 生成的公钥/私钥对存放到一个到了一个文件中，这个文件有密码保护，通称为 keystore。

**生成密钥对**

> $ keytool -genkey -alias signLegal -keystore examplestanstore -validity 1800

生成别名为 signLegal 的密钥对，存放在密钥库 examplestanstore 中，证书的有效期是1800天（默认是90天）。

输入一系列的参数。输入的参数遵循了 LDAP 的风格和标准。可以想象，生成的密钥对可以看成 LDAP 的一个条目。

命令执行成功后会在当前目录下创建一个叫 examplestanstore 的文件。

**查看密钥对**

> $ keytool -list -keystore examplestanstore -v

列出了examplestanstore密钥库的中所有密钥对。-v参数表示详细信息，详细信息中有证书的失效时间。

**导出公钥证书**

> $ keytool -export -keystore examplestanstore -alias signLegal -file StanSmith.cer

导出的公钥存放在当前目录的 StanSmith.cer 文件中，是个二进制文件。

**Java签名和验证**

参考了Java安全官方教程。

> [http://docs.oracle.com/javase/tutorial/security/apisign/index.html](http://docs.oracle.com/javase/tutorial/security/apisign/index.html)

在该官方教程中，GenSig.java 类生成密钥对，对输入的文件进行签名，输出了一个签名结果文件 sig 和公钥 suepk。

VerSig.java 类接受三个参数：公钥文件名（suepk）、签名文件（sig）、被签名的源文件名（hello.txt）。

该教程解释了两个类的原理，并附加有源码。将源码下载并编译。创建一个 hello.txt 的文件作为被签名的目标文件，里面随便放点字符串。然后执行：

> $ java GenSig hello.txt \(生成文件sig和suepk\)
>
> $ java VerSig suepk sig hello.txt
>
> signature verifies: true

在实际使用时，密钥对不可能每次在程序中重新生成。而 keytool 恰好可以生成并相对安全保存密钥对。所以下面结合了 keytool 和 Java 实现的功能。

**结合keytool与Java签名/验证**

参考”Oracle–The Java Tutorials: Weaknesses and Alternatives”

> [http://docs.oracle.com/javase/tutorial/security/apisign/enhancements.html](http://docs.oracle.com/javase/tutorial/security/apisign/enhancements.html)

密钥对由 keytool 生成并保存到 keystore 中保护起来（keystore有密码）。公钥也从 keystore 中导出。GenSig.java 类只需要从 keystore 中取得私钥进行签名即可。

VerSig.java 也要做适当的修改。貌似因为从 keystore 中导出的是证书而不是公钥，两者的封装格式估计有差异。

**具体步骤**

1. 利用 keytool -genkey 生成密钥对保存在 keystore 中（库文件是examplestanstore）；
2. 利用 `keytool -export` 从 keystore 中导出公钥证书（StanSmith.cer）；
3. 利用新类 GenSig2.java 生成签名（文件名是sig），GenSig2.java 会从 keystore 中取私钥；
4. 将公钥（StanSmith.cer）、签名（sig）、被签名文件（hello.txt）发给验证方；
5. 验证方利用 VerSig2.java 进行验证。

下面是 GenSig2.java 和 VerSig2.java 的源码和执行方式。

GenSig2.java

> import java.io.\*;
>
> import java.security.\*;
>
> class GenSig2 {
>
> public static void main\(String\[\] args\) {
>
> if \(args.length != 1\) {
>
> System.out.println\("Usage: java GenSig2 "\);
>
> }
>
> else try{
>
> /\*create key paire use keytool:
>
> $ keytool -genkey -alias signLegal -keystore examplestanstore -validity 1800\*/
>
> // read keystore file
>
> KeyStore ks = KeyStore.getInstance\("JKS"\);
>
> FileInputStream ksfis = new FileInputStream\("examplestanstore"\);
>
> BufferedInputStream ksbufin = new BufferedInputStream\(ksfis\);
>
> // open keystore and get private key
>
> // alias is 'signLeal', kpasswd/spasswd is 'vagrant'
>
> ks.load\(ksbufin, "vagrant".toCharArray\(\)\);
>
> PrivateKey priv = \(PrivateKey\) ks.getKey\("signLegal", "vagrant".toCharArray\(\)\);
>
> / _Create a Signature object and initialize it with the private key_ /
>
> Signature dsa = Signature.getInstance\("SHA1withDSA", "SUN"\);
>
> dsa.initSign\(priv\);
>
> / _Update and sign the data_ /
>
> FileInputStream fis = new FileInputStream\(args\[0\]\);
>
> BufferedInputStream bufin = new BufferedInputStream\(fis\);
>
> byte\[\] buffer = new byte\[1024\];
>
> int len;
>
> while \(bufin.available\(\) != 0\) {
>
> len = bufin.read\(buffer\);
>
> dsa.update\(buffer, 0, len\);
>
> };
>
> bufin.close\(\);
>
> /\* Now that all the data to be signed has been read in,
>
> generate a signature for it \*/
>
> byte\[\] realSig = dsa.sign\(\);
>
> / _Save the signature in a file_ /
>
> FileOutputStream sigfos = new FileOutputStream\("sig"\);
>
> sigfos.write\(realSig\);
>
> sigfos.close\(\);
>
> /\* public key file can export from keystore use keytool:
>
> $ keytool -export -keystore examplestanstore -alias signLegal -file StanSmith.cer \*/
>
> } catch \(Exception e\) {
>
> System.err.println\("Caught exception " + e.toString\(\)\);
>
> }
>
> };

编译后，这样运行：

> $ java GenSig2 hello.txt

会生成签名文件sig。

VerSig2.java

> import java.io.\*;
>
> import java.security.\*;
>
> import java.security.spec.\*;
>
> class VerSig2 {
>
> public static void main\(String\[\] args\) {
>
> / _Verify a DSA signature_ /
>
> if \(args.length != 3\) {
>
> System.out.println\("Usage: VerSig publickeyfile signaturefile datafile"\);
>
> }
>
> else try{
>
> / _import encoded public cert_ /
>
> FileInputStream certfis = new FileInputStream\(args\[0\]\);
>
> java.security.cert.CertificateFactory cf =
>
> java.security.cert.CertificateFactory.getInstance\("X.509"\);
>
> java.security.cert.Certificate cert = cf.generateCertificate\(certfis\);
>
> PublicKey pubKey = cert.getPublicKey\(\);
>
> / _input the signature bytes_ /
>
> FileInputStream sigfis = new FileInputStream\(args\[1\]\);
>
> byte\[\] sigToVerify = new byte\[sigfis.available\(\)\];
>
> sigfis.read\(sigToVerify \);
>
> sigfis.close\(\);
>
> / _create a Signature object and initialize it with the public key_ /
>
> Signature sig = Signature.getInstance\("SHA1withDSA", "SUN"\);
>
> sig.initVerify\(pubKey\);
>
> / _Update and verify the data_ /
>
> FileInputStream datafis = new FileInputStream\(args\[2\]\);
>
> BufferedInputStream bufin = new BufferedInputStream\(datafis\);
>
> byte\[\] buffer = new byte\[1024\];
>
> int len;
>
> while \(bufin.available\(\) != 0\) {
>
> len = bufin.read\(buffer\);
>
> sig.update\(buffer, 0, len\);
>
> };
>
> bufin.close\(\);
>
> boolean verifies = sig.verify\(sigToVerify\);
>
> System.out.println\("signature verifies: " + verifies\);
>
> } catch \(Exception e\) {
>
> System.err.println\("Caught exception " + e.toString\(\)\);
>
> };
>
> }
>
> }

编译后，这样运行（StanSmith.cer 是利用 keytool 导出的公钥证书，见前文）：

> $ java VerSig2 StanSmith.cer sig hello.txt
>
> signature verifies: true

**OpenSSL**

虽然也研究了一下 OpenSSL，但发现与 Java 难以结合，难度也很大。例如它的教程中采用的是 RSA，而上面的 Java 使用的是 DSA。所以只是贴在这里备忘，可以忽略。

参考”An Introduction to the OpenSSL command line tool”

> [http://users.dcc.uchile.cl/~pcamacho/tutorial/crypto/openssl/openssl\_intro.html](http://users.dcc.uchile.cl/~pcamacho/tutorial/crypto/openssl/openssl_intro.html)

**生成私钥**

> $ openssl genrsa -out key.pem 1024
>
> $ cat key.pem
>
> -----BEGIN RSA PRIVATE KEY-----
>
> MIICXQIBAAKBgQCzVDmu6Cf2QF7cERCGYU3B8Epm6pkkpMZFgotphXMgAmBBNJbh
>
> Si7qPH4R5JlEm1ZXPr5DZH/pyJBWQhiiHGeUAOve+GOgvt9Rk25r7OEWYvn/GCr/
>
> JBfLBGqwtlzn/t2s2x04IooshsGkOd6YpZoztkEDtu2gKHedFczF607IvwIDAQAB
>
> AoGAMdbIqUmwQYomUvcTJqXIXIwRwYSVx09cI1lisZL7Kfw/ECAzhq19WHAzgXmM
>
> 9zpMxraTXluCCVFKfA6mlfda+ZoBlKSYdOecwNB+TSAumf9XK8uHW/g8C+Ykq9OG
>
> g9Uiy8rKnl12Zaiu9H8L82ud0CkTFW2636/PuKgtp+4YbXECQQDhKdh8lwgumg7H
>
> YIw5476QOHnPL7c3OFPGtaOZMZJkjMPfRzgR4B5PjcGnOLDoTlkATcBPmXtLwwJJ
>
> SzaBdaRjAkEAy+NwdOzC1yQrTrkZQx1brNjO3iytfkl3t1xAWyz5Sy1IB7+4fsod
>
> Eh3br5E1o5YRipY2GJZvp2OAAt3tz6iS9QJASvIYwu+qo4hX3vk9847gwTRrJxFk
>
> 1JaFHCEdgUJEzf8ku08DVL/alvRCPxzZlZluenFmz5fwuDkCq87DJ7g2rQJBAMDM
>
> +SnIPdMeA8n0pRvfJjLD7pMP4pu6M3fzx3Owiqj5T9TsCjXzQBxCmdxizzs7DKll
>
> tA/6Kek64PFVFa25tgUCQQCTM1VwfNKjFbd+0HuF6WAs3Odjuo0gKk/QIjdn7M5/
>
> I0kxEApKxTto3oiuCQGeYL/sqy3WjM0476w48+xUsQeF
>
> -----END RSA PRIVATE KEY-----

**导出公钥**

> $ openssl rsa -in key.pem -pubout -out pub-key.pem
>
> $ cat pub-key.pem
>
> -----BEGIN PUBLIC KEY-----
>
> MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCzVDmu6Cf2QF7cERCGYU3B8Epm
>
> 6pkkpMZFgotphXMgAmBBNJbhSi7qPH4R5JlEm1ZXPr5DZH/pyJBWQhiiHGeUAOve
>
> +GOgvt9Rk25r7OEWYvn/GCr/JBfLBGqwtlzn/t2s2x04IooshsGkOd6YpZoztkED
>
> tu2gKHedFczF607IvwIDAQAB
>
> -----END PUBLIC KEY-----

**摘要计算**

创建一个内容是 1234 的文本文件 hello.txt。用 OpenSSL 计算它的 SHA256 摘要（SHA256 是 jarsigner 的默认摘要算法）：

> $ cat hello.txt
>
> 1234
>
> $ openssl dgst -SHA256 -out hello.sha256 hello.txt
>
> $ cat hello.sha256
>
> SHA256\(hello.txt\)= a883dafc480d466ee04e0d6da986bd78eb1fdd2178d04693723da3a8f95d42f4

**签名和验证**

对摘要文件 hello.sha256 进行签名：

> $ openssl rsautl -sign -in hello.sha256 -out hello.sign -inkey key.pem

用公钥对签名进行验证：

> $ openssl rsautl -verify -in hello.sign -inkey pub-key.pem -pubin
>
> SHA256\(hello.txt\)= a883dafc480d466ee04e0d6da986bd78eb1fdd2178d04693723da3a8f95d42f4

用公钥验证必须加上 -pubin 参数。 用私钥对签名进行验证：

> $ openssl rsautl -verify -in hello.sign -inkey key.pem
>
> SHA256\(hello.txt\)= a883dafc480d466ee04e0d6da986bd78eb1fdd2178d04693723da3a8f95d42f4

验证的STD输出与摘要文件 hello.sha256 的内容一样，说明验证可以通过。

【关于投稿】

如果大家有原创好文投稿，请直接给公号发送留言。

① 留言格式： 【投稿】+《 文章标题》+ 文章链接

② 示例： 【投稿】《不要自称是程序员，我十多年的 IT 职场总结》：[http://blog.jobbole.com/94148/](http://blog.jobbole.com/94148/)

③ 最后请附上您的个人简介哈~

看完本文有收获？请转发分享给更多人

**关注「ImportNew」，提升Java技能**

![](https://github.com/yangbao93/docs/tree/d23f2b2cbc4eb06e62d38114d6a7f5410080c7b5/技术知识/Java/Java%20结合%20keytool%20实现非对称签名与验证/640.jpg)

[阅读原文](https://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&amp;mid=2651481550&amp;idx=1&amp;sn=f5a111d4a2edcc7e3b6deff6da1a85b4&amp;chksm=bd2509b18a5280a72cc6d74ce849ce8ef755126f230cf6048e56a6a0c9cc3a1cc148c8332672&amp;mpshare=1&amp;scene=1&amp;srcid=0812YTa2slyuKmPQgM1u1n0h##)

[http://mp.weixin.qq.com/s?\_\_biz=MjM5NzMyMjAwMA==&mid=2651481550&idx=1&sn=f5a111d4a2edcc7e3b6deff6da1a85b4&chksm=bd2509b18a5280a72cc6d74ce849ce8ef755126f230cf6048e56a6a0c9cc3a1cc148c8332672&mpshare=1&scene=1&srcid=0812YTa2slyuKmPQgM1u1n0h\#rd](http://mp.weixin.qq.com/s?__biz=MjM5NzMyMjAwMA==&mid=2651481550&idx=1&sn=f5a111d4a2edcc7e3b6deff6da1a85b4&chksm=bd2509b18a5280a72cc6d74ce849ce8ef755126f230cf6048e56a6a0c9cc3a1cc148c8332672&mpshare=1&scene=1&srcid=0812YTa2slyuKmPQgM1u1n0h#rd)

