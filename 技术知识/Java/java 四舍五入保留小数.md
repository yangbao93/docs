# java 四舍五入保留小数

## java 四舍五入保留小数

## [java 四舍五入保留小数](http://www.cnblogs.com/hoojo/p/java_round_format_bigdecimal_decimalformat.html)

// 方式一： double f = 3.1516;

BigDecimal b = new BigDecimal\(f\);

double f1 = b.setScale\(2, BigDecimal.ROUND\_HALF\_UP\).doubleValue\(\); // 方式二： new java.text.DecimalFormat\("＃.00"\).format\(3.1415926\); // ＃.00 表示两位小数 ＃.0000四位小数 以此类推… // 方式三： double d = 3.1415926;

String result = String.format\("%.2f", d\);

// %.2f %. 表示 小数点前任意位数 2 表示两位小数 格式后的结果为f 表示浮点型。 //方法四：

Math.round\(5.2644555  _100\)_  0.01d;

//String.format\("%0" + 15 + "d", 23\) 23不足15为就在前面补0

Bigdecimal中的round\_half\_up / round\_half\_down 即是满五上入或不入 [http://www.cnblogs.com/hoojo/p/java\_round\_format\_bigdecimal\_decimalformat.html](http://www.cnblogs.com/hoojo/p/java_round_format_bigdecimal_decimalformat.html)

