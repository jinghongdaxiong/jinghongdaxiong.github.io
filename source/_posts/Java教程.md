# Java教程

## Java快速入门
## 面向对象编程
## 异常处理
## 反射
## 注解
## 泛型
## 集合
## IO
## 日期与时间
## 单元测试
## 正则表达式
## 加密与安全
## 多线程
## Maven基础
## 网络编程
## XML与JSON
## JDBC编程
## 函数式编程

函数式编程最早是数学家阿隆佐.邱奇研究的一套函数变换逻辑，又称Lambda Calculus(λ-Calculus),，所以也经常把函数式编程称为Lambda计算。

**特点： 允许把函数本省作为参数传入另一个函数，还允许返回一个函数**


### Lambda基础

#### Lambda表达式
在Java程序中，我们经常遇到一大堆单方法接口，即一个接口只定义了一个方法：
* Comparator
* Runnable
* Callable

以`Comparator`为例，我们想要调用`Arrays.sort()`时，可以传入一个`Comparator`实例，以匿名类方式编写如下：

    String[] array = ...
    Arrays.sort(array, new Comparator<String>() {
        public int compare(String s1, String s2) {
            return s1.compareTo(s2);        
        }
    }
    
上述方法非常繁琐。从Java8开始，我们可以用Lambda表达式替换单方法接口。改写上述代码如下：

    // Lambda
    public class Main(String[] args) {
         public static void main(String[] args) {
                String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
                Arrays.sort(array, (s1, s2) -> {
                    return s1.compareTo(s2);
                });
                System.out.println(String.join(", ", array));
            }
    }    
    
观察Lambda表达式的写法，它只需要写出方法定义：
    
     (s1,s2) -> {
        return s1.compareTo（s2）;
     }   
其中，参数是`(s1,s2)`,参数类型可以省略，因为编译器可以自动推断出`String`类型。` -> {... }`表示方法体，所有代码写在内部即可。
Lambda表达式没有`class`定义，因此写法非常简洁。如果只有一行`return xxx`的代码，完全可以用更简单的写法：

    Arrays.sort(array,(s1, s2) -> s1.compareTo(s2));
    
返回值得类型也是由编译器自动推断的，这里推断出的返回值是`int`,因此，只要返回`int`,编译器就不会报错。

#### FunctionalInterface
我们把只定义了单方法的接口称之为`FunctionalInterface`,用注解`@FunctionalInterface`标记。例如，`Callable`接口：

    @FunctionalInterface
    public interface Callable<V> {
        V call() throws Exception;
    }
    
再来看`Comparator`接口：
    
    @FuctionalInterface
    public interface Comparator<T> {
        
        int compare(T o1, T o2);
        
        boolean equals(Object obj);
        
        default Comparator<T> reversed() {
            return Collections.reverseOrder(this);
        }
        
        default Comparator<T> thenComparing(Comparator<? super T> other) {
            ...
        }
        ...
    }    
    
虽然`Comparator`接口有很多方法，但只有一个抽象方法`int compare(T o1, T o2)`，其他的方法都是`default`方法或`static`方法。
另外注意到`boolean equals(Object obj)`是`Object`定义的方法，不算在接口方法内。因此，`Comparator`也是一个`FunctionalInterface`。        


### 方法引用
使用Lambda表达式，我们就可以不必编写`FunctionalInterface`接口的实现类，从而简化代码：
    
    Arrays.sort(array, (s1, s2) -> {
        return s1.compareTo(s2);
    });
    
实际上，除了Lambda表达式，我们还可以直接传入方法引用。例如：

    public class Main {
        public static void main(String[] args) {
            String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
            Arrays.sort(array, Main::cmp);
            System.out.println(String.join(", ", array));
        }
    
        static int cmp(String s1, String s2) {
            return s1.compareTo(s2);
        }
    }

上述代码在`Arrays.sort()`中直接传入了静态方法`cmp`的引用，用`Main::cmp`表示。

因此，所谓方法引用，是指如果某个方法签名和接口恰好一致，就可以直接传入方法引用。

因为`Comparator<String>`接口定义的方法是`int compare(String, String)`，和静态方法`int cmp(String, String)`相比，除了方法名外，
方法参数一致，返回类型相同，因此，我们说两者的方法签名一致，可以直接把方法名作为Lambda表达式传入：
    
    Arrays.sort(array,Main::cmp);
    
注意：在这里，方法签名只看参数类型和返回类型，不看方法名称，也不看类的继承关系。

我们再看看如何引用实例方法。如果我们把代码改写如下：

    public class Main {
        public static void main(String[] args) {
            String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
            Arrays.sort(array, String::compareTo);
            System.out.println(String.join(", ", array));
        }
    }
    
不但可以编译通过，而且运行结果也是一样的，这说明String.compareTo()方法也符合Lambda定义。

观察`String.compareTo()`的方法定义：
    
    public final class String {
        public int compareTo(String o) {
            ...
        }
    }
    
这个方法的签名只有一个参数，为什么和`int Comparator<String>.compare(String, String)`能匹配呢？
因为实例方法有一个隐含的`this`参数，`String`类的`compareTo()`方法在实际调用的时候，第一个隐含参数总是传入`this`，相当于静态方法：

    public static int compareTo(this, String o);
    
所以，`String.compareTo()`方法也可以作为方法引用传入。

#### 构造方法引用
除了可以引用静态方法和实例方法，我们还可以引用构造方法。

我们来看一个例子：如果要把一个 `List<String>` 转换为`List<Person>`,应该怎么办？

    class Person {
        String name;
        public Person(String name) {
            this.name = name;
        }
    }    
    
    List<String> names = List.of("Bob", "Alice", "Tim");    
    List<Person> persons = ???
    
传统的做法是先定义一个`ArrayList<Person>`,然后用`for`循环填充这个`List`:

    List<String> names = List.of("Bob", "Alice", "Tim");
    List<Person> persons = new ArrayList<>();
    for (String name : names) {
        persons.add(new Person(name));
    }
    
要更简单地实现String到Person的转换，我们可以引用Person的构造方法：

    public class Main {
        public static void main(String[] args) {
            List<String> names = List.of("Bob", "Alice", "Tim");
            List<Person> persons = names.stream().map(Person::new).collect(Collectors.toList());
            System.out.println(persons);
        }
    }
   
    class Person {
        String name;
        public Person(String name) {
            this.name = name;
        }
        public String toString() {
            return "Person:" + this.name;
        }
    }

后面我们会讲到Stream的map()方法。现在我们看到，这里的map()需要传入的FunctionalInterface的定义是：

    @FunctionalInterface
    public interface Function<T, R> {
        R apply(T t);
    }
    
把泛型对应上就是方法签名`Person apply(String)`,即传入参数`String`,返回类型`Person`。    
而`Person`类的构造方法恰好满足这个条件，因为构造方法的参数是`String`，而构造方法虽然没有`return`语句，
但它会隐式地返回`this`实例，类型就是`Person`，因此，此处可以引用构造方法。构造方法的引用写法是`类名::new`，因此，此处传入`Person::new`。

#### 小结
`FunctionalInterface`允许传入：
* 接口的实现类（传统写法，代码较繁琐）；
* Lambda表达式（只需要列出参数名，由编译器推断类型）；
* 符合方法签名的静态方法；
* 符合方法签名的实例方法（实例类型被看做第一个参数类型）；
* 符合方法签名的构造方法（实例类型被看做返回类型）。

`FunctionalInterface`不强制继承关系，不需要方法名称相同，只要求方法参数（类型和数量）与方法返回类型相同，即认为方法签名相同。

### 使用Stream
Java从8开始，不但引入了Lambda表达式，还引入了一个全新的流式API：Stream API。它位于`java.util.stream`包中。

**划重点**这个`stream`不同于`java.io`的`InputStream`和`OutputStream`，它代表的是任意Java对象的序列。两者对比如下：

|对比|java.io|java.util.stream|
|:--- |:--- |:--- |
| 存储 | 顺序读写的`byte`或`char` | 顺序输出的任意Java对象实例|
| 用途 | 序列化至文件或网络	 | 内存计算／业务逻辑|

有同学会问：一个顺序输出的Java对象序列，不就是一个List容器吗？

再次划重点：这个Stream和List也不一样，List存储的每个元素都是已经存储在内存中的某个Java对象，而Stream输出的元素可能并没有预先存储在内存中，而是实时计算出来的。

换句话说，List的用途是操作一组已存在的Java对象，而Stream实现的是惰性计算，两者对比如下：

|对比|java.util.List | java.util.stream|
|:--- |:--- |:--- |
| 元素 | 已分配并存储在内存 | 可能未分配，实时计算|
| 用途 | 操作一组已存在的Java对象 | 惰性计算|

Stream看上去有点不好理解，但我们举个例子就明白了。

如果我们要表示一个全体自然数的集合，显然，用List是不可能写出来的，因为自然数是无限的，内存再大也没法放到List中：

    List<BigInteger> list = ??? // 全体自然数?

但是，用Stream可以做到。写法如下：

    Stream<BigInteger> naturals = createNaturalStream(); // 全体自然数

我们先不考虑createNaturalStream()这个方法是如何实现的，我们看看如何使用这个Stream。

首先，我们可以对每个自然数做一个平方，这样我们就把这个Stream转换成了另一个Stream：

    Stream<BigInteger> naturals = createNaturalStream(); // 全体自然数
    Stream<BigInteger> streamNxN = naturals.map(n -> n.multiply(n)); // 全体自然数的平方

因为这个streamNxN也有无限多个元素，要打印它，必须首先把无限多个元素变成有限个元素，
可以用limit()方法截取前100个元素，最后用forEach()处理每个元素，这样，我们就打印出了前100个自然数的平方：

    Stream<BigInteger> naturals = createNaturalStream();
    naturals.map(n -> n.multiply(n)) // 1, 4, 9, 16, 25...
            .limit(100)
            .forEach(System.out::println);

我们总结一下Stream的特点：它可以“存储”有限个或无限个元素。这里的存储打了个引号，是因为元素有可能已经全部存储在内存中，也有可能是根据需要实时计算出来的。

Stream的另一个特点是，一个Stream可以轻易地转换为另一个Stream，而不是修改原Stream本身。

最后，真正的计算通常发生在最后结果的获取，也就是惰性计算。

    Stream<BigInteger> naturals = createNaturalStream(); // 不计算
    Stream<BigInteger> s2 = natural.map(BigInteger::multiply); // 不计算
    Stream<BigInteger> s3 = s2.limit(100); // 不计算
    s3.forEach(System.out::println); // 计算
    
* 惰性计算的特点是：一个`Stream`转换为另一个`Stream`时，实际上只存储了转换规则，并没有任何计算发生。
    
例如，创建一个全体自然数的Stream，不会进行计算，把它转换为上述s2这个Stream，也不会进行计算。
再把s2这个无限Stream转换为s3这个有限的Stream，也不会进行计算。   
只有最后，调用forEach确实需要Stream输出的元素时，才进行计算。我们通常把Stream的操作写成链式操作，代码更简洁：

    createNaturalStream()
        .map(BigInteger::multiply)
        .limit(100)
        .forEach(System.out::println);
        
因此，Stream API的基本用法就是:创建一个`Stream`,然后做若干次转换，最后调用一个求值方法获取真正计算的结果：

    int result = createNaturalStream() // 创建Stream
                .filter(n -> n % 2 ==0) // 任意个转换
                .map(n -> n * n) // 任意个转换
                .limit(100) // 任意个转换
                .sum(); // 最终计算结果

##### 小结
Stream API的特点是：
* Stream API提供了一套新的流式处理的抽象序列；
* Stream API支持函数式编程和链式操作
* Stream可以表示无限序列，并且大多数情况下是惰性求值的。                
                        
#### 创建Stream
要使用Stream，就必须现创建它。创建Stream有很多种方法，我们来一一介绍。

##### Stream.of()
创建Stream最简单的方式是直接用`Stream.of()`静态方法，传入可变参数即创建了一个能输出确定元素的Stream：
    
    import java.util.stream.Stream;
    public class Main {
        public static void main(String[] args) {
            Stream<String> stream = Stream.of("A", "B", "C", "D");
            // forEach()方法相当于内部循环调用，
            // 可传入符合Consumer接口的void accept(T t)的方法引用：
            stream.forEach(System.out::println);
        }
    }
    
虽然这种方式基本上没啥实质性用途，但测试的时候很方便。

##### 基于数组或Collection
第二种创建Stream的方法是基于一个数组或者Collection，这样该Stream输出的元素就是数组或者Collection持有的元素：

    public class Main {
        public static void main(String[] args) {
            Stream<String> stream1 = Arrays.stream(new String[] { "A", "B", "C" });
            Stream<String> stream2 = List.of("X", "Y", "Z").stream();
            stream1.forEach(System.out::println);
            stream2.forEach(System.out::println);
        }
    }
把数组变成Stream使用Arrays.stream()方法。对于Collection（List、Set、Queue等），直接调用stream()方法就可以获得Stream。
上述创建Stream的方法都是把一个现有的序列变为Stream，它的元素是固定的。

##### 基于Supplier
创建Stream还可以通过Stream.generate()方法，它需要传入一个Supplier对象：

    Stream<String> s = Stream.generate(Supplier<String> sp);
    
**基于Supplier创建的Stream会不断调用Supplier.get()方法来不断产生下一个元素，这种Stream保存的不是元素，而是算法，它可以用来表示无限序列。**

例如，我们编写一个能不断生成自然数的Supplier，它的代码非常简单，每次调用get()方法，就生成下一个自然数：

    public class Main {
        public static void main(String[] args) {
            Stream<Integer> natual = Stream.generate(new NatualSupplier());
            // 注意：无限序列必须先变成有限序列再打印:
            natual.limit(20).forEach(System.out::println);
        }
    }

    class NatualSupplier implements Supplier<Integer> {
        int n = 0;
        public Integer get() {
            n++;
            return n;
        }
    }
上述代码我们用一个Supplier<Integer>模拟了一个无限序列（当然受int范围限制不是真的无限大）。如果用List表示，即便在int范围内，也会占用巨大的内存，而Stream几乎不占用空间，因为每个元素都是实时计算出来的，用的时候再算。

对于无限序列，如果直接调用forEach()或者count()这些最终求值操作，会进入死循环，因为永远无法计算完这个序列，所以正确的方法是先把无限序列变成有限序列，例如，用limit()方法可以截取前面若干个元素，这样就变成了一个有限序列，对这个有限序列调用forEach()或者count()操作就没有问题。

##### 其他方法
创建Stream的第三种方法是通过一些API提供的接口，直接获得Stream。

例如，Files类的lines()方法可以把一个文件变成一个Stream，每个元素代表文件的一行内容：

    try (Stream<String> lines = Files.lines(Paths.get("/path/to/file.txt"))) {
        ...
    }
    
此方法对于按行遍历文本文件十分有用。

另外，正则表达式的Pattern对象有一个splitAsStream()方法，可以直接把一个长字符串分割成Stream序列而不是数组：

    Pattern p = Pattern.compile("\\s+");
    Stream<String> s = p.splitAsStream("The quick brown fox jumps over the lazy dog");
    s.forEach(System.out::println);
    
##### 基本类型
因为Java的泛型不支持基本类型，所以我们无法用Stream<int>这样的类型，会发生编译错误。为了保存int，只能使用Stream<Integer>,但这样会产生频繁
的装箱、拆箱操作。为了提高效率，Java标准库提供了`IntStream`、`LongStream`和`DoubleStream`这三种使用基本类型的`Stream`,它们的使用方法和
泛型Stream没有大的区别，设计这三个Stream的目的是提高运行效率：
  
    // 将int[]数组变为IntStream:
    IntStream is = Arrays.stream(new int[] { 1, 2, 3 });
    // 将Stream<String>转换为LongStream:
    LongStream ls = List.of("1", "2", "3").stream().mapToLong(Long::parseLong);
    
##### 练习
编写一个能输出斐波拉契数列（Fibonacci）的LongStream：

1, 1, 2, 3, 5, 8, 13, 21, 34, ...
    
    public class Main {
        public static void main(String[] args) {
            LongStram fib = LongStram.generate(new FibSupplier());
            // 打印Fibonacci数列
            fib.limit(10).forEach(System.out::println);
        }
    }
    
    class FibSupplier implements LongSupplier {
        long n1 = 0;
        long n2 = 0;
        long n3 = 1;
        public long getAsLong() {
            n1 = n2;
            n2 = n3;
            n3 = n1 + n2;
            return n2;
        }
    }    
    
##### 小结
创建`Stream`的方法有：
* 通过指定元素、指定数组、指定Collection创建Stream;
* 通过Supplier创建Stream,可以是无限序列；
* 通过其他类的相关方法创建。
* 基本类型的Stream有IntStream、LongStream和DoubleStream。
  
  
#### 使用map
Stream.map()是Stream最常用的一个转换方法，它把一个Stream转换为另一个Stream。
所谓map操作，就是把一种操作运算，映射到一个序列的每一个元素上。例如，对`x`计算它的平方，可以使用函数`f(x) = x * x`。我们把这个函数映射到一个
序列1,2,3,4,5上，就得到了另一个序列1,4,9,16,25：

      f(x) = x * x
    
                      │
                      │
      ┌───┬───┬───┬───┼───┬───┬───┬───┐
      │   │   │   │   │   │   │   │   │
      ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼
    
    [ 1   2   3   4   5   6   7   8   9 ]
    
      │   │   │   │   │   │   │   │   │
      │   │   │   │   │   │   │   │   │
      ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼
    
    [ 1   4   9  16  25  36  49  64  81 ]
    
可见，map操作，把一个Stream的每个元素一一对应到应用了目标函数的结果上。

    Stream<Integer> s = Stream.of(1, 2, 3, 4, 5);
    Stream<Integer> s2 = s.map(n -> n * n);
    
如果我们查看Stream的源码，会发现map()方法接收的对象是Function接口对象，它定义了一个apply（）方法，负责把一个`T`类型转换成`R`类型：
    
    <R> Stream<R> map(Function<? super T, ? extends R> mapper);
其中`Function`的定义是：
    
    @FunctionalInterface
    public interface Function<T, R> {
        // 将T类型转换为R
        R apply(T t);
    }    
    
利用`map()`,不但能完成数学计算，对于字符串操作，以及任何Java对象都是非常有用的。例如：

    public class Main {
        public static void main(String[] args) {
            List.of("  Apple ", " pear ", " ORANGE", " BaNaNa ")
                .stream()
                .map(String::trim) // 去空格
                .map(String::toLowerCase) // 变小写
                .forEach(System.out::println); // 打印
        }
    }    
            
通过若干步`map`转换，可以写出逻辑简单、清晰的代码。

##### 练习
使用map（）把一组`String`转换为`LocalDate`并打印。    
    
    Stream<String>  stringStream = Arrays.stream(new String[]{"2020-04-01","2020-04-02","2020-04-03","2020-04-04"});
    stringStream.map(LocalDate::parse).forEach(System.out::println);

    public class MapProblem {
        public static void main(String[] args) {
            String[] array = new String[] {
                    " 2019-12-31 ", "2020 - 01-09", "2020- 05 - 01 ",
                    "2022 - 02 - 01", "2025-01 -01"};
            Arrays.stream(array)
                    .map(s -> s.replaceAll("\\s+", ""))
                    .map(LocalDate::parse)
                    .forEach(System.out::println);
        }
    }        
##### 小结
`map()`方法用于将一个`Stream`的每个元素映射成另一个元素并转换成一个新的`Stream`;

**可以将一种元素类型转换成另一种元素类型。**

#### 使用filter
Stream.filter()是Stream的另一个常用转换方法。

所谓filter()操作，就是对一个Stream的所有元素一一进行测试，不满足条件的就被“滤掉”了，剩下的满足条件的元素就构成了一个新的Stream。

例如，我们对1，2，3，4，5这个Stream调用filter()，传入的测试函数f(x) = x % 2 != 0用来判断元素是否是奇数，这样就过滤掉偶数，只剩下奇数，因此我们得到了另一个序列1，3，5：

              f(x) = x % 2 != 0
    
                      │
                      │
      ┌───┬───┬───┬───┼───┬───┬───┬───┐
      │   │   │   │   │   │   │   │   │
      ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼
    
    [ 1   2   3   4   5   6   7   8   9 ]
    
      │   X   │   X   │   X   │   X   │
      │       │       │       │       │
      ▼       ▼       ▼       ▼       ▼
    
    [ 1       3       5       7       9 ]
    
用IntStream写出上述逻辑，代码如下：

    public class Main {
        public static void main(String[] args) {
            IntStream.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
                    .filter(n -> n % 2 != 0)
                    .forEach(System.out::println);
        }
    }

从结果可知，经过filter()后生成的Stream元素可能变少。

filter()方法接收的对象是Predicate接口对象，它定义了一个test()方法，负责判断元素是否符合条件：

    @FunctionalInterface
    public interface Predicate<T> {
        // 判断元素t是否符合条件:
        boolean test(T t);
    }
    
filter()除了常用于数值外，也可应用于任何Java对象。例如，从一组给定的LocalDate中过滤掉工作日，以便得到休息日：

    public class Main {
        public static void main(String[] args) {
            Stream.generate(new LocalDateSupplier())
                    .limit(31)
                    .filter(ldt -> ldt.getDayOfWeek() == DayOfWeek.SATURDAY || ldt.getDayOfWeek() == DayOfWeek.SUNDAY)
                    .forEach(System.out::println);
        }
    }
    
    class LocalDateSupplier implements Supplier<LocalDate> {
        LocalDate start = LocalDate.of(2020, 1, 1);
        int n = -1;
        public LocalDate get() {
            n++;
            return start.plusDays(n);
        }
    }
##### 练习
请使用filter()过滤出成绩及格的同学，并打印出名字。

    public class Main {
    
    	public static void main(String[] args) {
    		List<Person> persons = List.of(new Person("小明", 88), new Person("小黑", 62), new Person("小白", 45),
    				new Person("小黄", 78), new Person("小红", 99), new Person("小林", 58));
    		// 请使用filter过滤出及格的同学，然后打印名字:
    		persons.stream()
    		.filter(p -> p.score >= 60)
    		.map(s -> s.name)
    		.forEach(System.out::println);
    	}
    }
    
    class Person {
    	String name;
    	int score;
    
    	Person(String name, int score) {
    		this.name = name;
    		this.score = score;
    	}
    }
    
##### 小结    
使用filter()方法可以对一个Stream的每个元素进行测试，通过测试的元素被过滤后生成一个新的Stream。

#### 使用reduce
`map()`和`filter()`都是`Stream`的转换方法，而`Stream.reduce()`则是Stream的一个聚合方法，它可以把一个Stream的所有元素按照聚合函数聚合成一个结果。

我们来看一个简单的聚合方法：
    
    public class Main {
        public static void main(String[] args) {
            int sum = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9).reduce(0, (acc,n) -> acc + n);
            System.out.println(sum); // 45
        }
    }
    
reduce()方法传入的对象是BinaryOperator接口，它定义了一个apply()方法，负责把上次累加的结果和本次的元素进行运算，并返回累加的结果：

    @FunctionalInterface
    public interface BinaryOperator<T> {
        // Bi操作：两个输入，一个输出
        T apply (T t, T u);
    }    
    
上述代码看上去不好理解，但我们用for循环改写一下，就容易理解了：
    
    Stream<Integer> stream = ...
    int sum = 0;
    for (n : stream) {
        sum = (sum, n) -> sum + n;
    }    
可见，reduce()操作首先初始化结果为指定值(这里是0),紧接着，reduce()对每个元素依次调用`(acc, n) -> acc + n`,其中，`acc`是上次计算的结果：    
    
    // 计算过程:
    acc = 0 // 初始化为指定值
    acc = acc + n = 0 + 1 = 1 // n = 1
    acc = acc + n = 1 + 2 = 3 // n = 2
    acc = acc + n = 3 + 3 = 6 // n = 3
    acc = acc + n = 6 + 4 = 10 // n = 4
    acc = acc + n = 10 + 5 = 15 // n = 5
    acc = acc + n = 15 + 6 = 21 // n = 6
    acc = acc + n = 21 + 7 = 28 // n = 7
    acc = acc + n = 28 + 8 = 36 // n = 8
    acc = acc + n = 36 + 9 = 45 // n = 9
因此，实际上这个reduce()操作是一个求和。
如果去掉初始值，我们会得到一个`Optional<Integer>`:

    Optional<Integer> opt = stream.reduce((acc, n) -> acc + n);
    if(opt.isPresent()) {
        System.out.println(opt.get());
    }
    
这是因为Stream的元素有可能是0个，这样就没法调用reduce()的聚合函数了，因此返回Optional对象，需要进一步判断结果是否存在。
利用reduce(),我们可以把求和改为求积，代码也十分简单：

    public class Main {
        public static void main(String[] args) {
            int s = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9).reduce(1, (acc, n) -> acc * n);
            System.out.println(s); // 362880
        }
    }        
    
** 注意:计算求积时，初始值必须设置为1**

除了可以对数值进行累积计算外，灵活运用reduce()也可以对java对象进行操作。下面的代码演示了如何将配置文件的每一行配置通过map（）和reduce（）操作聚合
成一个Map<String, String>:

    public class Main {
        public static void main(String[] args) {
            // 按行读取配置文件:
            List<String> props = List.of("profile=native", "debug=true", "logging=warn", "interval=500");
            Map<String, String> map = props.stream()
            // 把k=v转换为Map[k]=v
            .map(kv -> {
                String[] ss = kv.split("\\=", 2);
                return Map.of(ss[0], ss[1]);
            })
            // 把所有Map聚合到一个Map:
            .reduce(new HashMap<String, String>(), (m, kv) -> {
                m.putAll(kv);
                return m;
            });
        // 打印结果
        map.forEach((k, v) -> {
            System.out.println(k + "=" + v);
        });
        }
    }
    
##### 小结
`reduce()`方法将一个`Stream`的每个元素依次作用于`BinaryOperator`,并将结果合并。

`reduce()`是聚合方法，聚合方法会立刻对`Stream`进行计算。   
     
#### 输出集合
我们介绍了Stream的几个常见操作：map()、filter()、reduce()。这些操作对Stream来说可以分为两类，一类是转换操作，即把一个Stream转换为另一个Stream，例如map()和filter()，另一类是聚合操作，即对Stream的每个元素进行计算，得到一个确定的结果，例如reduce()。

区分这两种操作是非常重要的，因为对于Stream来说，对其进行转换操作并不会触发任何计算！我们可以做个实验：

    public class Main {
        public static void main(String[] args) {
            Stream<Long> s1 = Stream.generate(new NatualSupplier());
            Stream<Long> s2 = s1.map(n -> n*n);
            Stream<Long> s3 = s2.map(n -> n -1);
            System.out.println(s3); // java.util.stream.ReferencePipeline$3@49476842
        }
    }
    
    class NatualSupplier implements Supplier<Long> {
        long n = 0;
        public Long get() {
            n++;
            return n;
        }
    }
    
因为s1是一个Long类型的序列，它的元素高达922亿亿个，但执行上述代码，既不会有任何内存增长，也不会有任何计算，因为转换操作只是保存了转换规则，无论我们对一个Stream转换多少次，都不会有任何实际计算发生。

而聚合操作则不一样，聚合操作会立刻促使Stream输出它的每一个元素，并依次纳入计算，以获得最终结果。所以，对一个Stream进行聚合操作，会触发一系列连锁反应：
   
    Stream<Long> s1 = Stream.generate(new NatualSupplier());
    Stream<Long> s2 = s1.map(n -> n * n);
    Stream<Long> s3 = s2.map(n -> n - 1);
    Stream<Long> s4 = s3.limit(10);
    s4.reduce(0, (acc, n) -> acc + n);
    
    
    
 
#### 其他操作
    
## 设计模式
## Web开发
## Spring开发
## Spring Boot开发
## Spring Cloud开发

