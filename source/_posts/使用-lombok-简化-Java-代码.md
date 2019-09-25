---
title: '使用 lombok 简化 Java 代码  '
date: 2019-09-25 10:27:07
tags: [java,lombok]
categories: [Lombok]
---

# 一个典型的 Java 类

    public class A {
    
      private int a;
    
      private String b;
    
      public int getA() {
        return a;
      }
    
      public String getB() {
        return b;
      }
    
      public void setA(int a) {
        this.a = a;
      }
    
      public void setB(String b) {
        this.b = b;
      }
    
    }

对于这样一个简单的 Java 类，我们通常需要给每个属性写getter和setter，而这种实际上没有什么太大的意义。当然，如果有的公司或团队使用代码行数评估工作量，还是多写几行吧；同时，可以考虑一下我们团队。

# 使用 lombok，简化代码
为了简化getter与setter，lombok 提供了一种机制，帮助我们自动生成这些样板代码。以上的代码，如果使用lombok的话，将变得很简单：

    @lombok.Getter
    @lombok.Setter
    public class A {
    
        private int a;
    
        private String b;
    
    }

顾名思义，lombok.Getter就是生成getter，lombok.Setter就是生成setter。但是，这样真的就可以了么？编译下，让我们看看生成的二进制代码。(请自行下载lombok.jar)

    命令行> javac -cp lombok.jar A.java
    命令行> javap -c A.class

输出结果略。可以看到完全一样。

更进一步，如果在编译的时候，加入-g:none选项，甚至可以看到生成的文件完全一样。

# 简单使用
    
虽然我们可以在编译的时候，加入classpath，但是，一般来说，在各类IDE中使用，还是需要特殊处理一下。

## Maven
加上依赖就好。同时，由于lombok只在编译期才处理，所以并不需要在运行时有这个依赖，可以把scope定义为provided。

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.8</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

值得注意的是，maven的maven-compiler-plugin低版本和lombok高版本不兼容，目前已知maven-compiler-plugin的2.3.X与lombok的1.6.X不兼容。这个需要了解lombok的原理才能进一步说明。

## Eclipse
由于eclipse的默认编译器并不是javac，所以，需要额外安装，基本就是改下引导参数，可以直接运行jar包，或者手动在eclipse.ini里加上参数-Xbootclasspath/a:lombok.jar -javaagent:lombok.jar。

## IDEA IntelliJ
    
虽然IDEA IntelliJ默认使用javac作为编译器，理论上可以不装插件。可是，跳转等特性也随之没了。所以，还是安装个插件吧，直接去仓库里搜索lombok就成。

如果项目中使用高级配置，需要额外注意一下。虽然在编译的时候，lombok配置文件可以在任何能找到的目录，但是，lombok-intellij插件默认并不支持在任何目录，如果有配置文件，建议放在java的源代码根目录中。    

# 更多 lombok 注解
| 注解 | 解释 |
| :-- | :-- |
| @val | 如果你要定义一个final的变量，并且不想写类型，这个可以帮到你。但是，在实际项目中，完全没有使用到。|
| @NonNull | 这个在参数中使用，如果调用时传了null，就直接抛空指针。 |
| @Data | @ToString、@EqualsAndHashCode、@Getter、@Setter和@RequiredArgsConstructor注解的集合。|
| @Getter与@Setter|作用于属性和类上，自动生成属性的getXXX()和setXXX()方法。若在类上，则对所有属性有效。并可通过AccessLevel参数控制方法的访问级别。|
|@ToString|作用于类，自动重写类的ToString()方法。常用的参数有exclude（指定方法中不包含的属性）、callSuper（方法中是否包含父类ToString()方法返回的值）|
|@EqualsAndHashCode|作用于类，自动重写类的equals()、hashCode()方法。常用的参数有exclude（指定方法中不包含的属性）、callSuper（方法中是否包含父类ToString()方法返回的值）|
|@NoArgsConstructor, @RequiredArgsConstructor和@AllArgsConstructor|作用于类，@NoArgsConstructor自动生成不带参数的构造方法；@RequiredArgsConstructor自动生成带参数的构造方法，主要针对一些需要特殊处理的属性，比如未初始化的final属性；@AllArgsConstructor自动生成包含所有属性的构造方法。|
|@Synchronized|作用于方法，可锁定指定的对象，如果不指定，则默认创建创建一个对象锁定。|
|@Log，或者直接@Slf4j| 作用于类，具体包含@CommonsLog、@Log、@Log4j、@Log4j2、@Slf4j和@XSlf4j，分别对用不同的日志系统。利用此类注解，可为类创建一个log属性。|

## sonar源码审查

sonar是一个源码审查工具。最新版5.X已经支持lombok的全部注解，不再认为是没有使用的变量。但是，旧的4.X还是认为没有使用这些变量。可以后向移植这些包，或者应用单独的补丁。

[摘自](https://segmentfault.com/a/1190000005133786)

[sonar](https://docs.sonarqube.org/latest/)

                  

 

               

 

                                                                  



                    

 

           

 

                  

  
