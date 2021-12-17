# findbug问题及解决方法

ME\_ENUM\_FIELD\_SETTER 将方法改成private或者是移除

ST\_WRITE\_TO\_STATIC\_FROM\_INSTANCE\_METHOD 描述：Write to static field 通过实例方法更新静态属性 常见于常量类，直接通过类名.常量名获取的方式违背了封装的原则，findbugs不提倡使用，而如果将常量改成静态成员变量，又因为spring不支持静态注入导致不能实现，解决方法是非静态的setter调用静态的setter方法给静态成员变量赋值。 解决方法： 常量类F：

class F{ public static String a = “123”; }

常量a改为静态成员变量，通过F.getA\(\)获取，且由于spring不支持静态注入，改为：

class F{ private static String a; public static Integer getA\(\) { return a; } public void setA\(String a\) { setAValue\(a\); } public static void setAValue\(String a\) { F.a = a; } }

STCAL\_INVOKE\_ON\_STATIC\_DATE\_FORMAT\_INSTANCE DateFormats 是非线程安全的， 其实要避免这个问题方法很简单，不使用SimpleDateFormat，或者不使用成员变量/静态成员变量的SimpleDateFormat对象即可。 参考： [http://rainbow702.iteye.com/blog/1155362](http://rainbow702.iteye.com/blog/1155362)

NM\_METHOD\_NAMING\_CONVENTION 方法应该是动词，与第一个字母小写混合的情况下，与每个单词的首字母大写的内部。 方法名不以小写字母开始

方法应该是动词,与第一个字母小写字母大小写混合,与内部每个词的第一个字母大写。

参数名大小写注意

RV\_RETURN\_VALUE\_IGNORED\_BAD\_PRACTICE 关于一个方法逻辑执行是否成功，有两种方式，一种是抛出异常，一种是提供boolean类型的返回值。举一个例子，用户登录，某些人将login方法的返回值定义为int，然后枚举出各个值的含义，比如0代表成功，1代表用户名不存在等等；而有些人，把这些枚举值看成是use case中的异常流，将它们定义为异常对象，遇到“异常”情况直接抛出异常从而实现分支的流程。第一种方式是典型的C语言面向过程风格，第二种方式，带有强烈的面向对象味道，特别是java提供了checked Exception，貌似偏离主题了。 java中很多方法的执行成功依赖于异常的分支实现，但也有提供返回值的实现，比如这里的File.delete方法，上面的写法忽略了返回值（如果调用某个方法却不使用其返回值要特别注意），删除一个文件很可能不成功，但是从代码里并没有看到这一层面的意思。 解决方法： 文件删除不成功该怎么办？现在能处理就处理，现在不能处理就把父类的方法也改成有返回值的，然后向上传递，这跟处理异常的道理是一样的，当然，你也可以把它封装成一个异常对象。

OBL\_UNSATISFIED\_OBLIGATION\_EXCEPTION\_EDGE **方法可能因为checked exception导致清理流或资源失败** **参考：** [http://just4java.iteye.com/blog/2160704](http://just4java.iteye.com/blog/2160704)

```text
-     public static void closeResources (Connection con, PreparedStatement ps,ResultSet rs){
-         try {
-             if (rs != null) {
-                 rs.close();
-             }
-         } catch (SQLException e) {
-             LOG.error("释放ResultSet出错", e);
-         } finally {
-             try {
-                 if (ps != null){
-                     ps.close();
-                 }
-             } catch (SQLException e) {
-                 LOG.error("释放PreparedStatement出错", e);
-             } finally {
-                 if (con != null) {
-                     try {
-                         con.close();
-                     } catch (SQLException e) {
-                         LOG.error("释放Connection出错", e);
-                     }
-                 }
-             }
       }
-     }
```

DM\_STRING\_VOID\_CTOR 使用没有参数的构造方法去创建新的String对象是浪费内存空间的，因为这样创建会和空字符串“”混淆。Java中保证完成相同的构造方法会产生描绘相同的String对象。所以你只要使用空字符串来创建就可以了。

[DM\_STRING\_CTOR](http://10.1.38.22:7070/jenkins/job/build-accountarchive/25/findbugsResult/module.837842580/package.2086092729/file.786878306/type.-1085807924/) 使用java.lang.String\(String\)构造函数会浪费内存因为这种构造方式和String作为参数在功能上容易混乱。只是使用String直接作为参数的形式

DM\_BOXED\_PRIMITIVE\_FOR\_PARSING 使用parseInt更有效率一点。 valueOf\(\).intValue\(\):转换过程是 parseInt:

EI\_EXPOSE\_REP 描述：may expose internal representation by returning getter方法返回引用类型 eclipse自动生成的引用类型\(Object、数组、Date等\)的getter、setter方法会得到或通过对可变对象的引用操作而暴露代码内部实现，解决方法很多，只要返回的或赋值的对象不是原引用对象即可。 解决方法： 以Date类型为例：

public Date getHappenTime\(\) { if\(happenTime != null\){ return \(Date\) happenTime.clone\(\); }else{ return null; } }

EI\_EXPOSE\_REP2 描述：may expose internal representation by storing an externally mutable object into setter方法返回引用类型 eclipse自动生成的引用类型\(Object、数组、Date等\)的getter、setter方法会得到或通过对可变对象的引用操作而暴露代码内部实现，解决方法很多，只要返回的或赋值的对象不是原引用对象即可。 解决方法： 以Date类型为例：

public void setHappenTime\(Date happenTime\) { if\(happenTime != null\){ this.happenTime = \(Date\) happenTime.clone\(\); }else{ this.happenTime = null; } }

BX\_UNBOXING\_IMMEDIATELY\_REBOXED 描述： Boxed value is unboxed and then immediately reboxed 装箱的值被拆箱，然后立刻重新装箱了 常见的是三目运算时，同时存在基本类型和包装类型。 解决方法：

Integer a = null; //... a = \(a == null\)?0:a;

此问题在于a不为null时，会被拆箱，赋值时再装箱。这是自动装箱拆箱的特性，只要运算中有不同类型，当涉及到类型转换时，编译器就会向下转型，再进行运算。修改方法，统一类型：

Integer a = null; //... a = \(a == null\)?Integer.valueOf\(0\):a;

**DLS\_DEAD\_LOCAL\_STORE**

public static void main\(String args\[\]\) throws Exception{ Object str = new Object\(\) ; //报错处 str = new Object\(\) ; System.out.println\(str\); }

Object str = new Object\(\);是无用的代码，因为在下面有一句str= new Object\(\);，很多语言编译器它都会做优化，比如：去除一些无用的代码以提高效率。JAVA编译器也会做一些优化，但是，Java编译器对上面这段代码却没有做优化（你可以DComplie确认一下），编译后的.class文件还是new了两次，具体什么原因导致它不去做这个优化我还不能确定，我觉得难做这种优化不是借口，起码不应该是Sun的借口。

修改这段代码方法很简单，随便去掉一行new Object\(\);就可以了。

SBSC\_USE\_STRINGBUFFER\_CONCATENATION

方法在循环中使用+进行字符串拼接 For example:

// This is bad

String s ="";

for \(int i = 0; i&lt; field.length; ++i\) {

s = s +field\[i\];

}

// This is better

StringBuffer buf= new StringBuffer\(\);

for \(int i = 0; i&lt; field.length; ++i\) {

buf.append\(field\[i\]\);

}

String s = buf.toString\(\);

NP\_NULL\_ON\_SOME\_PATH\_EXCEPTION 可能存在为空的情况，必须进行处理，而不是产生null指针

```text
public void test(){
  //
  ResultSet rs = cmd.executeQuery();
  if (rs != null && rs.next()) {
    goodsId = rs.getInt("goods_id");
  }
  rs.close();　　//rs可能为null
  //
}
```

可以用try...catch...finally...处理可能出现的异常，也可以在rs.close之前，先判断rs是否为null

ICAST\_INT\_CAST\_TO\_DOUBLE\_PASSED\_TO\_CEIL integral的值转换为double后使用了Math.ceil方法

注意前后int 和double的转换

DM\_DEFAULT\_ENCODING 编码格式问题，没有设置默认编码格式，这可能导致在不同平台上出现编码不一致的问题

RCN\_REDUNDANT\_NULLCHECK\_OF\_NONNULL\_VALUE 对非空的不断重复检查

NP\_ALWAYS\_NULL

IP\_PARAMETER\_IS\_DEAD\_BUT\_OVERWRITTEN 将忽略此参数的初始值和参数是覆盖。这通常表明一种错误的观念就是,写入参数将被返回给调用者。

UC\_USELESS\_OBJECT 报错，无用的对象。检查对象是否有使用

REC\_CATCH\_EXCEPTION

RCN\_REDUNDANT\_NULLCHECK\_WOULD\_HAVE\_BEEN\_A\_NPE 一个多余的空判断 检查一个值是否为空,但这个值不能是零,因为以前的引用时,如果是空一个空指针异常发生在前面的废弃。从本质上讲,这段代码和前面的废弃不同意这个值是否允许空值。检查是多余的或以前的废弃是错误的。

参考 [http://blog.csdn.net/aya19880214/article/details/41958347](http://blog.csdn.net/aya19880214/article/details/41958347)

RV\_RETURN\_VALUE\_IGNORED\_NO\_SIDE\_EFFECT

SA\_LOCAL\_SELF\_ASSIGNMENT public void fun\(\){ int x =3; x = x; }

SF\_DEAD\_STORE\_DUE\_TO\_SWITCH\_FALLTHROUGH

MS\_CANNOT\_BE\_FINAL 增加final

UWF\_UNWRITTEN\_FIELD 字段永远没被复制，值一直为空

DMI\_HARDCODED\_ABSOLUTE\_FILENAME 包含一个硬编码的绝对路径

NP\_NULL\_ON\_SOME\_PATH\_FROM\_RETURN\_VALUE 方法的返回值没有进行是否为空的检查就重新赋值，这样可能会出现空指针异常。

BC\_VACUOUS\_INSTANCEOF 在instanceof判断中，除非为空，否则永远是true，如果是为了判断空，用更好的方法判断。使用instanceof判断会使得程序不利于别人理解。

DC\_DOUBLECHECK 多线程错误 - 可能对属性进行了双重检测

IS2\_INCONSISTENT\_SYNC

RCN\_REDUNDANT\_NULLCHECK\_OF\_NULL\_VALUE

NP\_NULL\_ON\_SOME\_PATH 可能存在为空的引用，应考虑其解决

[http://10.1.38.22:7070/jenkins/job/build-accountarchive/24/findbugsResult/module.837842580/package.2071149210/](http://10.1.38.22:7070/jenkins/job/build-accountarchive/24/findbugsResult/module.837842580/package.2071149210/)

