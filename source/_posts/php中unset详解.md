---
title: php中unset详解
date: 2018-06-20 08:59:33
tags: [php,unset]
categories: 
- php
---

unset()经常会被用到,用于销毁指定的变量,但它有自己的行为模式,如果
不仔细的话可能被中文解释给迷惑:

先来看看官方文档的说法:

unset ---- unset a given variable

void unset(mixed $var [,mixed $...]);

parameters:

var:The variable to be unset. //要unset的变量

...Anther variable... // 其他需要unset的变量

return Values:No value is returned.  //unset不返回值


Because this is a language construct and not a function,it
cannot be called using variable functions

//unset()是语言结构,不是函数,因此不能被函数变量调用,具体参照函数变量.

使用function_exists('unset')返回的false,以此证明unset并不是一个函数
,所以无法使用$fun='unset';$fun()的方式调用unset()

it is possible to unset even object properties visible in current context.

// 通用环境下unset可以销毁对象或者对象的可见属性(public)

It is not possible to unset $this inside on object method since PHP5

// 在PHP5之前unset无法销毁对象中的$this方法

when using unset() on inaccessible object properties,the _unset()
overloading method will be called,if declare.

当unset()无法销毁对象中的属性,例如私有属性,保护属性,那么会自动加载对象中的_unset
方法.

description:

unset()destroys the specified variables. //unset()销毁指定的变量

The behavior of unset()inside of a function can vary depending

on what type of variable you are attempting to destroy.

// unset()的行为在函数内部可以根据你所指定销毁的变量类型变化.


情况一:

if a globalized variable is unset() inside of a function,only the local
variable is destroyed.The variable in the calling environment will 
retain the same value as before unset() was called.

如果在函数内使用一个global使其全局化的变量,使用unset进行销毁,那么只有局部的
变量会被销毁,在调用环境的变量将会保留没有unset()销毁之前的调用的变量值.

the example:

    <?php  
    function destroy_foo()   
    {  
        global $foo;  
        unset($foo);  
    }  
      
    $foo = 'bar';  
    destroy_foo();  
    echo $foo;  
    ?>  
    
the above example will output:bar

这是官方文档的例子,可能这样还是不太明显,把上面的例子改成下面这样,一切就很清晰了.

    <?php   
    function des(){  
        global $foo;  
        $foo='bars';  
        unset($foo);  
        echo $foo;  
    }  
    $foo='bar';  
    echo "The globalized variable is unset() inside of a function:";  
    des();  
    echo "<br/>";  
    echo "The variable in the calling environment:";  
    echo $foo;  



