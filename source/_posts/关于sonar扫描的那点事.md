---
title: 关于sonar扫描的那点事
date: 2021-04-10 17:54:39
tags:
categories:
---


关于sonar扫描的那点事

````
1、".equals()" should not be used to test the values of "Atomic" classes.
bug  主要
不要使用equals方法对AtomicXXX进行是否相等的判断
Atomic变量永远只会和自身相等，Atomic变量没有覆写equals()方法.
2、"=+" should not be used instead of "+="
bug 主要
"=+" 与 "=+" 意义不同
a =+ b;虽然正确但写法不合规，应写成 a = +b;
3、"@NonNull" values should not be set to null
bug 次要
标注非空假定非空且在使用之前不进行非空检查，设置为空会导致空指针异常
4、"BigDecimal(double)" should not be used
bug 主要
因为浮点的不精确,可能使用BigDecimal(double)得不到期望的值
5、"compareTo" results should not be checked for specific values
bug 次要
compareTo可能返回不是具体的值（除0外），建议用 >0、<0、=0
6、"compareTo" should not return "Integer.MIN_VALUE"
bug 次要
compareTo只代表一个不等标识，不代表不等的程度，应返回-1,0,1标识即可
7、 "Double.longBitsToDouble" should not be used for "int"
bug 主要  
Double.longBitsToDouble返回给定的位所代表的double值,需要一个64位的long类型参数.
8、 "equals" method overrides should accept "Object" parameters
bug 主要  
equals作为方法名应该仅用于重写Object.equals(Object)来避免混乱.
9、 "equals(Object obj)" should test argument type
bug 次要
要比较obj的class  type是否一样
10、"equals" methods should be symmetric and work for subclasses
bug 次要
equals应是对等并且在有子类参与时能正常工作
11、"equals(Object obj)" and "hashCode()" should be overridden in pairs
bug 次要
成对重写
12、"Externalizable" classes should have no-arguments constructors
bug 主要
Externalizable(可序列化与返序列化)类应该有无参构造器
13、"getClass" should not be used for synchronization
bug  主要
{synchronized (this.getClass())}  错误 子类继承此方法时不能做到同步
{synchronized (MyClass.class)} 正确
14、"hashCode" and "toString" should not be called on array instances
bug 主要
使用Arrays.toString(args)和Arrays.hashCode(args)代替.
15、"instanceof" operators that always return "true" or "false" should be removed
bug 主要
16、"InterruptedException" should not be ignored
bug 主要
try {
while (true) {
// do stuff
}
}catch (InterruptedException e) {
LOGGER.log(Level.WARN, "Interrupted!", e);
// Restore interrupted state...
Thread.currentThread().interrupt();
}
17、"Iterator.hasNext()" should not call "Iterator.next()"
bug 主要
18、"Iterator.next()" methods should throw "NoSuchElementException"
bug 次要
public String next(){
if(!hasNext()){
throw new NoSuchElementException();
}
...
}
19、"notifyAll" should be used
bug 主要
notify可能不能唤醒正确的线程，notifyAll代之。
20、"null" should not be used with "Optional"
bug 主要
把判空包装起来使用而不直接使用!=null
21、"PreparedStatement" and "ResultSet" methods should be called with valid indices
bug 阻断
PreparedStatement与ResultSet参数设置与获取数据由序号1开始而非0
22、"read" and "readLine" return values should be used
bug 主要
BufferedReader.readLine(), Reader.read()及子类中的相关方法都应该先存储再比较
buffReader = new BufferedReader(new FileReader(fileName));
String line = null;
while ((line = buffReader.readLine()) != null) {
// ...
}
23、"runFinalizersOnExit" should not be called
bug 严重
JVM退出时不可能运行finalizers,System.runFinalizersOnExit 和 Runtime.runFinalizersOnExit可以在jvm退出时运行但是因为他们不安全而弃用.
正确用法:
Runtime.addShutdownHook(new Runnable() {
public void run(){
doSomething();
}
});
24、"ScheduledThreadPoolExecutor" should not have 0 core threads
bug 严重
java.util.concurrent.ScheduledThreadPoolExecutor由属性corePoolSize指定线程池大小，如果设置为0表示线程执行器无线程可用且不做任何事.
25、"Serializable" inner classes of non-serializable classes should be "static"
bug 次要
序列化非静态内部类将导致尝试序列化外部类,如果外部类不是序列化类,会产生运行时异常，内部类静态化会避免这种情况
26、"SingleConnectionFactory" instances should be set to "reconnectOnException"
bug 主要
使用Spring SingleConnectionFactory而不启用reconnectOnException设置当连接恶化将阻止自动连接恢复。
27、"StringBuilder" and "StringBuffer" should not be instantiated with a character
bug 主要
StringBuffer foo = new StringBuffer('x');  错  equivalent to StringBuffer foo = new StringBuffer(120);
StringBuffer foo = new StringBuffer("x");  对
28、 "super.finalize()" should be called at the end of "Object.finalize()" implementations
bug 严重
protected void finalize() {
releaseSomeResources();
super.finalize();       //调用，最后调用
}
29、"toArray" should be passed an array of the proper type
bug 次要
toArray()无参且强制类型转换会产生运行时异常,应传入一个合适的类弄作参数
public String [] getStringArray(List<String> strings) {
return strings.toArray(new String[0]);
}
30、"toString()" and "clone()" methods should not return null
bug 主要
可返回""
31、 "wait" should not be called when multiple locks are held
bug 阻断
32、 "wait", "notify" and "notifyAll" should only be called when a lock is obviously held on an object
bug 主要
先要获得对象锁才能进行上述操作
private void removeElement() {
synchronized(obj) {
while (!suitableCondition()){
obj.wait();
}
... // Perform removal
}
}

      or


      private synchronized void removeElement() {
while (!suitableCondition()){
wait();
}
... // Perform removal
}
33、"wait(...)" should be used instead of "Thread.sleep(...)" when a lock is held
bug 阻断
当持有锁的当前线程调用Thread.sleep(...)可能导致性能和扩展性问题，甚至死锁因为持有锁的当前线程已冻结.合适的做法是锁对象wait()释放锁让其它线程进来运行.
34、A "for" loop update clause should move the counter in the right direction
bug 主要
检查for循环下标递增或递减正确
35、All branches in a conditional structure should not have exactly the same implementation
bug 主要
分支中不应该有相同的实现
36、Blocks should be synchronized on "private final" fields or parameters
bug 主要
synchronized同步块应该锁在private final fields或parameters对象上,因为同步块内非final锁对象可能改变导致其它线程进来运行.
37、Boxing and unboxing should not be immediately reversed
bug  次要
自动拆箱和装箱不需手动转换
38、Child class methods named for parent class methods should be overrides
bug 主要
以下情况不是重写:
a、父类方法是static的而子类方法不是static的
b、子类方法的参数或返回值与父类方法不是同一个包
c、父类方法是private
为了不产生混乱，不要与父类方法同名
39、Classes extending java.lang.Thread should override the "run" method
bug 主要
线程类应该重写run方法
40、Classes should not be compared by name
bug 主要
不要用类名称比较类是否相同，而用instanceof或者Class.isAssignableFrom()进行底动类型比较
41、Classes that don't define "hashCode()" should not be used in hashes
bug 主要
没有定义hashCode()方法的类不能作为hash集合中的键值，因为equal相同的实例对像可能返回不同的hash值.
42、Collections should not be passed as arguments to their own methods
bug 主要
集合实例不应该作为参数被传给集合实你还自已的方法中
43、Conditionally executed blocks should be reachable
bug 主要
条件执行块应该可达
44、Constructor injection should be used instead of field injection
bug 主要
构造器注入应该替代属性注入(非Spring framework)
因为任何非Spring framework实例化而是通过构造器实例化的实例不能注入属性,这样公有的构造器实化化后可能产生NullPointerException，除非所有的构造器都是私有的
45、Consumed Stream pipelines should not be reused
bug 主要
流不应该重用
46、Custom resources should be closed
bug 阻断
资源应该关闭
47、Custom serialization method signatures should meet requirements
bug 主要
自定义类序列化方法签名应该合法
49、Dependencies should not have "system" scope
bug 严重
maven依赖不要在system scope
50、Dissimilar primitive wrappers should not be used with the ternary operator without explicit casting
bug 主要
不同的原始包装类如果没有明确的类转换不能用于三元操作中
51、Double Brace Initialization should not be used
bug 次要
双构造初始不要用
Map source = new HashMap(){{ // Noncompliant
put("firstName", "John");
put("lastName", "Smith");
}};
此操作如一个anonymous inner class，如果anonymous inner class返回且被其它对象引用，可能产生memory leaks，既使不产生memory leaks也会让大多维护者感到迷惑
52、Double-checked locking should not be used
bug 阻断
重复检查的锁块不要使用
public static Resource getInstance() {
if (resource == null) {
synchronized (DoubleCheckedLocking.class) {
if (resource == null)
resource = new Resource();
}
}
return resource;
}
应
public synchronized static Resource getInstance() {
if (resource == null)
resource = new Resource();
return resource;
}
53、Equals Hash Code
bug 严重
成对重写equals()与hashCode()
54、Exception should not be created without being thrown
bug 主要
不被抛出的异常不要创建
55、Expressions used in "assert" should not produce side effects
bug 主要
assert表达式不要产生负影响，不要改变数据状态
56、Failed unit tests should be fixed
bug 主要
失败的单元测试应该尽快解决掉
57、Floating point numbers should not be tested for equality
bug 主要
浮点数不要进行比较
58、Getters and setters should be synchronized in pairs
bug 主要
get与set应该成对进行同步操作
59、Identical expressions should not be used on both sides of a binary operator
bug 主要
相同的表达式不要作为二进制操作的操作数使用,应该简化
60、Inappropriate "Collection" calls should not be made
bug 主要
正确使用集合元素类型
61、Inappropriate regular expressions should not be used
bug 主要
正确使用正则表达式
62、Intermediate Stream methods should not be left unused
bug 主要
中间流应该被使用
63、Ints and longs should not be shifted by zero or more than their number of bits-1
bug 次要
整型与长整型位移操作数应该价于1与类型占位数-1
64、Invalid "Date" values should not be used
bug 主要
正确使用日期
65、Jump statements should not occur in "finally" blocks
bug 主要
finally块中使用return, break, throw等Jump statements，会阻止在try catch中抛出的未处理异常的传播
66、Locks should be released
bug 严重
保证锁的能够释放
67、Loop conditions should be true at least once
bug 主要
循环应该至少走一次
68、Loops should not be infinit
bug 阻断
循环不应该死循环
69、Math operands should be cast before assignment
bug 次要
数字操作在操作或赋值前要转化
70、Math should not be performed on floats
bug 次要
BigDecimal代替floats进行大数精确运算
71、Methods "wait(...)", "notify()" and "notifyAll()" should not be called on Thread instances
bug 阻断
不要在线程中使用"wait(...)", "notify()" and "notifyAll()"
72、Methods should not be named "hashcode" or "equal"
bug 主要
除非Override重写这些方法
73、Multiline blocks should be enclosed in curly braces
bug 主要
多列块应用大括号括起来
74、Neither "Math.abs" nor negation should be used on numbers that could be "MIN_VALUE"
bug 次要
不要对数值类型的MIN_VALUE值或返回值为此值进行Math.abs与取反操作，因为不会起作用。
75、Non-public methods should not be "@Transactional"
bug 主要
非public方法不要注解Transactional,调用时spring 会抛出异常
76、Non-serializable classes should not be written
bug 主要
执行写操作的类要序列化，否则会抛出异常
77、Non-serializable objects should not be stored in "HttpSession" objects
bug 主要
HttpSession要保存序列化的对象
78、Non-thread-safe fields should not be static
bug 主要
非线程安全的域不应该静态化
79、Null pointers should not be dereferenced
bug 主要
空指针引用不应被访问
80、Optional value should only be accessed after calling isPresent()
bug 主要
Optional实例值的获取要isPresent()之后再做操作
90、Printf-style format strings should not lead to unexpected behavior at runtime
bug 阻断
因为Printf风格格式化是在运行期解读，而不是在编译期检验,会存在风险
91、Raw byte values should not be used in bitwise operations in combination with shifts
bug 主要
原始字节值不应参与位运算
result = (result << 8) | readByte(); // Noncompliant
正:
result = (result << 8) | (readByte() & 0xff);
92、Reflection should not be used to check non-runtime annotations
bug 主要
反射操作不应该运于检查非运行时注解
93、Related "if/else if" statements should not have the same condition
bug 主要
if/else if中不应该有相同的条件
94、Resources should be closed
bug 阻断
打开的资源应该关闭并且放到finally块中进行关闭
95、Return values from functions without side effects should not be ignored
bug 主要
操作对函数返回值没有影响的应该忽略
public void handle(String command){
command.toLowerCase(); // Noncompliant; result of method thrown away
...
}
96、Servlets should not have mutable instance fields
bug 主要
servlet容器对每一个servlet创建一个实例导致实例变量共享产生问题
struts1.x 也是单例
97、Short-circuit logic should be used to prevent null pointer dereferences in conditionals
bug 主要
应正确使用短路逻辑来防止条件中的空指针引用访问
98、Silly equality checks should not be made
bug 主要
愚蠢的相等检查不应该做
非同类型的对象equal
99、Spring "@Controller" classes should not use "@Scope"
bug 主要
保持spring controller的单例
100、Synchronization should not be based on Strings or boxed primitives
bug 主要
字符串和封箱类不应该被用作锁定对象，因为它们被合并和重用。
101、The non-serializable super class of a "Serializable" class should have a no-argument constructor
bug 次要
序列化的类的非序列化父类应有一个无参构造器
102、The Object.finalize() method should not be called
bug 主要
Object.finalize()不要人为去调用
103、The Object.finalize() method should not be overriden
bug 主要
Object.finalize()不要重写
104、The signature of "finalize()" should match that of "Object.finalize()"
bug 主要
Object.finalize()不要重写
105、The value returned from a stream read should be checked
bug 次要
从流中读取的值应先检查再操作
106、Thread.run() should not be called directly
bug 主要
调用start()
107、Useless "if(true) {...}" and "if(false){...}" blocks should be removed
bug 主要
无用的if(true)和if(false)块应移除
108、Value-based classes should not be used for locking
bug 主要
基于值的类不要用于锁对象
109、Value-based objects should not be serialized
bug 次要
基于值的对象不应被用于序列化
110、Values should not be uselessly incremented
bug 主要
值增减后不存储是代码浪费甚至是bug
111、Variables should not be self-assigned
bug 主要
变量不应该自分配如下:
public void setName(String name) {
name = name;
}
112、Week Year ("YYYY") should not be used for date formatting
bug 主要
日期格式化错误
113、Zero should not be a possible denominator
bug 严重
零不应该是一个可能的分母
114、Loops should not be infinite
Bug   阻断
循环不应该是无限的








漏洞类型:


1、"@RequestMapping" methods should be "public"
漏洞  阻断
标注了RequestMapping是controller是处理web请求。既使方法修饰为private,同样也能被外部调用，因为spring通过反射调用方法,没有检查方法可视度，
2、"enum" fields should not be publicly mutable
漏洞 次要
枚举类域不应该是public，也不应该进行set
3、"File.createTempFile" should not be used to create a directory
漏洞 严重
File.createTempFile不应该被用来创建目录
4、"HttpServletRequest.getRequestedSessionId()" should not be used
漏洞 严重
HttpServletRequest.getRequestedSessionId()返回客户端浏览器会话id不要用，用HttpServletRequest.getSession().getId()
5、"javax.crypto.NullCipher" should not be used for anything other than testing
漏洞 阻断
NullCipher类提供了一种“身份密码”，不会以任何方式转换或加密明文。 因此，密文与明文相同。 所以这个类应该用于测试，从不在生产代码中。
6、"public static" fields should be constant
漏洞 次要
public static 域应该 final
7、Class variable fields should not have public accessibility
漏洞 次要
类变量域应该是private,通过set，get进行操作
8、Classes should not be loaded dynamically
漏洞 严重
不应该动态加载类,动态加载的类可能包含由静态类初始化程序执行的恶意代码.
Class clazz = Class.forName(className);  // Noncompliant
9、Cookies should be "secure"
漏洞 次要
Cookie c = new Cookie(SECRET, secret);  // Noncompliant; cookie is not secure
response.addCookie(c);
正:
Cookie c = new Cookie(SECRET, secret);
c.setSecure(true);
response.addCookie(c);
10、Credentials should not be hard-coded
漏洞 阻断
凭证不应该硬编码
11、Cryptographic RSA algorithms should always incorporate OAEP (Optimal Asymmetric Encryption Padding)
漏洞 严重
加密RSA算法应始终包含OAEP（最优非对称加密填充）
12、Default EJB interceptors should be declared in "ejb-jar.xml"
漏洞 阻断
默认EJB拦截器应在“ejb-jar.xml”中声明
13、Defined filters should be used
漏洞 严重
web.xml文件中定义的每个过滤器都应该在<filter-mapping>元素中使用。 否则不会调用此类过滤器。
14、Exceptions should not be thrown from servlet methods
漏洞 次要
不应该从servlet方法抛出异常
15、HTTP referers should not be relied on
漏洞 严重
不应依赖于http，将这些参数值中止后可能是安全的，但绝不应根据其内容作出决定。
如:
String referer = request.getHeader("referer");  // Noncompliant
if(isTrustedReferer(referer)){
//..
}
16、IP addresses should not be hardcoded
漏洞 次要
ip 地址不应该硬编码
17、Member variable visibility should be specified
漏洞 次要
应指定成员变量的可见性
18、Members of Spring components should be injected
漏洞 严重
spring组件的成员应注入，单例注入非静态成员共享会产生风险
19、Mutable fields should not be "public static"
漏洞 次要
多变在域不应为 public static
20、Neither DES (Data Encryption Standard) nor DESede (3DES) should be used
漏洞 阻断
不应使用DES（数据加密标准）和DESEDE（3DES）
21、Only standard cryptographic algorithms should be used
漏洞 严重
标准的加密算法如 SHA-256, SHA-384, SHA-512等,非标准算法是危险的，可能被功能者攻破算法
22、Pseudorandom number generators (PRNGs) should not be used in secure contexts
漏洞 严重
伪随机数生成器（PRNG）不应在安全上下文中使用
23、Return values should not be ignored when they contain the operation status code
漏洞 次要
当函数调用的返回值包含操作状态代码时，应该测试此值以确保操作成功完成。
24、Security constraints should be definedin
漏洞 阻断
应定义安全约束，当web.xml文件没有<security-constraint>元素时，此规则引发了一个问题
25、SHA-1 and Message-Digest hash algorithms should not be used
漏洞 严重
不应该使用SHA-1和消息摘要散列算法,已证实不再安全
26、SQL binding mechanisms should be used
漏洞 阻断
应该使用SQL绑定机制
27、Struts validation forms should have unique names
漏洞 阻断
struts验证表单应有唯一名称
28、Throwable.printStackTrace(...) should not be called
漏洞 次要
Throwable.printStackTrace(...)会打印异常信息，但会暴露敏感信息
29、Untrusted data should not be stored in sessions
漏洞 主要
不受信任的数据不应存储在会话中。
Web会话中的数据被认为在“信任边界”内。 也就是说，它被认为是值得信赖的。 但存储未经身份验证的用户未经验证的数据违反信任边界，并可能导致该数据被不当使用。
30、Values passed to LDAP queries should be sanitized
漏洞 严重
传递到LDAP查询的值应该被清理
31、Values passed to OS commands should be sanitized
漏洞 严重
传递给OS命令的值应该被清理
32、Web applications should not have a "main" method
漏洞 严重
web 应用中不应有一个main方法






坏味道：


1、"==" and "!=" should not be used when "equals" is overridden
坏味道   次要
当类重写equals方法后,不应该再用"=="与"!="进行对象比较
2、"@Deprecated" code should not be used
坏味道   次要
弃用方代码不应再用,弃用代码意味首将会移除,应该使用替代代码
3、"@Override" should be used on overriding and implementing methods
坏味道   主要
重写的和实现在方法要加Override标注
4、"action" mappings should not have too many "forward" entries
坏味道   次要
默认   4
action不要有太多的forward
5、"Arrays.stream" should be used for primitive arrays
坏味道   主要
Arrays.stream用于原始流类型(IntStream, LongStream, DoubleStream)会有更好的性能
6、"catch" clauses should do more than rethrow
坏味道   次要
只是重新抛出捕获的异常和完全放弃异常捕获效果一样，但会给维护者带来疑惑
7、"clone" should not be overridden
坏味道   阻断
不应重写clone方法
8、"Cloneables" should implement "clone"
坏味道   严重
Cloneables类应该实现clone方法
9、"collect" should be used with "Streams" instead of "list::add"
坏味道   次要
虽然您可以使用forEach（list :: add）或使用Stream收集，但是收集是更好的选择，因为它自动线程安全并且可并行
10、"Collections.EMPTY_LIST", "EMPTY_MAP", and "EMPTY_SET" should not be used
坏味道   次要
11、"DateUtils.truncate" from Apache Commons Lang library should not be used
坏味道   主要
使用Java 8中引入的Instant类来截断日期可能会比Commons Lang的DateUtils类快得多。
12、"deleteOnExit" should not be used
坏味道   主要
不推荐使用File.deleteOnExit()
13、"entrySet()" should be iterated when both the key and value are needed
坏味道   主要
当循环中只需要一个map的键时，迭代keySet就是有意义的。 但是，当需要键和值两者时，迭代entrySet更有效，这将允许访问键和值
14、"equals(Object obj)" should be overridden along with the "compareTo(T obj)" method
坏味道   次要
“equals（Object obj）”应该与“compareTo（T obj）”方法一起被重写
15、"Exception" should not be caught when not required by called methods
坏味道   次要
如果被调方法没有抛出“Exception”时不要捕获"Exception",应捕后被调方法抛出的异常
16、"final" classes should not have "protected" members
坏味道   次要
最终类意味首不可继承所以不要有受保存成员，这样没有意义
17、"finalize" should not set fields to "null"
坏味道   次要
finalize不应设置域空，对垃圾收集是没必要的，还可能为垃圾收集带来额处开消
18、"for" loop increment clauses should modify the loops' counters
坏味道   严重
loop循环应该增加递增序号变量
19、"for" loop stop conditions should be invariant
坏味道   主要
循环停止条件应为不变
20、"indexOf" checks should not be for positive numbers
坏味道   严重
检查indexOf返回不应使用正数
21、"indexOf" checks should use a start position
坏味道   次要
如果您需要查看一个子字符串是否位于字符串中某个特定点之外，则可以测试该子字符串与该目标点的indexOf，也可以使用该起始点参数的indexOf版本。 后者可以更清楚，因为结果是针对-1测试的，这是一个容易识别的“未找到”指标
22、"java.lang.Error" should not be extended
坏味道   主要
java.lang.Error及其子类表示异常情况，例如OutOfMemoryError，它只能由Java虚拟机进行处理。
23、"java.nio.Files#delete" should be preferred
坏味道   主要
Files.delete(path)道选
24、"java.time" classes should be used for dates and times
坏味道   主要
Date 和 Calendar类非线程同步，推荐使用LocalDate
25、"Lock" objects should not be "synchronized"
坏味道   主要
“锁定”对象不应“同步”
java.util.concurrent.locks.Lock提供比同步块更强大和灵活的锁定操作，应该使用tryLock（）和unlock（）锁定和解锁这些对象
26、"main" should not "throw" anything
坏味道   阻断
main方法不应该抛出异常
27、"NullPointerException" should not be caught
坏味道   主要
空指针不应捕获处理，应该避免NullPointerException，而不是被捕获
28、"NullPointerException" should not be explicitly thrown
坏味道   主要
“NullPointerException”不应该被显式抛出
29、"Object.finalize()" should remain protected (versus public) when overriding
坏味道   严重
重写finalizey方法应为protected
30、"Object.wait(...)" and "Condition.await(...)" should be called inside a "while" loop
坏味道   严重
“Object.wait（...）”和“Condition.await（...）”应该在“while”循环内调用
31、"Object.wait(...)" should never be called on objects that implement "java.util.concurrent.locks.Condition"
坏味道   主要
不应该在实现“java.util.concurrent.locks.Condition”的对象上调用“Object.wait（...）”
32、"Optional" should not be used for parameters
坏味道   主要
Optional不要被用作参数
33、"Preconditions" and logging arguments should not require evaluation
坏味道   主要
将连接的字符串传递到日志记录方法也可能导致不必要的性能消耗，因为每次调用该方法时将执行级联，无论日志级别是否足够低以显示消息
34、"private" methods called only by inner classes should be moved to those classes
坏味道   次要
只被内部类调用的方法，应该在内部类内部
35、"private" methods that don't access instance data should be "static"
坏味道   次要
不访问实例数据的“私有”方法应该是“静态”
36、"readObject" should not be "synchronized"
坏味道   主要
“readObject”不应该被“同步”
37、"readResolve" methods should be inheritable
坏味道   严重
“readResolve”方法应该是可继承的
38、"ResultSet.isLast()" should not be used
坏味道   主要
ResultSet.isLast()不应用
39、"Serializable" classes should have a version id
坏味道   严重
“Serializable”类应该有一个版本号
40、"Serializable" inner classes of "Serializable" classes should be static
坏味道   次要
实现序列化的内部类应该是静态的
41、"static" members should be accessed statically
坏味道   主要
“静态”成员应类访问
42、"Stream.anyMatch()" should be preferred
坏味道   次要
“Stream.anyMatch（）”应该是首选的
43、"switch case" clauses should not have too many lines of code
坏味道   主要
默认  5
“switch case”子句不应该有太多的代码行
44、"switch" statements should end with "default" clauses
坏味道   严重
“switch”语句应以“default”子句结尾
45、"switch" statements should have at least 3 "case" clauses
坏味道   次要
“switch”语句应具有至少3个“case”子句，可以用if替代
46、"switch" statements should not contain non-case labels
坏味道   阻断
switch语句不用包含非case标签
47、"switch" statements should not have too many "case" clauses
坏味道   主要
默认 30
switch语句不应包含太多case语句
48、"Thread.sleep" should not be used in tests
坏味道   主要
“Thread.sleep”不应该在测试中使用
49、"ThreadLocal.withInitial" should be preferred
坏味道   次要
“ThreadLocal.withInitial”应该是首选
ThreadLocal<List<String>> myThreadLocal = ThreadLocal.withInitial(ArrayList::new);
50、"Threads" should not be used where "Runnables" are expected
坏味道   主要
Noncompliant Code Example


public static void main(String[] args) {
Thread r =new Thread() {
int p;
@Override
public void run() {
while(true)
System.out.println("a");
}
};
new Thread(r).start();  // Noncompliant
Compliant Solution


public static void main(String[] args) {
Runnable r =new Runnable() {
int p;
@Override
public void run() {
while(true)
System.out.println("a");
}
};
new Thread(r).start();
51、"throws" declarations should not be superfluous
坏味道   次要
“抛出”声明不应该是多余的
52、"toString()" should never be called on a String object
坏味道   次要
不应该在String对象上调用“toString（）”
53、"URL.hashCode" and "URL.equals" should be avoided
坏味道   主要
应避免使用“URL.hashCode”和“URL.equals”
54、"writeObject" should not be the only "synchronized" code in a class
坏味道   主要
“writeObject”不应该是类中唯一的“同步”代码
55、 @FunctionalInterface annotation should be used to flag Single Abstract Method interfaces
坏味道   严重
一个只有一个抽象方法的接口应加FunctionalInterface注释
56、A "while" loop should be used instead of a "for" loop
坏味道   次要
当在for循环中仅定义条件表达式，并且缺少初始化和增量表达式时，应使用while循环来增加可读性
57、A close curly brace should be located at the beginning of a line
坏味道   次要
共享编码约定使得团队有可能有效地进行协作。 这个规则使得强制要在行的开头放置一个大括号。
58、A field should not duplicate the name of its containing class
坏味道   主要
字段不应该重复其包含的类的名称
59、Abbreviation As Word In Name
坏味道   主要
检查验证标识符名称中的缩写（连续大写字母）长度，还允许执行骆驼案例命名
60、Abstract Class Name
坏味道   主要
检查抽象类名是否符合指定的格式



ignoreName  
Controls whether to ignore checking the name. Realistically only useful if using the check to identify that match name and do not have the abstract modifier name. Default is false.
默认值
false
ignoreModifier
Controls whether to ignore checking for the abstract modifier on classes that match the name. Default is false.
默认值
false
format  
Regular expression
默认值


61、Abstract class names should comply with a naming convention
坏味道   次要
抽象类名称应符合命名约定

     Regular expression used to check the abstract class names against.
     默认值
     ^Abstract[A-Z][a-zA-Z0-9]*$
62、Abstract classes without fields should be converted to interfaces
坏味道   次要
没有字段的抽象类应该转换为接口
63、Abstract methods should not be redundant
坏味道   次要
抽象方法不应该是多余的
64、An abstract class should have both abstract and concrete methods
坏味道   次要
抽象类应该有抽象和具体的方法
65、An open curly brace should be located at the beginning of a line
坏味道   次要
开放的大括号应位于一行的开头
66、An open curly brace should be located at the end of a line
坏味道   次要
开放的大括号应位于一行的末尾
67、Annotation arguments should appear in the order in which they were declared
坏味道   次要
注释参数应按其声明顺序显示
68、Annotation Location
坏味道   主要
注释位置
allowSamelineSingleParameterlessAnnotation  
To allow single parameterless annotation to be located on the same line as target element.
默认值
true
allowSamelineParameterizedAnnotation
To allow parameterized annotation to be located on the same line as target element.
默认值
false
allowSamelineMultipleAnnotations
To allow annotation to be located on the same line as target element.
默认值
false
tokens  
tokens to check
默认值
CLASS_DEF,INTERFACE_DEF,ENUM_DEF,METHOD_DEF,CTOR_DEF,VARIABLE_DEF
69、Annotation repetitions should not be wrapped
坏味道   次要
注释重复不应包装
70、Annotation Use Style
坏味道   主要

trailingArrayComma
Defines the policy for trailing comma in arrays. Default is never.
closingParens  
Defines the policy for ending parenthesis. Default is never.
elementStyle  
Defines the annotation element styles. Default value is compact_no_array.
71、Anon Inner Length
坏味道   主要
检查长匿名内部类。
max
maximum allowable number of lines. Default is 20.
72、Anonymous inner classes containing only one method should become lambdas
坏味道   主要
只有一个方法的匿名内部类应该变成lambdas
jdk8以下自动禁用
73、Array designators "[]" should be located after the type in method signatures
坏味道   次要
数组代号“[]”应位于方法签名类型之后
74、Array designators "[]" should be on the type, not the variable
坏味道   次要
数组代号“[]”应位于类型之后而不是变量之后
75、Array Trailing Comma
坏味道   主要
检查数组初始化是否包含逗号
76、Array Type Style
坏味道   次要
数组类型样式
javaStyle  
Controls whether to enforce Java style (true) or C style (false). Default is true.
77、Arrays should not be created for varargs parameters
坏味道   次要
不应为varargs参数创建数组
78、Artifact ids should follow a naming convention
坏味道   次要
共享命名约定允许团队有效协作。 当pom的artifactId与提供的正则表达式不匹配时，此规则引发了一个问题

regex
The regular expression the "artifactId" should match
默认值
[a-z][a-z-0-9]+
79、Assertions should be complete
坏味道   阻断
断言应该是完整的
80、Assignments should not be made from within sub-expressions
坏味道   主要
不应在子表达式中作出赋值操作当赋值变量没有用到
81、At-clause Order
坏味道   主要
检查从句顺序
tagOrder
allows to specify the order by tags.
默认值
@author,@version,@param,@return,@throws,@exception,@see,@since,@serial,@serialField,@serialData,@deprecated
target  
allows to specify targets to check at-clauses.
82、Avoid Escaped Unicode Characters
坏味道   主要
避免转义的Unicode字符

allowIfAllCharactersEscaped
Allow if all characters in literal are escaped.
默认值
false
allowNonPrintableEscapes
Allow non-printable escapes.
默认值
false
allowByTailComment
Allow use escapes if trail comment is present.
默认值
false
allowEscapesForControlCharacters
Allow use escapes for non-printable(control) characters.
默认值
false
83、Avoid Inline Conditionals
坏味道   次要
避免内联条件
84、Avoid Nested Blocks
坏味道   主要
避免嵌套块
allowInSwitchCase
Allow nested blocks in case statements. Default is false.
85、Avoid Star Import
坏味道   次要
检查发现使用*符号的导入语句
excludes  
packages where star imports are allowed. Note that this property is not recursive, subpackages of excluded packages are not automatically excluded.
allowStaticMemberImports
whether to allow starred static member imports like <code>import static org.junit.Assert.*;</code>. Default is false.
默认值
false
allowClassImports
whether to allow starred class imports like <code>import java.util.*;</code>. Default is false.
默认值
false
86、Avoid Static Import
坏味道   次要
避免静态导入
87、Boolean checks should not be inverted
坏味道   次要
布尔检查不应该被反转
Noncompliant Code Example


if ( !(a == 2)) { ...}  // Noncompliant
boolean b = !(i < 10);  // Noncompliant
Compliant Solution


if (a != 2) { ...}
boolean b = (i >= 10);
88、Boolean Expression Complexity
坏味道   主要
将嵌套布尔运算符（&&，||和^）限制为指定的深度（默认= 3）。

max
the maximum allowed number of boolean operations in one expression. Default is 3.
默认值
3
tokens  
tokens to check. Default is LAND,BAND,LOR,BOR,BXOR.
默认值
LAND,BAND,LOR,BOR,BXOR
89、Boolean expressions should not be gratuitous
坏味道   主要
如果boolean表达式的值是已定的，那么boolean表达式是没有必要的可以移除
90、Boolean literals should not be redundant
坏味道   次要
boolean不需再与true,false比较作为boolean表达式
91、Branches should have sufficient coverage by tests
坏味道   主要
分支应有足够的测试覆盖

minimumBranchCoverageRatio
默认值
65
92、Case insensitive string comparisons should be made without intermediate upper or lower casing
坏味道   次要
使用toLowerCase（）或toUpperCase（）来使不区分大小写的比较无效，因为它需要创建临时的中间String对象。
93、Catch Parameter Name
坏味道   主要
检查catch参数名是否符合format属性指定的格式

format  
Specifies valid identifiers. Default is ^(e|t|ex|[a-z][a-z][a-zA-Z]+)$
默认值
^(e|t|ex|[a-z][a-z][a-zA-Z]+)$
94、Catches should be combined
坏味道   次要
由于Java 7可以一次捕获多个异常。 因此，当多个catch块具有相同的代码时，它们应该被组合以便更好的可读性，sonar.java.source低于7时，此规则将自动禁用
95、Checked exceptions should not be thrown
坏味道   主要
检查的异常不应该被抛出，要处理
96、Child class fields should not shadow parent class fields
坏味道   阻断
子类字段不应该private父类的非private字段
97、Class Data Abstraction Coupling
坏味道   主要
度量衡量给定类中其他类的实例化数。

max
the maximum threshold allowed. Default is 7.
excludedClasses
User-configured class names to ignore.
excludeClassesRegexps
User-configured regular expressions to ignore classes
excludedPackages
User-configured packages to ignore
98、Class Fan Out Complexity
坏味道   主要
类的依赖类数量

max
the maximum threshold allowed. Default is 20.
excludedClasses
User-configured class names to ignore
excludeClassesRegexps
User-configured regular expressions to ignore classes
excludedPackages
User-configured packages to ignore
99、Class names should comply with a naming convention
坏味道   次要
类名应符合命名约定

format  
Regular expression used to check the class names against.
默认值
^[A-Z][a-zA-Z0-9]*$
100、Class names should not shadow interfaces or superclasses
坏味道   严重
类名称不应该影响接口或超类（相同）
101、Class Type(Generic) Parameter Name
坏味道   主要
泛型参数名称符合指定的格式

format  
Regular expression
默认值
^[A-Z]$
102、Classes and enums with private members should have a constructor
坏味道   主要
有私有成员的类和枚举应该有一个构造函数
103、Classes and methods that rely on the default system encoding should not be used
坏味道   次要
不应使用依赖于默认系统编码的类和方法
104、Classes from "sun.*" packages should not be used
坏味道   主要
不得使用“sun.*”软件包的类,sun类*或com.sun *包被视为实现细节，不属于Java API

Exclude  
Comma separated list of Sun packages to be ignored by this rule. Example: com.sun.jna,sun.misc
105、Classes named like "Exception" should extend "Exception" or a subclass
坏味道   主要
名为“异常”的类应该扩展“异常”或者一个子类
106、Classes should not access their own subclasses during initialization
坏味道   严重
类在初始化期间不应访问自己的子类
107、Classes should not be coupled to too many other classes (Single Responsibility Principle)
坏味道   主要
类不应与太多其他类（单一责任原则）相耦合（依赖）

max
Maximum number of classes a single class is allowed to depend upon
默认值
20
108、Classes should not be empty
坏味道   次要
空类没意义，作为公共扩展点可以作为接口
109、Classes should not be too complex
坏味道   严重  废弃
类不应太复杂

max
Maximum complexity allowed.
默认值
200
110、Classes should not have too many "static" imports
坏味道   主要
静态导入类允许您使用其公共静态成员，而不必使用类名。 这可以很方便，但如果静态导入太多的类，你的代码可能会变得混乱，很难维护

threshold  
The maximum number of static imports allowed
默认值
4
111、Classes should not have too many fields
坏味道   主要
类不应有太多字段
countNonpublicFields
Whether or not to include non-public fields in the count
默认值
true
maximumFieldThreshold
The maximum number of fields
默认值
20
112、Classes should not have too many methods
坏味道   主要
类不应该有太多方法

countNonpublicMethods
Whether or not to include non-public methods in the count.
默认值
true
maximumMethodThreshold
The maximum number of methods authorized in a class.
默认值
35
113、Classes that override "clone" should be "Cloneable" and call "super.clone()"
坏味道   次要
覆盖“克隆”的类应该是“可克隆”，并调用“super.clone（）”
114、Classes with only "static" methods should not be instantiated
坏味道   主要
只有“静态”方法的类不应该被实例化
115、Classes without "public" constructors should be "final"
坏味道   次要
只有私有构造函数的类应该被标记为final，以防止任何错误的扩展尝试。
116、Close curly brace and the next "else", "catch" and "finally" keywords should be located on the same line
坏味道   次要
关闭大括号，下一个“else”，“catch”和“finally”关键字应位于同一行
117、Close curly brace and the next "else", "catch" and "finally" keywords should be on two different lines
坏味道   次要
关闭大括号和下一个“else”，“catch”和“finally”关键字应该在两个不同的行
118、Cognitive Complexity of methods should not be too high
坏味道   严重
认知复杂度是衡量一种方法的控制流程难以理解的度量。 认知复杂性较高的方法难以维持。
Threshold
The maximum authorized complexity.
默认值
15
119、Collapsible "if" statements should be merged
坏味道   主要
可合并的“if”语句应该合并
120、Collection methods with O(n) performance should be used carefully
坏味道   次要
应仔细使用具有O（n）性能的集合方法
121、Collection.isEmpty() should be used to test for emptiness
坏味道   次要
应该使用Collection.isEmpty（）来测试空集合
122、Comment pattern matcher
坏味道   次要
该规则允许在TODO，NOPMD，...之外的任何类型的内容中找到任何类型的模式，NOSONAR除外
123、Comments Indentation
坏味道   次要
注释缩进

tokens  
tokens to check
默认值
SINGLE_LINE_COMMENT,BLOCK_COMMENT_BEGIN
124、Comments should not be located at the end of lines of code
坏味道   次要
注释不应位于代码行的末尾

legalTrailingCommentPattern
Description Pattern for text of trailing comments that are allowed. By default, comments containing only one word.
默认值
^\s*+[^\s]++$
125、Comparators should be "Serializable"
坏味道   严重
Comparators should be "Serializable"
126、Conditionals should start on new lines
坏味道   严重
条件表达式应该起始新行
127、Constant Name
坏味道   次要
检查常数名称是否符合指定的格式
applyToPackage
Controls whether to apply the check to package-private member
默认值
true
format  
Regular expression
默认值
^[A-Z][A-Z0-9]*(_[A-Z0-9]+)*$
applyToPublic  
Controls whether to apply the check to public member
默认值
true
applyToProtected
Controls whether to apply the check to protected member
默认值
true
applyToPrivate
Controls whether to apply the check to private member
默认值
true
128、Constant names should comply with a naming convention
坏味道   严重
常数名称应符合命名约定

format  
Regular expression used to check the constant names against.
默认值
^[A-Z][A-Z0-9]*(_[A-Z0-9]+)*$
129、Constants should not be defined in interfaces
坏味道   严重
常量不应在接口中定义
130、Constructors should not be used to instantiate "String" and primitive-wrapper classes
坏味道   主要
构造函数不应用于实例化“String”和原始包装类
131、Constructors should only call non-overridable methods
坏味道   严重
构造函数只应该调用不可覆盖的方法
132、Control flow statements "if", "for", "while", "switch" and "try" should not be nested too deeply
坏味道   严重
控制流程语句“if”，“for”，“while”，“switch”和“try”不能嵌套太深

max
Maximum allowed control flow statement nesting depth.
默认值
3
133、Control structures should use curly braces
坏味道   严重
控制结构应使用花括号
134、Covariant Equals
坏味道   严重
检查一个类是否定义了一个协变方法equals，那么它定义了方法equals（java.lang.Object）
135、Custom Import Order
坏味道   主要
检查导入声明组按照用户指定的顺序显示。 如果有导入，但是在组态中未指定其组，则导入应放在导入列表的末尾。
thirdPartyPackageRegExp
RegExp for THIRDPARTY_PACKAGE group imports.
默认值
^$
separateLineBetweenGroups
Force empty line separator between import groups.
默认值
true
sortImportsInGroupAlphabetically
Force grouping alphabetically.
默认值
false
specialImportsRegExp
RegExp for SPECIAL_IMPORTS group imports.
默认值
^$
customImportOrderRules
List of order declaration customizing by user.
standardPackageRegExp
RegExp for STANDARD_JAVA_PACKAGE group imports.
默认值
java|javax
136、Cyclomatic Complexity
坏味道   主要
检查针对特定限制的方法的循环复杂性

switchBlockAsSingleDecisionPoint
whether to treat the whole switch block as a single decision point
默认值
false
max
the maximum threshold allowed.
默认值
10
tokens  
tokens to check
默认值
LITERAL_WHILE,LITERAL_DO,LITERAL_FOR,LITERAL_IF,LITERAL_SWITCH,LITERAL_CASE,LITERAL_CATCH,QUESTION,LAND,LOR
137、Dead stores should be removed
坏味道   主要
没用的存储应该除移
138、Declaration Order
坏味道   提示
声名的顺序
ignoreModifiers
Whether to ignore modifiers
默认值
false
ignoreConstructors
Whether to ignore constructors
默认值
false
139、Declarations should use Java collection interfaces such as "List" rather than specific implementation classes such as "LinkedList"
坏味道   次要
声明应该使用Java集合接口，例如“List”，而不是特定的实现类，如“LinkedList”
140、Default annotation parameter values should not be passed as arguments
坏味道   次要
默认注解参数值不应作为参数传递
141、Default Comes Last
坏味道   主要
检查在switch语句中的所有情况之后的默认值。

skipIfLastAndSharedWithCase
whether to allow default along with case if they are not last
默认值
false
142、Deprecated "${pom}" properties should not be used
坏味道   次要
不应使用不推荐使用的“$ {pom}”属性
143、Deprecated code should be removed
坏味道   提示
应删除弃用的代码
144、Deprecated elements should have both the annotation and the Javadoc tag
坏味道   主要
弃用元素应有注解和doc标签
145、Descendant Token
坏味道   次要
检查其他令牌下的限制令牌
maximumMessage
error message when the maximum count is exceeded
maximumDepth  
the maximum depth for descendant counts
limitedTokens  
set of tokens with limited occurrences as descendants
maximumNumber  
a maximum count for descendants
minimumMessage
error message when the maximum count is exceeded
minimumNumber  
a minimum count for descendants
minimumDepth  
the minimum depth for descendant counts
sumTokenCounts
whether the number of tokens found should be calculated from the sum of the individual token counts
默认值
false
146、Design For Extension
坏味道   次要
扩展设计

ignoredAnnotations
Annotations which allow the check to skip the method from validation.
默认值
Test,Before,After,BeforeClass,AfterClass
147、EJB interceptor exclusions should be declared as annotations
坏味道   阻断
EJB interceptor exclusions应该以注解的形式使用
148、Empty arrays and collections should be returned instead of null
坏味道   主要
应该返回空数组和集合，而不是null
149、Empty Block
坏味道   主要
Checks for empty blocks
tokens
blocks to check
默认值
LITERAL_WHILE,LITERAL_TRY,LITERAL_FINALLY,LITERAL_DO,LITERAL_IF,LITERAL_ELSE,LITERAL_FOR,INSTANCE_INIT,STATIC_INIT,LITERAL_SWITCH,LITERAL_SYNCHRONIZED
option  
policy on block contents
默认值
stmt
150、Empty catch block
坏味道   主要
检查空的catch块。 有两个选项可以使验证更加精确（默认情况下，检查允许空的catch块和任何注释）

exceptionVariableName
Format of skipping exception''s variable name.
默认值
^$
commentFormat  
Format of comment.
默认值
.*
151、Empty For Initializer Pad
坏味道   次要
检查初始化程序为空的填充; 那是空的是否需要一个空的初始化程序，或者禁止这样的空格。 示例：for（; i <j; i ++，j--）

option  
policy on how to pad an empty for iterator
152、Empty For Iterator Pad
坏味道   次要
检查一个空的填充迭代器; 那就是空格是否需要一个空的迭代器，否则这样的空格是被禁止的。 示例：for（Iterator foo = very.long.line.iterator（）; foo.hasNext（）;）

option  
policy on how to pad an empty for iterator
153、Empty Line Separator
坏味道   主要
在标题，包，所有导入声明，字段，构造函数，方法，嵌套类，静态初始化器和实例初始化器之后检查空行分隔符
allowNoEmptyLineBetweenFields
Allow no empty line between fields
默认值
false
allowMultipleEmptyLines
Allows multiple empty lines between class members.
默认值
true
tokens  
assignments to check
默认值
PACKAGE_DEF,IMPORT,CLASS_DEF,INTERFACE_DEF,ENUM_DEF,STATIC_INIT,INSTANCE_INIT,METHOD_DEF,CTOR_DEF,VARIABLE_DEF
allowMultipleEmptyLinesInsideClassMembers
Allow multiple empty lines inside class members
默认值
true
154、Empty Statement
坏味道   次要
检测空的语句（独立的';'）。
155、Empty statements should be removed
坏味道   次要
移除空语句
156、Enumeration should not be implemented
坏味道   主要
不应实同Enumeration
157、Equality operators should not be used in "for" loop termination conditions
坏味道   严重
循环终止条件下不应使用平等运算符
158、Equals Avoid Null
坏味道   主要
159、Escaped Unicode characters should not be used
坏味道   主要
不应使用转义的Unicode字符
160、Exception classes should be immutable
坏味道   次要
异常类应该是不可变的
161、Exception handlers should preserve the original exceptions
坏味道   主要
异常处理程序应保留原始异常
162、Exception types should not be tested using "instanceof" in catch blocks
坏味道   主要
异常类型不应该在catch块中使用“instanceof”进行测试
163、Exceptions should not be thrown in finally blocks
坏味道   严重
异常不应该在finally块中抛出
164、Executable Statement Count
坏味道   主要
将可执行语句的数量限制为指定的限制（默认= 30）。

max
the maximum threshold allowed. Default is 30.
默认值
30
tokens  
members to check
默认值
CTOR_DEF,METHOD_DEF,INSTANCE_INIT,STATIC_INIT
165、Execution of the Garbage Collector should be triggered only by the JVM
坏味道   严重
垃圾收集器的执行只能由JVM触发
166、Exit methods should not be called
坏味道   阻断
调用System.exit（int status）或Rutime.getRuntime（）。exit（int status）调用关闭挂钩并关闭整个Java虚拟机。 调用Runtime.getRuntime（）。halt（int）立即关闭，而不调用关闭挂钩，并跳过完成
167、Explicit Initialization
坏味道   主要
检查任何类或对象成员是否明确地初始化为其类型值的默认值（对于对象引用为空，数字类型为零，对于布尔为char为false）。
168、Expressions should not be too complex
坏味道   严重
表达式不应太复杂

max
Maximum number of allowed conditional operators in an expression
默认值
3
169、Extensions and implementations should not be redundant
坏味道   次要
扩展和实现不应该是多余的
170、Fall Through
坏味道   主要
171、Field names should comply with a naming convention
坏味道   次要
字段名称应符合命名约定
format
Regular expression used to check the field names against.
默认值
^[a-z][a-zA-Z0-9]*$
172、Fields in a "Serializable" class should either be transient or serializable
坏味道   严重
“Serializable”类中的字段应该是transient或可序列化的
173、Fields in non-serializable classes should not be "transient"
坏味道   次要
不可序列化类的字段不应该是“transient”
174、Fields should not be initialized to default values
坏味道   次要
不应将字段初始化为默认值
175、File Contents Holder
坏味道   次要
配置为TreeWalker子模块时，保留当前的全局访问文件内容。 例如，过滤器可以通过此模块访问当前文件内容
176、File Length
坏味道   主要
如果源文件变得很长，那么很难理解。 因此，长类通常应该重构到专注于特定任务的几个单独的类中

fileExtensions
file type extension of files to process
max
maximum allowable number of lines. Default is 2000.
177、File Tab Character
坏味道   次要
检查源代码中没有制表符（'\ t'）

fileExtensions
file type extension of files to process
eachLine  
whether to report on each line containing a tab, or just the first instance. Default is false.
178、Files should contain an empty new line at the end
坏味道   次要
文件最后应该包含一个空的新行
179、Files should contain only one top-level class or interface each
坏味道   主要
文件应该只包含一个顶级类或接口
180、Files should not be empty
坏味道   次要
删除空文件
181、Files should not have too many lines of code
坏味道   主要
源码文件代码行数检查
182、Modifiers should be declared in the correct order
坏味道   次要
Java语言规范建议按以下顺序列出修饰符：
1. Annotations
2. public
3. protected
4. private
5. abstract
6. static
7. final
8. transient
9. volatile
10. synchronized
11. native
12. strictfp
183、Sections of code should not be "commented out"
坏味道   主要
不要有注掉的代码，影响可读性，可以删除
184、Strings should not be concatenated using '+' in a loop
坏味道   次要
用StringBuilder代替String拼接
185、String function use should be optimized for single characters
坏味道   主要
字符串方法操作中单字符建议优先用单引号
186、Unused local variables should be removed
坏味道   次要
如果一个局部变量被声明但未被使用，那么它是死代码，应该被删除。 这样做会提高可维护性，因为开发人员不会想知道使用什么变量
187、The diamond operator ("<>") should be used
坏味道   次要
Java 7引入了操作符（<>）来减少泛型代码的冗长度。
List<String> strings = new ArrayList<>()
188、Useless imports should be removed
坏味道   次要
不要导入没有用到的导入
189、Source files should not have any duplicated blocks
坏味道   主要
源文件不应有任何重复块
190、Only static class initializers should be used
坏味道   主要
静态代码块应加，static标识
191、Generic exceptions should never be thrown
坏味道   主要
通用异常如Error, RuntimeException, Throwable, and Exception不应抛出,应定义和抛出一个专门的异常，而不是使用通用异常
192、Method names should comply with a naming convention
坏味道   次要
方法名称应符合命名约定
默认规则：^[a-z][a-zA-Z0-9]*$
193、Synchronized classes Vector, Hashtable, Stack and StringBuffer should not be used
坏味道   主要
Java API的早期类，例如Vector，Hashtable和StringBuffer已被同步，使其成为线程安全的。 不幸的是，即使从单个线程使用这些集合，同步也会对性能产生很大的负面影响
194、Standard outputs should not be used directly to log anything
坏味道   主要
用日志记录代替标准输出
195、Local variable and method parameter names should comply with a naming convention
坏味道   次要
局部变量和方法参数名称应符合命名约定
默认:^[a-z][a-zA-Z0-9]*$
196、Local Variables should not be declared and then immediately returned or thrown
坏味道   次要
声明一个变量只是立即返回或抛出它是一个糟糕的做法
197、Instance methods should not write to "static" fields
坏味道   严重
静态属性更新需同步
198、Methods should not be empty
坏味道   严重
不要存在空方法
199、Utility classes should not have public constructors
坏味道   主要
帮助类不应该有公共构造函数，帮助类不宜实例化，且应该有一个如下的私有构造方法
private StringUtils() {
throw new IllegalStateException("Utility class");
}
200、Static non-final field names should comply with a naming convention
坏味道   次要
静态非最终字段名称应符合命名约定
默认:^[a-z][a-zA-Z0-9]*$
201、Methods returns should not be invariant
坏味道   阻断
方法返回值不应该是相同的值
202、Return of boolean expressions should not be wrapped into an "if-then-else" statement
坏味道   次要
可以根据boolean表达式就能返回的直接返回boolean表达式，不需要if-then-else语句
203、Try-with-resources should be used
坏味道   严重
Try-with-resources代替try-catch-finally  
204、String literals should not be duplicated
坏味道   严重
重复的字符串文字会使重构过程容易出错，因为您必须确保更新所有

threshold  
Number of times a literal must be duplicated to trigger an issue
默认值
3
205、




坏味道


可调整:


1、Abbreviation As Word In Name    (默认 关闭)
坏味道   主要
检查验证标识符名称中的缩写（连续大写字母）长度，还允许执行骆驼案例命名
allowedAbbreviationLength 3
6、Annotation Location             (默认 关闭)
坏味道   主要
注释位置
allowSamelineSingleParameterlessAnnotation  
To allow single parameterless annotation to be located on the same line as target element.
默认值
true
allowSamelineParameterizedAnnotation
To allow parameterized annotation to be located on the same line as target element.
默认值
false
allowSamelineMultipleAnnotations
To allow annotation to be located on the same line as target element.
默认值
false
tokens  
tokens to check
默认值
CLASS_DEF,INTERFACE_DEF,ENUM_DEF,METHOD_DEF,CTOR_DEF,VARIABLE_DEF
7、Annotation Use Style                 (默认 关闭)
坏味道   主要

trailingArrayComma
Defines the policy for trailing comma in arrays. Default is never.
closingParens  
Defines the policy for ending parenthesis. Default is never.
elementStyle  
Defines the annotation element styles. Default value is compact_no_array.
8、Artifact ids should follow a naming convention     (默认 关闭)
坏味道   次要
共享命名约定允许团队有效协作。 当pom的artifactId与提供的正则表达式不匹配时，此规则引发了一个问题

regex
The regular expression the "artifactId" should match
默认值
[a-z][a-z-0-9]+
9、At-clause Order      (默认 关闭)  
坏味道   主要
检查从句顺序
tagOrder
allows to specify the order by tags.
默认值
@author,@version,@param,@return,@throws,@exception,@see,@since,@serial,@serialField,@serialData,@deprecated
target  
allows to specify targets to check at-clauses.
10、Avoid Escaped Unicode Characters   (默认 关闭)
坏味道   主要
避免转义的Unicode字符

allowIfAllCharactersEscaped
Allow if all characters in literal are escaped.
默认值
false
allowNonPrintableEscapes
Allow non-printable escapes.
默认值
false
allowByTailComment
Allow use escapes if trail comment is present.
默认值
false
allowEscapesForControlCharacters
Allow use escapes for non-printable(control) characters.
默认值
false
11、Avoid Nested Blocks       (默认 关闭)
坏味道   主要
避免嵌套块
allowInSwitchCase
Allow nested blocks in case statements. Default is false.
12、Avoid Star Import       (默认 关闭)
坏味道   次要
检查发现使用*符号的导入语句
excludes  
packages where star imports are allowed. Note that this property is not recursive, subpackages of excluded packages are not automatically excluded.
allowStaticMemberImports
whether to allow starred static member imports like <code>import static org.junit.Assert.*;</code>. Default is false.
默认值
false
allowClassImports
whether to allow starred class imports like <code>import java.util.*;</code>. Default is false.
默认值
false
13、Boolean Expression Complexity     (默认 关闭)
坏味道   主要
将嵌套布尔运算符（&&，||和^）限制为指定的深度（默认= 3）。

max
the maximum allowed number of boolean operations in one expression. Default is 3.
默认值
3
tokens  
tokens to check. Default is LAND,BAND,LOR,BOR,BXOR.
默认值
LAND,BAND,LOR,BOR,BXOR
14、Branches should have sufficient coverage by tests    (默认 关闭)
坏味道   主要
分支应有足够的测试覆盖

minimumBranchCoverageRatio
默认值
65
15、Catch Parameter Name       (默认 关闭)
坏味道   主要
检查catch参数名是否符合format属性指定的格式

format  
Specifies valid identifiers. Default is ^(e|t|ex|[a-z][a-z][a-zA-Z]+)$
默认值
^(e|t|ex|[a-z][a-z][a-zA-Z]+)$
16、Class Data Abstraction Coupling     (默认 关闭)
坏味道   主要
度量衡量给定类中其他类的实例化数。

max
the maximum threshold allowed. Default is 7.
excludedClasses
User-configured class names to ignore.
excludeClassesRegexps
User-configured regular expressions to ignore classes
excludedPackages
User-configured packages to ignore
17、Class Fan Out Complexity      (默认 关闭)
坏味道   主要
类的依赖类数量

max
the maximum threshold allowed. Default is 20.
excludedClasses
User-configured class names to ignore
excludeClassesRegexps
User-configured regular expressions to ignore classes
excludedPackages
User-configured packages to ignore
18、Class names should comply with a naming convention   (开放)
坏味道   次要
类名应符合命名约定

format  
Regular expression used to check the class names against.
默认值
^[A-Z][a-zA-Z0-9]*$
19、Classes from "sun.*" packages should not be used    (开放)
坏味道   主要
不得使用“sun.*”软件包的类,sun类*或com.sun *包被视为实现细节，不属于Java API

Exclude  
Comma separated list of Sun packages to be ignored by this rule. Example: com.sun.jna,sun.misc   
20、Classes should not be coupled to too many other classes (Single Responsibility Principle)      (默认 关闭)
坏味道   主要
类不应与太多其他类（单一责任原则）相耦合（依赖）

max
Maximum number of classes a single class is allowed to depend upon
默认值
20
21、Classes should not be too complex         (默认 关闭)
坏味道   严重  废弃
类不应太复杂

max
Maximum complexity allowed.
默认值
200
22、Classes should not have too many "static" imports       (默认 关闭)
坏味道   主要
静态导入类允许您使用其公共静态成员，而不必使用类名。 这可以很方便，但如果静态导入太多的类，你的代码可能会变得混乱，很难维护

threshold  
The maximum number of static imports allowed
默认值
4
23、Classes should not have too many fields     (默认 关闭)
坏味道   主要
类不应有太多字段
countNonpublicFields
Whether or not to include non-public fields in the count
默认值
true
maximumFieldThreshold
The maximum number of fields
默认值
20
24、Classes should not have too many methods      (默认 关闭)
坏味道   主要
类不应该有太多方法

countNonpublicMethods
Whether or not to include non-public methods in the count.
默认值
true
maximumMethodThreshold
The maximum number of methods authorized in a class.
默认值
35
25、Close curly brace and the next "else", "catch" and "finally" keywords should be located on the same line    (默认 关闭)
坏味道   次要
关闭大括号，下一个“else”，“catch”和“finally”关键字应位于同一行
26、Close curly brace and the next "else", "catch" and "finally" keywords should be on two different lines    (默认 关闭)
坏味道   次要
关闭大括号和下一个“else”，“catch”和“finally”关键字应该在两个不同的行
29、Comments should not be located at the end of lines of code    (默认 关闭)
坏味道   次要
注释不应位于代码行的末尾

legalTrailingCommentPattern
Description Pattern for text of trailing comments that are allowed. By default, comments containing only one word.
默认值
^\s*+[^\s]++$
30、Constant Name    (默认 关闭)
坏味道   次要
检查常数名称是否符合指定的格式
applyToPackage
Controls whether to apply the check to package-private member
默认值
true
format  
Regular expression
默认值
^[A-Z][A-Z0-9]*(_[A-Z0-9]+)*$
applyToPublic  
Controls whether to apply the check to public member
默认值
true
applyToProtected
Controls whether to apply the check to protected member
默认值
true
applyToPrivate
Controls whether to apply the check to private member
默认值
true
31、Control flow statements "if", "for", "while", "switch" and "try" should not be nested too deeply   (默认 关闭)
坏味道   严重
控制流程语句“if”，“for”，“while”，“switch”和“try”不能嵌套太深

max
Maximum allowed control flow statement nesting depth.
默认值
3
32、Custom Import Order  (默认 关闭)
坏味道   主要
检查导入声明组按照用户指定的顺序显示。 如果有导入，但是在组态中未指定其组，则导入应放在导入列表的末尾。
thirdPartyPackageRegExp
RegExp for THIRDPARTY_PACKAGE group imports.
默认值
^$
separateLineBetweenGroups
Force empty line separator between import groups.
默认值
true
sortImportsInGroupAlphabetically
Force grouping alphabetically.
默认值
false
specialImportsRegExp
RegExp for SPECIAL_IMPORTS group imports.
默认值
^$
customImportOrderRules
List of order declaration customizing by user.
standardPackageRegExp
RegExp for STANDARD_JAVA_PACKAGE group imports.
默认值
java|javax
33、Cyclomatic Complexity  (默认 关闭)
坏味道   主要
检查针对特定限制的方法的循环复杂性

switchBlockAsSingleDecisionPoint
whether to treat the whole switch block as a single decision point
默认值
false
max
the maximum threshold allowed.
默认值
10
tokens  
tokens to check
默认值
LITERAL_WHILE,LITERAL_DO,LITERAL_FOR,LITERAL_IF,LITERAL_SWITCH,LITERAL_CASE,LITERAL_CATCH,QUESTION,LAND,LOR
34、Default Comes Last (默认 关闭)
坏味道   主要
检查在switch语句中的所有情况之后的默认值。

skipIfLastAndSharedWithCase
whether to allow default along with case if they are not last
默认值
false
35、Empty catch block (默认 关闭)
坏味道   主要
检查空的catch块。 有两个选项可以使验证更加精确（默认情况下，检查允许空的catch块和任何注释）

exceptionVariableName
Format of skipping exception''s variable name.
默认值
^$
commentFormat  
Format of comment.
默认值
.*
36、Empty For Initializer Pad   (默认 关闭)
坏味道   次要
检查初始化程序为空的填充; 那是空的是否需要一个空的初始化程序，或者禁止这样的空格。 示例：for（; i <j; i ++，j--）

option  
policy on how to pad an empty for iterator
37、Empty For Iterator Pad
坏味道   次要
检查一个空的填充迭代器; 那就是空格是否需要一个空的迭代器，否则这样的空格是被禁止的。 示例：for（Iterator foo = very.long.line.iterator（）; foo.hasNext（）;）

option  
policy on how to pad an empty for iterator
38、Empty Line Separator   (默认 关闭)
坏味道   主要
在标题，包，所有导入声明，字段，构造函数，方法，嵌套类，静态初始化器和实例初始化器之后检查空行分隔符
allowNoEmptyLineBetweenFields
Allow no empty line between fields
默认值
false
allowMultipleEmptyLines
Allows multiple empty lines between class members.
默认值
true
tokens  
assignments to check
默认值
PACKAGE_DEF,IMPORT,CLASS_DEF,INTERFACE_DEF,ENUM_DEF,STATIC_INIT,INSTANCE_INIT,METHOD_DEF,CTOR_DEF,VARIABLE_DEF
allowMultipleEmptyLinesInsideClassMembers
Allow multiple empty lines inside class members
默认值
true
39、Executable Statement Count  (默认 关闭)
坏味道   主要
将可执行语句的数量限制为指定的限制（默认= 30）。

max
the maximum threshold allowed. Default is 30.
默认值
30
tokens  
members to check
默认值
CTOR_DEF,METHOD_DEF,INSTANCE_INIT,STATIC_INIT
40、Expressions should not be too complex (默认 关闭)
坏味道   严重
表达式不应太复杂

max
Maximum number of allowed conditional operators in an expression
默认值
3
42、File Length    (默认 关闭)
坏味道   主要
如果源文件变得很长，那么很难理解。 因此，长类通常应该重构到专注于特定任务的几个单独的类中

fileExtensions
file type extension of files to process
max
maximum allowable number of lines. Default is 2000.
43、File Tab Character  (默认 关闭)
坏味道   次要
检查源代码中没有制表符（'\ t'）

fileExtensions
file type extension of files to process
eachLine  
whether to report on each line containing a tab, or just the first instance. Default is false.
44、Files should contain an empty new line at the end  (默认 关闭)
坏味道   次要
文件最后应该包含一个空的新行

45、Add a private constructor to hide the implicit public one.
工具类不应该有默认或者公共的构造函数，也就是说这个类里可能方法都是static，那就不需要构造它的实例，因此应该给加一个private的构造函数，就不会报这个错了。
a class which only has private constructors should be final
例如上一个，加了private构造函数，又会出这个，把class设置成final即可。例：
1
public class Shape {
2
private Shape() {
3
/* set something here */
4
}
5

6
public static Shape makeShape(/* arglist */) {
7
System.out.println("here is the shape you ordered");
8
return (new Shape());
9
}
10
}


已调整：


1、String literals should not be duplicated      (调整)
坏味道   严重
重复的字符串文字会使重构过程容易出错，因为您必须确保更新所有

threshold  
Number of times a literal must be duplicated to trigger an issue
默认值
3   调整为 5






已关闭：


1、Utility classes should not have public constructors          (关闭)
坏味道   主要
帮助类不应该有公共构造函数，帮助类不宜实例化，且应该有一个如下的私有构造方法
private StringUtils() {
throw new IllegalStateException("Utility class");
}
2、Methods returns should not be invariant  (关闭)
坏味道   阻断
方法返回值不应该是相同的值
3、Return of boolean expressions should not be wrapped into an "if-then-else" statement    (关闭)
坏味道   次要
可以根据boolean表达式就能返回的直接返回boolean表达式，不需要if-then-else语句
4、The diamond operator ("<>") should be used     (关闭)   jdk7+可用
坏味道   次要
Java 7引入了操作符（<>）来减少泛型代码的冗长度
5、Sections of code should not be "commented out"    (关闭)
坏味道   主要
不要有注掉的代码，影响可读性，可以删除
6、Try-with-resources should be used    (关闭)   jdk7+可用
坏味道   严重
Try-with-resources代替try-catch-finally
7、Loops should not be infinite         (关闭)  
Bug   阻断
循环不应该是无限的
8、Credentials should not be hard-coded    (关闭)  
漏洞 阻断
凭证不应该硬编码
9、Anonymous inner classes containing only one method should become lambdas  (关闭)  
坏味道   主要
只有一个方法的匿名内部类应该变成lambdas
10、"throws" declarations should not be superfluous    (关闭)  抛出运行时异常，有的框架接口即抛出此类异常
坏味道   次要
“抛出”声明不应该是多余的
11、IP addresses should not be hardcoded   (关闭)
漏洞 次要
ip 地址不应该硬编码
12、"@Override" should be used on overriding and implementing methods   (关闭)  
坏味道   主要
重写的和实现在方法要加Override标注
13、An open curly brace should be located at the beginning of a line    (关闭)  
坏味道   次要
开放的大括号应位于一行的开头
14、Cognitive Complexity of methods should not be too high  (关闭)
坏味道   严重
认知复杂度是衡量一种方法的控制流程难以理解的度量。 认知复杂性较高的方法难以维持。
Threshold
The maximum authorized complexity.
默认值
```