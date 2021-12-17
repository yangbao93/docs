# java时间操作

1. 获取当前天的零点零分零秒

/\*\*

* 获取今天零时零分零秒 \*/ private Date getToday\(\){ Calendar calendar = Calendar.getInstance\(\); calendar.setTime\(new Date\(\)\); calendar.set\(Calendar.HOUR\_OF\_DAY, 0\); calendar.set\(Calendar.MINUTE, 0\); calendar.set\(Calendar.SECOND, 0\); Date zero = calendar.getTime\(\); return zero; }
* 获取当前时间的前一天

/\*\*

* 获取上一天的日期 \*/ private Date getLastDay\(Date date\) { Calendar calendar = Calendar.getInstance\(\); calendar.setTime\(date\); calendar.add\(Calendar.DAY\_OF\_MONTH, -1\); return calendar.getTime\(\); }
* 格式化时间

Calendar calendar = Calendar.getInstance\(\); DateFormat format = new SimpleDateFormat\("yyyy-MM-dd"\); format.format\(date\);

1. String 转时间

```text
SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd");//小写的mm表示的是分钟
String dstr="2008-4-24";
```

java.util.Date date=sdf.parse\(dstr\);

about:blank

