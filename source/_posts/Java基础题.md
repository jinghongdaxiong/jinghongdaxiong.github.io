---
title: Java基础题
date: 2020-04-19 16:50:24
tags: [java,面试]
categories: java
---

# 1.以下哪个是finalize()方法的正确形式？
    A.protected void finalize() throws Throwable
    B.final finalize()
    C.public final finalize()
    D.private boolean finalize()
    答案：A
    
#    2.如果finalize（）方法抛出一个运行时异常，以下哪个描述正确？
    A.正在运行的应用程序系统崩溃
    B.此异常被忽略，并且该异常对象被垃圾回收器回收
    C.此异常被忽略，但是该异常对象未被垃圾回收期回收
    D.此异常导致JVM崩溃
    答案：B
    答案解析：finalize()方法只是用于清除对象，而不是实际的销毁对象，因此对该方法的调用不会引起系统崩溃，该方法抛出的异常也会作为废弃对象被垃圾回收期回收
    
#    3.如何释放掉一个对象占据的内存空间？
    A.调用free()方法
    B.调用System.gc()方法
    C.赋值给该对象的引用为null
    D.程序员无法明确强制垃圾回收器运行
    答案：D
    
#    4.给出以下代码：
    1.public class Example {
    2. public static void main(String[] args) {
    3.  String s = "abcd";
    4.  Integer x = new Integer(3);
    5.  String s2 = s + 4;
    6.  s2 = null;
    7.  s = null;
    8. }
    9.}
    改程序运行到第几行变量S2引用的对象符合垃圾回收器回收条件？
    A.第7行
    B.不存在
    C.第6行
    D.直到main线程结束，S2应用的对象才可能被回收
    答案：C
    
#    5.以下代码运行到关键点处，有多少对象符合垃圾回收的条件？
    public class Example {
     public static void main(String[] args) {
      String name;
      String newName = "Nick";
      newName = "Jason";
      name = "Frieda";
      String newestName = name;
      name = null;
      // 关键点
     }
    }
    A.0个
    B.1个
    C.2个
    D.3个
    答案：B
    
# 6.以下哪些是有关垃圾回收器的正确描述？
    A.程序员可以在制定时间调用垃圾回收器释放内存
    B.垃圾回收器可以保证Java程序不会产生内存溢出
    C.程序员可以制定垃圾回收器回收对象
    D.对象的finalize()方法在对象被垃圾回收器回收之前获得调用
    答案：C、D
    答案解析：通过通配符*号引入的两个不同包中存在同名的类，当代码中不加包名直接使用时，会产生编译错误，使用时需要提供完整包路径
    
 # 7.拥有下列哪些引用类型的对象在虚拟机内存足够的情况下不会被垃圾回收机制回收？
    A.强引用
    B.软引用
    C.弱引用
    D.虚引用
    答案：A、B
    
    
 # 8.finalize和C++ 的 destructors有何差异?
    答案：Java内部具有“内存使用回收” 的机制， 虽然它也提供了类似 (C++ 的) destructors的 finalize()，每个对象都可以使用这个方法 method，但必须冒著破坏原先回收机制的危险。所以建议尽量避免使用finalize()，不妨考虑多使用引用队列来释出资源会好一些。
    
   # 9.Java语言中异常的分类是哪项？
    A.运行时异常和异常
    B.受检异常和非受检异常
    C.错误和异常
    D.错误和运行时异常
    答案：C
    
# 10.所有异常的父类是哪项？
    A.Throwable
    B.Error
    C.RuntimeException
    D.Exception
    答案：A
    
#     11.下列属于非受检异常（运行时异常）的是哪项？
    A.SQLException
    B.IOException
    C.NullPointerException
    D.OutOfMemoryError
    答案：C
    
#     12.假设有自定义异常类ServiceException,那么抛出该异常的语句正确的是哪项？
    A.raise ServiceException
    B.throw new ServiceException()
    C.throw ServiceException
    D.throws ServiceException
    答案：B
    
#     13.在方法声明中，说明该方法可能会抛出的异常列表时使用哪个关键字？
    A.throw 
    B.catch
    C.finally
    D.throws
    答案：D
    
#     14.现有代码：
    public class Example {
     public static void main(String[] args) {
      try {
       System.out.print(Integer.parseInt("forty"));   
      } catch (RuntimeException e) {
       System.out.println("Runtime");
      }catch (NumberFormatException e) {
       System.out.println("Number");
      }
     }
    }
    执行结果是什么？
    A.输出Number
    B.输出Runtime
    C.输出40
    D.编译失败
    答案：D
    答案解析：NumberFormatException是RuntimeException的子类，因此两个catch块位置应该交换才能正确处理异常
    
#     15.现有代码如下：
    public class Example {
    
     void topGo() {
      try {
       middleGo();
      } catch (Exception e) {
       System.out.println("catch");
      }
     }
    
     void middleGo() throws Exception {
      go();
      System.out.println("late middle");
     }
    
     void go() throws Exception {
      throw new Exception();
     }
    
     public static void main(String[] args) {
      Example example = new Example();
      example.topGo();
     }
    }
    该代码的执行结果是？
    A.输出late middle
    B.输出catch
    C.输出late middle catch
    D.输出catch late middle
    答案：B
    
#     16.如下代码执行后的输出结果是？
    public class Example {
     public static void main(String[] args) {
      try {
       throw new Exception();
      } catch (Exception e) {
       try {
        throw new Exception();
       } catch (Exception e2) {
        System.out.println("inner");
       }
       System.out.println("middle");
      }
      System.out.println("out");
     }
    }
    A.inner outer
    B.middle outer
    C.inner middle outer
    D.编译失败
    答案：C
    
#     17.现有如下代码：
    public class Example extends Utils{
     public static void main(String[] args) {
      try {
       System.out.println(new Example().getInt("42"));
      } catch (NumberFormatException e) {
       System.out.println("NFExc");
      }
     } 
     int getInt(String arg) throws NumberFormatException{
      return Integer.parseInt(arg);
     }
    }
    
    class Utils {
     int getInt(String arg) {
      return 42;
     }
    }
    该代码执行的结果是？
    A.NFExc
    B.42.0
    C.42NFExc
    D.编译失败
    答案：B
    答案解析：Utils中的getInt方法没有抛出异常，而子类Example中的getInt抛出了运行时异常，这是符合方法覆盖的抛出异常特性规范的，因为运行时异常并不会强制要求方法调用代码捕获处理
    
#     18.现有如下代码：
    public class Example extends Utils{
     public static void main(String[] args) {
      try {
       System.out.println(new Example().getInt("42"));
      } catch (NumberFormatException e) {
       System.out.println("NFExc");
      }
     } 
     int getInt(String arg) throws Exception{
      return Integer.parseInt(arg);
     }
    }
    
    class Utils {
     int getInt(String arg) {
      return 42;
     }
    }
    该代码执行的结果是？
    A.NFExc
    B.42.0
    C.42NFExc
    D.编译失败
    答案：D
    答案解析：子类抛出的异常不符合方法覆盖的异常列表要求，因此编译失败（见上题）
    
#     19.现有如下代码：
    public class Example {
     public static void main(String[] args) {// a
      new Example().topGo();
     }
    
     void topGo() {// b
      middleGo();
     }
    
     void middleGo() {// c
      go();
      System.out.println("late middle");
     }
    
     void go() {// d
      throw new Exception();
     }
    }
    为了使代码能够编译通过，需要在哪个地方加入声明throws Exception?
    A.d
    B.c和d
    C.b、c和d
    D.a、b、c和d
    答案：D
    
#     20.下面代码的执行结果是？
    class Example extends Utils {
     public static void main(String[] args) {
      try {
       System.out.print(new Example().getlnt("42"));
      } catch (Exception e) {
       System.out.println("Exc");
      }
     }
    
     int getlnt(String arg) throws Exception {
      return Integer.parseInt(arg);
     }
    }
    
    class Utils {
     int getlnt() {
      return 42;
     }
    }
    A.NFExc
    B.42.0
    C.42NFExc
    D.编译失败
    答案：B
    答案解析：本题没有实现方法覆盖
    
#     21.关于异常处理，说法错误的是？
    A.try⋯catch⋯finally结构中，必须有try语句块，catch语句块和finally语句块不是必须的，但至少要两者取其一
    B.在异常处理中，若try中的代码可能产生多种异常则可以对应多个catch语句，若catch中的参数类型有父类子类关系，此时应该将子类放在后面，父类放在前面
    C.一个方法可以抛出多个异常，方法的返回值也能够是异常
    D.Throwable是所有异常的超类
    答案：B
    答案解析：若catch中的参数类型有父类子类关系，此时应该将子类放在前面，父类放在后面
    
#     22.以下关于Error和Exception类的描述正确的是？
    A.Error类和Exception类都是Throwable类的子类
    B.Error类是一个final类，而Exception类是一个非final类
    C.Exception类是一个final类，而Error类是一个非final类
    D.Error类和Exception类都实现了Throwable接口
    答案：A
    
#     23.请问以下哪个是声明一个方法抛出异常的正确形式？
    A.void m() throws IOException{}
    B.void m() throw IOException
    C.void m(){} throws IOException
    D.void m(void) throw IOException{}
    答案：A
    
    
 #    24.请问以下哪些关于try⋯catch⋯finally结构中的finally语句的描述是正确的？
    A.只有当一个catch语句获得执行后，finally语句才获得执行
    B.只有当catch语句未获得执行时，finally语句才获得执行
    C.如果有finally语句，return语句将在finally语句执行完毕后才会返回
    D.只有当异常抛出时，finally语句才获得执行
    答案：C
    
    
 #    25.请问以下代码的直接执行结果是？
    class Example{
     public static void main(String[] args) {
      try {
       System.out.println(args[0]);
       System.out.println("I'm nomal");
       if (true)
        return;
      } catch (Exception ex) {
       System.out.println("I'm exception");
       if (true)
        return;
      } finally {
       System.out.println("I'm finally.");
      }
    
      System.out.println("Out of try.");
     }
    ｝
    A.I'm exception
    I'm finally.
    B.代码不能编译通过，因为最后一条语句位于return后，不可到达
    C.代码编译通过，但运行时输出异常信息
    D.I'm nomal
    I'm finally.
    答案：A
    
    
 #    26.关于以下代码，说法正确的是？
    class Example{
     public static void main(String[] args) throws IOException {
      if (args[0] == "hello") {
       throw new IOException();
      }
     }
    }
    A.代码编译成功
    B.代码编译失败，因为main()方法是入口方法，不能抛出异常
    C.代码编译失败，因为IOException异常是系统异常，不能由应用程序抛出
    D.代码编译失败，因为字符串应该用equals方法判定一致性
    答案：A
    
    
 #    27.关于以下代码，说法正确的是？
    class Example {
     public static void main(String[] args) throws IOException {
      System.out.println("Before Try");
      try {
      } catch (java.io.IOException e) {
       System.out.println("Inside Catch");
      }
      System.out.println("At the End");
     }
    }
    A.代码编译失败，因为无异常抛出
    B.代码编译失败，因为未导入IOException异常类
    C.输出Before Try
    At the End
    D.输出Inside Catch
    At the End
    答案：A
    
    
 #    28.关于以下代码，说法正确的是？
    class Example {
     public static void main(String[] args) throws IOException {
      System.out.println("Before Try");
      try {
      } catch (Throwable e) {
       System.out.println("Inside Catch");
      }
      System.out.println("At the End");
     }
    }
    A.代码编译失败，因为无异常抛出
    B.代码编译失败，因为未导入IOException异常类
    C.输出Before Try
    At the End
    D.输出Inside Catch
    At the End
    答案：C
    
    
  #   29.给出以下代码：
    class Example {
     public static void main(String[] args) throws IOException {
      try {
       methodA();   
      } catch (IOException e) {
       System.out.println("caught IOException");
      }catch (Exception e) {
       System.out.println("caught Exception");
      }
     }
    }
    如果methodA()方法抛出一个IOException异常，则该程序的运行结果是什么？
    A.无内容输出
    B.代码编译失败
    C.输出caught IOException
    D.输出caught Exception
    答案：C
    
    
 #    30.下列代码的运行结果是？
    class Example {
     public static void main(String[] args) throws IOException {
      try {
       return;
      } finally{
       
       System.out.println("Finally");
      }
     }
    }
    A.无内容输出
    B.输出Finally
    C.代码编译失败
    D.输出异常信息
    答案：B
    
    
  #   31.给出以下代码，执行结果是？
    class Example {
     public static void main(String[] args) throws IOException {
      aMethod();
     }
     
     static void aMethod(){
      try {
       System.out.println("Try");
       return;
      } catch (Exception e) {
       System.out.println("Catch");
      }finally{
       System.out.println("Finally");
      }
     }
    }
    A.代码编译成功，但运行期间抛出异常
    B.代码便以失败，因为return语句错误
    C.输出Try和Finally
    D.输出Try
    答案：C
    
    
 #    32.以下代码中，如果test()方法抛出一个NullPointException异常时，打印输出什么内容？
    class Example {
     public static void main(String[] args) throws IOException {
      try {
       test();
       System.out.println("Message1");
      } catch (ArrayIndexOutOfBoundsException e) {
       System.out.println("Message2");
      }finally{
       System.out.println("Message3");
      }
     }
    }
    A.打印输出Message1
    B.打印输出Message2
    C.打印输出Message3
    D.以上都不对
    答案：C
    
    
 #    33.以下代码执行结果是什么？
    class Example {
     public static String output = "";
     public static void foo(int i) {
      try {
       if (i == 1) {
        throw new Exception();
       }
       output += "1";
      } catch (Exception e) {
       output += "2";
       return;
      } finally {
       output += "3";
      }
      output += "4";
     }
    
     public static void main(String[] args) throws IOException {
      foo(0);
      foo(1);
      System.out.println(output);
     }
    }
    A.无内容输出
    B.代码编译失败
    C.输出13423
    D.输出14323
    答案：C
    
    
 #    34.以下代码执行结果是？
    public abstract class Example extends Base {
     public abstract void method();
    }
    
    class Base {
     public Base() throws IOException {
      throw new IOException();
     }
    }
    A.代码编译失败，因为非抽象类不能被扩展为抽象类
    B.代码编译失败，因为必须提供一个可以抛出或可以不抛出IOException异常的构造器
    C.代码编译失败，以in为必须提供一个可以抛出IOException异常或其子类的构造器
    D.代码编译成功
    答案：C
    
    
  #   35.关于以下代码正确的说法是：
    public class Example {
    int x = 0;
    
     public Example(int inVal) throws Exception {
    if (inVal != this.x) {
     throw new Exception("Invalid input");
    }
     }
    
    public static void main(String[] args) {
     Example t = new Example(4);
     }
    }
    A.代码在第1行编译错误
    B.代码在第4行编译错误
    C.代码在第6行编译错误
    D.代码在第11行编译错误
    答案：D
    
    
  #   36.关于try⋯catch⋯finally结构，描述正确的是些？
    A.可以有多个catch
    B.只能有一个catch
    C.可以没有catch
    D.finally必须有
    答案：A、C
    
    
 #    37.当fragile()方法抛出一个IllegalArgumentException异常时，下列代码的运行结果是什么？
    public static void main(String[] args) throws IOException {
      try {
       fragile();
      } catch (NullPointerException e) {
       System.out.println("NullPointerException thrown");
      } catch (Exception e) {
       System.out.println("Exception thrown");
      } finally {
       System.out.println("Done with exceptions");
      }
      System.out.println("myMethod is done");
     }
    ｝
    A.输出NullPointerException thrown
    B.输出Exception thrown
    C.输出Done with Exception
    D.输出myMethod is done
    答案：B、C、D
    
    
#     38.现有如下代码：
    public class Example { 
     public static void main(String[] args) {
      try {
       int x=Integer.parseInt("42a");
       //插入代码处
       System.out.println("oops");
      }
     }
    }
    在插入代码处插入哪些语句可以在运行后输出oops？
    A. } catch (IllegalArgumentException e) {
    B.} catch (IllegalStateException c) {
    C. } catch (NumbelFormatException n) {
    D.} catch (ClassCastException c) {
    答案：A、C
    
    
#     39.下列代码的执行结果是？
    class Example {
     public static void main(String[] args) throws IOException {
      int i = 1, j = 1;
      try {
       i++;
       j--;
       if (i == j) {
        j++;
       }
      } catch (ArithmeticException e) {
       System.out.println(0);
      } catch (ArrayIndexOutOfBoundsException e) {
       System.out.println(1);
      } catch (Exception e) {
       System.out.println(2);
      } finally {
       System.out.println(3);
      }
      System.out.println(4);
     }
    }
    A.输出1
    B.输出2
    C.输出3
    D.输出4
    答案：C、D
    
    
 #    40.下列代码的执行结果是？
    class Example {
     public static void main(String[] args) throws IOException {
      int i = 1, j = 1;
      try {
       i++;
       j--;
       if (i/j > 1) {
        j++;
       }
      } catch (ArithmeticException e) {
       System.out.println(0);
      } catch (ArrayIndexOutOfBoundsException e) {
       System.out.println(1);
      } catch (Exception e) {
       System.out.println(2);
      } finally {
       System.out.println(3);
      }
      System.out.println(4);
     }
    }
    A.输出0
    B.输出2
    C.输出3
    D.输出4
    答案：A、C、D
    
    
 #    41.现有如下代码：
    public class Example { 
     public static void main(String[] args) {
      try {
       System.out.println("before");
       doRisyThing();
       System.out.println("after");
      } catch (Exception e) {
       System.out.println("catch");
      }
      System.out.println("done");
     }
     
     public static void doRisyThing() throws Exception{
      //this code returns unless it throws an Exception
     }
    }
    该代码可能的执行结果有哪些？
    A.before catch
    B.before after done
    C.before catch done
    D.before after catch
    答案：B、C
    
 #    42.以下有关java.lang.Exception异常类的正确描述有？
    A.该类是一个公共类
    B.该类是Throwable类的子类
    C.该类实现了Throwable接口
    D.该类可以序列化
    答案：A、B、D
    
#     43.给出以下代码：
    1. public void aMethod(){  
    2.  
    3.  if(Condition){
    4.   
    5.  }
    6.  
    7. }
    当if条件表达式为true时，插入哪些语句可以抛出MyException异常？
    A.在第4行插入throws new MyException();
    B.在第4行插入throw new MyException();
    C.在第6行插入throw new MyException();
    D.在第1行插入throws MyException
    答案：B、D
    
 #    44.以下哪些是catch语句能够捕获处理的异常？
    A.Throwable
    B.Error
    C.Exception
    D.String
    答案：A、B、C
    Error也是可以被catch捕获的
    
 #    45.以下哪些描述是正确的？
    A.try语句块后必须至少存在一个catch语句块
    B.try语句块后可以存在不限数量的finally语句块
    C.try语句块后必须至少存在一个catch语句块或finally语句块
    D.如果catch和finally语句块同时存在，则catch语句块必须位于finally语句块前
    答案：C、D
    
 #    46.下列代码的执行结果是？
    class Example {
    
     private void method1() throws Exception {
      throw new RuntimeException();
     }
    
     public void method2() {
      try {
       method1();
      } catch (RuntimeException e) {
       System.out.println("Caught Runtime Exception");
      } catch (Exception e) {
       System.out.println("Caught Exception");
      }
     }
    
     public static void main(String[] args) throws IOException {
      Example a = new Example();
      a.method2();
     }
    }
    A.代码编译失败
    B.输出Caught Runtime Exception
    C.输出Caught Exception
    D.输出Caught Runtime Exception和Caught Exception
    答案：B
    
    
 #    47.以下代码的输出结果是什么？选择所有的正确答案。
    class Example {
     public static void main(String[] args) throws IOException {
      for (int i = 0; i < 10; i++) {
    
       try {
        try {
         if (i % 3 == 0)
          throw new Exception("E0");
         System.out.println(i);
        } catch (Exception inner) {
         i *= 2;
         if (i % 3 == 0)
          throw new Exception("E1");
        } finally {
         ++i;
        }
       } catch (Exception outer) {
        i += 3;
       } finally {
        --i;
       }
      }
     }
    }
    A.4.0
    B.5.0
    C.6.0
    D.7.0
    答案：A、B
    
 #    48.Java中异常的分类
    答案：
    java.lang.Throwable
    |-- Error错误：JVM内部的严重问题。无法恢复。程序人员不用处理。
    |--Exception异常：普通的问题。通过合理的处理，程序还可以回到正常执行流程。要求编程人员要进行处理。
    |--RuntimeException:也叫非受检异常(unchecked exception).这类异常是编程人员的逻辑问题。应该承担责任。Java编译器不进行强制要求处理。 也就是说，这类异常再程序中，可以进行处理，也可以不处理。
    |--非RuntimeException:也叫受检异常(checked exception).这类异常是由一些外部的偶然因素所引起的。Java编译器强制要求处理。也就是说，程序必须进行对这类异常进行处理。
    
 #    49.给出常见的RuntimeException
    答案：
    常见的运行时异常有如下这些ArithmeticException, ArrayStoreException, BufferOverflowException, BufferUnderflowException, CannotRedoException, CannotUndoException, ClassCastException, CMMException, ConcurrentModificationException, DOMException, EmptyStackException, IllegalArgumentException, IllegalMonitorStateException, IllegalPathStateException, IllegalStateException, ImagingOpException, IndexOutOfBoundsException, MissingResourceException, NegativeArraySizeException, NoSuchElementException, NullPointerException, ProfileDataException, ProviderException, RasterFormatException, SecurityException, SystemException, UndeclaredThrowableException, UnmodifiableSetException, UnsupportedOperationException
    
#     50.error和exception有什么区别
    答案：
    error 表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出。不可能指望程序能处理这样的情况
    exception 表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况
    
 #    51.以下代码的执行结果是？
    public static int fun() {
      int result = 5;
      try {
       result = result / 0;
       return result;
      } catch (Exception e) {
       System.out.println("Exception");
       result = -1;
       return result;
      } finally {
       result = 10;
       System.out.println("i am in finally");
      }
     }
    
     public static void main(String[] args) {
      int x=fun();
      System.out.println(x);
    
     }
    
    答案：
    Exception
    I am in finally
    -1
    
#     52.以下代码的执行结果是？
    public class Example {
     public static StringBuffer fun() {
    
      StringBuffer result = new StringBuffer("Hello");
      Integer i = new Integer(5);
      try {
       if (true)
        throw new RuntimeException();
       return result;
      } catch (Exception e) {
       System.out.println("Exception");
       result.append(" World");
       return result;
      } finally {
       result.append(" Java");
       System.out.println("i am in finally");
      }
     }
    
     public static void main(String[] args) {
      StringBuffer x = fun();
      System.out.println(x);
    
     }
    
    答案：
    Exception
    i am in finally
    Hello World Java
    
    
#     53.什么时候用assert?
    答案：
    断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true。如果表达式计算为 false，那么系统会报告一个 Assertionerror。它用于调试目的：
    
    assert(a > 0); // throws anAssertionerror if a <= 0
    
    断言可以有两种形式：
    
    assert Expression1 ;
    
    assert Expression1 :Expression2 ;
    
      Expression1 应该总是产生一个布尔值。
    
      Expression2 可以是得出一个值的任意表达式。这个值用于生成显示更多调试信息的 String 消息。
    
      断言在默认情况下是禁用的。要在编译时启用断言，需要使用 source 1.4 标记：
    
      javac -source 1.4 Test.java
    
      要在运行时启用断言，可使用 -enableassertions 或者 -ea 标记。
    
      要在运行时选择禁用断言，可使用 -da 或者 -disableassertions 标记。
    
      要系统类中启用断言，可使用 -esa 或者 -dsa 标记。还可以在包的基础上启用或者禁用断言。
    
     可以在预计正常情况下不会到达的任何位置上放置断言。断言可以用于验证传递给私有方法的参数。不过，断言不应该用于验证传递给公有方法的参数，因为不管是否启用了断言，公有方法都必须检查其参数。不过，既可以在公有方法中，也可以在非公有方法中利用断言测试后置条件。另外，断言不应该以任何方式改变程序的状态。
    
    
 #    54.给我一个你最常见到的runtime exception
    答案：
    常见的运行时异常有如下这些ArithmeticException, ArrayStoreException, BufferOverflowException, BufferUnderflowException, CannotRedoException, CannotUndoException, ClassCastException, CMMException, ConcurrentModificationException, DOMException, EmptyStackException, IllegalArgumentException, IllegalMonitorStateException, IllegalPathStateException, IllegalStateException, ImagingOpException, IndexOutOfBoundsException, MissingResourceException, NegativeArraySizeException, NoSuchElementException, NullPointerException, ProfileDataException, ProviderException, RasterFormatException, SecurityException, SystemException, UndeclaredThrowableException, UnmodifiableSetException, UnsupportedOperationException
    
    
#     55.谈谈final, finally, finalize的区别
    答案：
    final—修饰符（关键字）如果一个类被声明为final，意味着它不能再派生出新的子类，不能作为父类被继承。因此一个类不能既被声明为 abstract的，又被声明为final的。将变量或方法声明为final，可以保证它们在使用中不被改变。被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取，不可修改。被声明为final的方法也同样只能使用，不能重载
    finally—再异常处理时提供 finally 块来执行任何清除操作。如果抛出一个异常，那么相匹配的 catch 子句就会执行，然后控制就会进入 finally 块（如果有的话）
    finalize—方法名。Java 技术允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。它是在 Object 类中定义的，因此所有的类都继承了它。子类覆盖 finalize() 方法以整理系统资源或者执行其他清理工作。finalize() 方法是在垃圾收集器删除对象之前对这个对象调用的
    
 #    56.下列说法错误的是？
    A.Object类是所有Java类的顶层类，即类继承树的根。
    B.如果一个类没有使用extends关键字扩展任何类，则编译器自动将创建的类视为Object类的子类
    C.Object类中提供了equals()方法来判定本对象和其他对象中的内容是否一致
    D.Object中提供的clone默认为浅克隆
    答案：C
    equals方法默认和==一致
    
 #    57.定义在Object类上的hashCode()方法的返回值类型是什么？
    A.char
    B.long
    C.int
    D.float
    答案：C
    
 #    58.关于集合中对象的equals()和hashCode()规定说法错误的是？
    A.如果两个对象相同，那么他们的hashCode值需要一致
    B.如果两个对象的hashCode值一致，他们的equals方法不一定返回true
    C.equals方法默认和==判定一致
    D.Java中hashCode就是对象的内存地址
    答案：D
    Java中hashCode不是内存地址，但是可以一定程度上代表地址特诊
    
 #    59.以下代码执行结果是什么？
    class Person {
     static void sayHello() {
      System.out.println("HelloWorld!");
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      ((Person) null).sayHello();
     }
    }
    A.编译失败
    B.编译成功，运行时产生NullPointerException
    C.输出HelloWorld!
    D.输出空白字符串
    答案：C
    null能够被造型撑任何类型，而sayHello方法是静态方法，不依赖实例调用
    
 #    60.下列代码执行结果是：
    class RectObject {
     public int x;
     public int y;
    
     public RectObject(int x, int y) {
      this.x = x;
      this.y = y;
     }
    
     @Override
     public int hashCode() {
      final int prime = 31;
      int result = 1;
      result = prime * result + x;
      result = prime * result + y;
      return result;
     }
    
     @Override
     public boolean equals(Object obj) {
      if (this == obj)
       return true;
      if (obj == null)
       return false;
      if (getClass() != obj.getClass())
       return false;
      final RectObject other = (RectObject) obj;
      if (x != other.x) {
       return false;
      }
      if (y != other.y) {
       return false;
      }
      return true;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      HashSet<RectObject> set = new HashSet<RectObject>();
      RectObject r1 = new RectObject(3, 3);
      RectObject r2 = new RectObject(5, 5);
      RectObject r3 = new RectObject(3, 3);
      set.add(r1);
      set.add(r2);
      set.add(r3);
      set.add(r1);
      System.out.println("size:" + set.size());
     }
    }
    A.size:1
    B.size:2
    C.size:3
    D.size:4
    答案：B
    因为我们重写了RectObject类的hashCode方法，只要RectObject对象的x,y属性值相等那么他的hashCode值也是相等的，所以先比较hashCode的值，r1和r2对象的x,y属性值不等，所以他们的hashCode不相同的，所以r2对象可以放进去，但是r3对象的x,y属性值和r1对象的属性值相同的，所以hashCode是相等的，这时候在比较r1和r3的equals方法，因为他么两的x,y值是相等的，所以r1,r3对象是相等的，所以r3不能放进去了，同样最后再添加一个r1也是没有没有添加进去的，所以set集合中只有一个r1和r2这两个对象
    
#     61.以下代码的执行结果是？
    class RectObject {
     public int x;
     public int y;
    
     public RectObject(int x, int y) {
      this.x = x;
      this.y = y;
     }
    
    
     @Override
     public boolean equals(Object obj) {
      if (this == obj)
       return true;
      if (obj == null)
       return false;
      if (getClass() != obj.getClass())
       return false;
      final RectObject other = (RectObject) obj;
      if (x != other.x) {
       return false;
      }
      if (y != other.y) {
       return false;
      }
      return true;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      HashSet<RectObject> set = new HashSet<RectObject>();
      RectObject r1 = new RectObject(3, 3);
      RectObject r2 = new RectObject(5, 5);
      RectObject r3 = new RectObject(3, 3);
      set.add(r1);
      set.add(r2);
      set.add(r3);
      set.add(r1);
      System.out.println("size:" + set.size());
     }
    }
    A.size:1
    B.size:2
    C.size:3
    D.size:4
    答案：C
    首先判断r1对象和r2对象的hashCode，因为Object中的hashCode方法返回的是对象本地内存地址的换算结果，不同的实例对象的hashCode是不相同的，同样因为r3和r1的hashCode也是不相等的，但是r1==r1的，所以最后set集合中只有r1,r2,r3这三个对象，所以大小是3
    
 #    62.以下代码执行结果是？
    class RectObject {
     public int x;
     public int y;
    
     public RectObject(int x, int y) {
      this.x = x;
      this.y = y;
     }
    
    
     @Override
     public boolean equals(Object obj) {
      return false;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      HashSet<RectObject> set = new HashSet<RectObject>();
      RectObject r1 = new RectObject(3, 3);
      RectObject r2 = new RectObject(5, 5);
      RectObject r3 = new RectObject(3, 3);
      set.add(r1);
      set.add(r2);
      set.add(r3);
      set.add(r1);
      System.out.println("size:" + set.size());
     }
    }
    A.size:1
    B.size:2
    C.size:3
    D.size:4
    答案：C
    首先是判断hashCode是否相等，不相等的话，直接跳过，相等的话，然后再来比较这两个对象是否相等或者这两个对象的equals方法，因为是进行的或操作，所以只要有一个成立即可，那这里我们就可以解释了，其实上面的那个集合的大小是3,因为最后的一个r1没有放进去，以为r1==r1返回true的，所以没有放进去了。所以集合的大小是3，如果我们将hashCode方法设置成始终返回false的话，这个集合就是4了。
    
 #    63.以下代码执行结果是？
    class RectObject {
     public int x;
     public int y;
    
     public RectObject(int x, int y) {
      this.x = x;
      this.y = y;
     }
    
     @Override
     public int hashCode() {
      // TODO Auto-generated method stub
      return (int)System.nanoTime();
     }
    
     @Override
     public boolean equals(Object obj) {
      return false;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      HashSet<RectObject> set = new HashSet<RectObject>();
      RectObject r1 = new RectObject(3, 3);
      RectObject r2 = new RectObject(5, 5);
      RectObject r3 = new RectObject(3, 3);
      set.add(r1);
      set.add(r2);
      set.add(r3);
      set.add(r1);
      System.out.println("size:" + set.size());
     }
    }
    A.size:1
    B.size:2
    C.size:3
    D.size:4
    答案：D
    见上题
    
 #    64.下列关于Math类说法错误的是
    A.java.lang.Math类是final类，因此不能被其他类继承
    B.java.lang.Math类的构造器是私有的，即声明为private，不能实例化一个Math类的对象
    C.java.lang.Math类上定义的所有常量和方法均是public和static的，因此可以直接通过类名调用
    D.min()和max()方法的参数之一，如果是NaN值，则方法将返回另一个参数值
    答案：D
    min()和max()方法的参数之一，如果是NaN值，则方法的返回值就为NaN
    
 #    65.以下哪个方法是Math类中定义的？
    A.absolute()
    B.log()
    C.cosine()
    D.sine()
    答案：B
    在Math类中对应的正确方法应为abs()\cos()\sin()
    
 #    66.定义在Math类上的round(double d)方法的返回值类型是什么？
    A.char
    B.int
    C.long
    D.double
    答案：C
    round方法用于获取一个四舍五入的整数
    
 #    67.以下哪个方法用于计算平方根？
    A.squareRoot()
    B.sqrt()
    C.root()
    D.sqr()
    答案：B
    
 #    68.调用Math.random()方法最有可能输出以下哪些结果？
    A.-0.12和0.56E3
    B.0.12和1.1E1
    C.-23.45和0.0
    D.0.356和0.03
    答案：D
    random()方法返回值的取值范围在0.0..1.0之间
    
 #    69.以下代码的输出结果是什么？
    public class Example {
     public static void main(String[] args) {
      System.out.println(Math.round(Float.MAX_VALUE));
     }
    }
    A.输出Integer.MAX_VALUE
    B.输出一个最接近Float.MAX_VALUE的整数
    C.编译失败
    D.运行时输出异常信息
    答案：A
    Math.round(Float.MAX_VALUE)的返回值为Integer.MAX_VALUE，Math.round(Double.MAX_VALUE)的返回值为Long.MAX_VALUE（真实计算结果超过返回值范围）
    
 #    70.以下代码的运行结果是什么？
    public class Example {
     public static void main(String[] args) {
      System.out.println(Math.min(0.0, -0.0));
     }
    }
    A.代码编译失败
    B.输出0.0
    C.输出-0.0
    D.代码编译成功，但运行时输出异常信息
    答案：C
    浮点数的取值范围内存在正负0.0
    
 #    71.以下代码的执行结果是？
    public class Example {
     public static void main(String[] args) {
      System.out.println(Math.min(0.0, -0.0));
     }
    }
    A.输出4
    B.输出5
    C.输出6 
    D.输出9
    答案：D
    比2.3大的最接近整数是3，因此ceil(2.3f)=3.0，因为2.7的四舍五入的值为3.0，所以round(2.7)=3.0，最终打印输出等于9
    
 #    72.以下代码的运行结果是什么？
    public class Example {
     public static void main(String[] args) {
      double d1 = -0.5;
      System.out.println("Ceil d1=" + Math.ceil(d1));
      System.out.println("Floor d1=" + Math.floor(d1));
     }
    }
    
    A.输出Ceil d1=-0.0 Floor d1=-1.0
    B.输出Ceil d1=0.0 Floor d1=-1.0
    C.输出Ceil d1=-0.0 Floor d1=-0.0
    D.输出Ceil d1=0.0 Floor d1=0.0
    答案：A
    
    
 #    73.给出以下代码，为了结果输出-12.0，方法method(d)应为以下哪个方法？
    public class Example {
     public static void main(String[] args) {
      double d = -11.1;
      double d1 = method(d);
      System.out.println(d1);
     }
    }
    A.floor()
    B.ceil()
    C.round()
    D.abs()
    答案：A
    
    
 #    74.给出以下代码，请问在程序的第6行插入那条语句，改程序可依次打印输出11、10、9？
    1.public class Example {
    2. public static void main(String[] args) {
    3.  double x[] = { 10.2, 9.1, 8.7 };
    4.  int i[] = new int[3];
    5.  for (int a = 0; a < x.length; a++) {
    6.
    7.   System.out.println(i[a]);
    8.  }
    9. }
    10.}
    A.i[1] = ((int)Math.min(x[a]));
    B.i[1] = ((int)Math.max(x[a]));
    C.i[1] = ((int)Math.ceil(x[a]));
    D.i[1] = ((int)Math.floor(x[a]));
    答案：C
    
    
#     75.以下代码执行结果是？
    public class Example {
     public static void main(String[] args) {
      System.out.println(Math.min(Float.NaN, Float.POSITIVE_INFINITY));
     }
    }
    A.输出NaN
    B.打印输出Infinity
    C.运行时异常，因为NaN不是有效的参数
    D.运行时异常，因为Infinity不是有效的参数
    答案：A
    min()和max()方法的参数之一，如果是NaN值，则方法的返回值就为NaN
    
 #    76.以下代码的执行结果是？
    public class Example{
      public static void main(String s[]){
       String str=”123”;
    String str_=new String(“123”);
    String  _str=”123”;
       System.out.println(str==_str);
    System.out.println(str==str_);
    } 
    }
    
    A.输出true true
    B.输出false false
    C.输出true false
    D.输出false true
    答案：C
    字符串创建的时候可以使用常量池
    
 #    77.public class Example {
     public static void main(String[] args) {
      Integer i = 100;
      Integer j = 100;
      System.out.println(i == j);
      i = 300;
      j = 300;
      System.out.println(i == j);
     }
    }
    A.输出true true
    B.输出false false
    C.输出true false
    D.输出false true
    答案：C
    128以内的数进行自动包装时使用池操作
    
 #    78.以下哪个不是基本类型的包装类？
    A.Char
    B.Integer
    C.Boolean
    D.float
    答案：A
    
#     79.以下说法正确的是？
    A.Void类是Class类的子类
    B.Float类是Double类的子类
    C.Double类是Wrapper类的子类
    D.Integer类是Number类的子类
    答案：D
    
  #   80.定义在Integer类上的哪些方法用于将一个Integer对象转换为一个基本数据int类型？
    A.valueOf（）
    B.intValue（）
    C.getInt（）
    D.getInteger（）
    答案：B
    
 #    81.一下代码的执行结果是什么？
    public class Example {
     public static void main(String[] args) {
      String val = null;
      int x = Integer.parseInt(val);
      System.out.println(x);
     }
    }
    A.输出0
    B.输出null
    C.输出NumberFormatException异常
    D.无内容输出
    答案：C
    
 #    82.由于Java中存在字符串对象池，因此采用下面方法创建的两个字符串变量，他们指向的是同一个字符串对象：String str1="asddsg";String str2="asddsg"
    A.调用字符串上定义的改变字符串内容的方法，返回值都是一个新字符串，而原有字符串内容不变
    B.调用replace（char oldChar,char newChar）方法时，当参数oldChar和newChar一致时，返回一个和源对象内容一致的新字符串
    C.String的equals方法用于判定两个字符串内容是否一致
    D.调用toUpperCase()和toLowerCase()方法，当为进行大小写转换时，返回源字符串对象
    答案：B
    调用replace（char oldChar,char newChar）方法时，当参数oldChar和newChar一致时，返回源字符串对象
    
 #    83.以下说法错误的是？
    A.String中的append方法用于在源字符串后追加内容
    B.StringBuffer中的append方法用于在源字符串后追加内容
    C.StringBuffer是一个缓冲区，器内容可变
    D.String中的concat方法用于字符串串联
    答案：A
    String中没有append方法
    
 #    84.以下哪些有关通过子类来扩展String类功能的描述是正确的？
    A.无法子类化，因为String类是一个final类
    B.可以子类化，通过覆盖String类中的方法实现功能扩展
    C.无法子类化，因为String类是一个抽象类
    D.可以子类化，但是只能覆盖Object类中声明的方法，因为String类中定义的其他方法否是final的
    答案：A
    
#     85.嗲用以下哪个方法会导致字符串被改变？
    A.concat()
    B.toUpperCase()
    C.replace()
    D.没有改变字符串的方法可以调用
    答案：D
    
#     86.如何获取一个String类实例S包含的字符个数？
    A.s.size
    B.s.length
    C.s.size()
    D.s.length()
    答案：D
    
 #    87.以下代码执行结果是？
    public class Example {
     public static void main(String[] args) {
      System.out.println("string".endsWith(""));
     }
    }
    A.输出true
    B.输出false
    C.编译失败
    D.运行时输出异常信息
    答案：A
    
#     88.有String s = "Metallica";请问以下哪个语句可以打印输出ica？
    A.System.out.println(s.substring(7));
    B.System.out.println(s.substring(6));
    C.System.out.println(s.substring(6，8));
    D.System.out.println(s.substring(7，9));
    答案：B
    
 #    89.以下那些关于String类的描述是正确的？
    A.该类是一个final类
    B.该类是一个public类
    C.该类可以序列化
    D.该类有一个一StringBuffer实例作为参数的构造器
    答案：A、B、C、D
    
 #    90.以下哪些是String类中定义的方法？
    A.length（）
    B.toUpper()
    C.toString()
    D.equals()
    答案：A、C、D
    
 #    91.以下哪些关于封装类的描述是正确的？
    A.封装类都是public类
    B.封装类均可序列化
    C.封装类均是final类
    D.封装类都是java.lang.Number类的子类
    答案：A、B、C
    
 #    92.请问以下哪些方法是定义在Object类上的，请选择所有正确答案
    A.toString()
    B.equals(Object o)
    C.println()
    D.wait()
    答案：A、B、D
    
 #    93.请问以下哪些描述是正确的？请选择所有正确答案
    A.Class类是Object类的超类
    B.Object类是一个final类
    C.Class类可用于装载其他类
    D.ClassLoader类可用于装载其他类
    答案：C、D
    
  #   94.给出以下代码，请问以下哪些定义在Math类上的方法可以使表达式结果为true?
    Method(-4.4) == -4
    A.round()
    B.trunc()
    C.floor()
    D.ceil()
    答案：A、D
    
 #    95.下面这条语句一共创建了多少个对象：String s="a"+"b"+"c"+"d";
    一个字符串对象和一个指向这个对象的引用
    对于如下代码：
    
    Strings1 = "a";
    
    Strings2 = s1 + "b";
    
    Strings3 = "a" + "b";
    
    System.out.println(s2== "ab");
    
    System.out.println(s3== "ab");
    答案：
    第一条语句打印的结果为false，第二条语句打印的结果为true，这说明javac编译可以对字符串常量直接相加的表达式进行优化，不必要等到运行期去进行加法运算处理，而是在编译时去掉其中的加号，直接将其编译成一个这些常量相连的结果。
    
    题目中的第一行代码被编译器在编译时优化后，相当于直接定义了一个”abcd”的字符串，所以，上面的代码应该只创建了一个String对象和一个指向该对象的饮用。写如下两行代码，
    
                   String s = "a" +"b" + "c" + "d";
    
                   System.out.println(s =="abcd");  最终打印的结果应该为true。
    
 #    96.以下代码的执行结果是？
    class ShadowClone implements Cloneable {
    
     private int a;
     private int[] b;
    
     @Override
     public Object clone() {
      ShadowClone sc = null;
      try {
       sc = (ShadowClone) super.clone();
      } catch (CloneNotSupportedException e) {
       e.printStackTrace();
      }
      return sc;
     }
    
     public int getA() {
      return a;
     }
    
     public void setA(int a) {
      this.a = a;
     }
    
     public int[] getB() {
      return b;
     }
    
     public void setB(int[] b) {
      this.b = b;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      ShadowClone c1 = new ShadowClone();
      c1.setA(100);
      c1.setB(new int[] { 1000 });
      System.out.println("克隆前c1:  a=" + c1.getA() + " b[0]=" + c1.getB()[0]);
      ShadowClone c2 = (ShadowClone) c1.clone();
      c2.setA(50);
      int[] a = c2.getB();
      a[0] = 5;
      c2.setB(a);
      System.out.println("克隆后c1:  a=" + c1.getA() + " b[0]=" + c1.getB()[0]);
      System.out.println("克隆后c2:  a=" + c2.getA() + " b[0]=" + c2.getB()[0]);
     }
    }
    
    答案：
    克隆前c1:  a=100 b[0]=1000
    克隆后c1:  a=100 b[0]=5
    克隆后c2:  a=50 b[0]=5
    
    Java中Object类提供的克隆方法默认为浅克隆，因此克隆后的引用属性和原始对象中的引用属性引用了同一对象，对克隆对象中引用数据的变更就直接反映到原始对象中
    
 #    97.以下代码的执行结果是？
    class DeepClone implements Cloneable {
    
     private int a;
     private int[] b;
    
     @Override
     public Object clone() {
      DeepClone sc = null;
      try {
       sc = (DeepClone) super.clone();
       int[] t = sc.getB();
       int[] b1 = new int[t.length];
       for (int i = 0; i < b1.length; i++) {
        b1[i] = t[i];
       }
       sc.setB(b1);
      } catch (CloneNotSupportedException e) {
       e.printStackTrace();
      }
      return sc;
     }
    
     public int getA() {
      return a;
     }
    
     public void setA(int a) {
      this.a = a;
     }
    
     public int[] getB() {
      return b;
     }
    
     public void setB(int[] b) {
      this.b = b;
     }
    }
    
    public class Example {
     public static void main(String[] args) {
      DeepClone c1 = new DeepClone();
      c1.setA(100);
      c1.setB(new int[] { 1000 });
      System.out.println("克隆前c1:  a=" + c1.getA() + " b[0]=" + c1.getB()[0]);
      DeepClone c2 = (DeepClone) c1.clone();
      c2.setA(50);
      int[] a = c2.getB();
      a[0] = 5;
      c2.setB(a);
      System.out.println("克隆后c1:  a=" + c1.getA() + " b[0]=" + c1.getB()[0]);
      System.out.println("克隆后c2:  a=" + c2.getA() + " b[0]=" + c2.getB()[0]);
     }
    }
    
    答案：
    克隆前c1:  a=100 b[0]=1000
    克隆后c1:  a=100 b[0]=1000
    克隆后c2:  a=50 b[0]=5
    
    
    自定义的深度克隆
    
 #    98.两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对
    答案：
    
    不对，有相同的hash code
    
    
 #    99.覆盖equals()方法时需要注意的设计原则有哪些？
    答案：
    
    对称性：如果x.equals(y)返回是“true”，那么y.equals(x)也应该返回是“true”。
    反射性：x.equals(x)必须返回是“true”。
    类推性：如果x.equals(y)返回是“true”，而且y.equals(z)返回是“true”，那么z.equals(x)也应该返回是“true”。
    一致性：如果x.equals(y)返回是“true”，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是“true”。
    任何情况下，x.equals(null)，永远返回是“false”；x.equals(和x不同类型的对象)永远返回是“false”。 
    
 #    100.equals方法和==的区别
    答案：
    1.基本数据类型，也称原始数据类型。byte,short,char,int,long,float,double,boolean
      他们之间的比较，应用双等号（==）,比较的是他们的值。
    2.复合数据类型(类)
      当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址，所以，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。 JAVA当中所有的类都是继承于Object这个基类的，在Object中的基类中定义了一个equals的方法，这个方法的初始行为是比较对象的内存地 址，但在一些类库当中这个方法被覆盖掉了，如String,Integer,Date在这些类当中equals有其自身的实现，而不再是比较类在堆内存中的存放地址了。
      对于复合数据类型之间进行equals比较，在没有覆写equals方法的情况下，他们之间的比较还是基于他们在内存中的存放位置的地址值的，因为Object的equals方法也是用双等号（==）进行比较的，所以比较后的结果跟双等号（==）的结果相同。 
    
 #    101.String、StringBuffer、StringBuilder有什么区别？
    答案：
    String类表示内容不可改变的字符串。
    而StringBuffer类表示内容可以被修改的字符串。当你知道字符数据要改变的时候你就可以使用StringBuffer。典型地，你可以使用StringBuffers来动态构造字符数据。
    另外，String实现了equals方法，new String(“abc”).equals(new String(“abc”)的结果为true,
    而StringBuffer没有实现equals方法，所以，new StringBuffer(“abc”).equals(new StringBuffer(“abc”)的结果为false。
    StringBuffer和StringBuilder类都表示内容可以被修改的字符串，StringBuilder是线程不安全的，运行效率高，如果一个字符串变量是在方法里面定义，这种情况只可能有一个线程访问它，不存在不安全的因素了，则用StringBuilder。
    如果要在类里面定义成员变量，并且这个类的实例对象会在多线程环境下使用，那么最好用StringBuffer。
    
    

