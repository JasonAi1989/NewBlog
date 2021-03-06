title: Ruby elementary 2
toc: true
date: 2015-04-07 14:46:42
tags: [Ruby]
categories: Ruby
description: The basic knowledge about the Ruby
feature: http://7xj4cp.com1.z0.glb.clouddn.com/ruby.png
---

### 语句块
    BEGIN{
        code
    }
运行在程序开始

    END{
        code
    }
运行在程序结尾

<!--more-->

### 注释块
    =begin
        code
    =end
也可以用#来做注释
### 文件和行的关键字
    __FILE__ 是文件的关键字
    __LINE__ 是行的关键字
    
### 数据类型
基本类型：Number、String、Ranges、Symbols

特殊值：true、false、nil

重要类型：Array和Hash

#### Number
可分为整型和浮点型，当出现下划线时，下划线可被忽略，如

        a=1_000_000
        puts a
        
结果为 1000000

#### String为字符串
在字符串中存在使用\的转移字符，同时在字符串中可以使用#{code}来插入ruby表达式的结果，如下：
    
    puts "hello\nworld"
    puts "Here is #{__LINE__}"
    
结果为：

    hello
    world
    Here is 10
    
#### Ranges为范围
可以使用三种方式定义一个范围类型

    (1..5)  结果为1，2，3，4，5
    (1...5)  结果为1，2，3，4
    Range.new 这个不清楚
    
#### Array数组
定义方式：

    a=["hello", 12, "world"]
  
在我看来，ruby的数组是一种泛型的数组，不想c语言中，一个数组中的所有元素类型都是一样的，在ruby的数组中，可以有多种元素的类型。

在数组中可以调用方法length计算数组的长度

    a.length

#### Hash哈希表
哈希表要知道键和值的概念，用键去找值，定义方式如下：
    
    a={"red"=>123, "yellow"=>20, "white"=>89}
    
### 类和对象
五种变量类型（准确的说应该是五种变量的作用范围）

    局部变量：    a, _a
    实例变量：    @a
    类变量：      @@a
    全局变量：    $a
    常量         VAR
    
注：常量必须以大写字母开头，常量只有在定义时能赋值（初始化），之后不允许修改其值，常量不能在方法中被定义，但可以在类中被定义（此时只被此类使用）

##### 类的定义
    
    clase Test
        code
    end
    
定义一个名字为test的类

##### 函数定义

    def func
        code
    end
    
定义一个名字为func的函数

注：

+ 使用#可以取变量的值，比如#a, #_a, #@a, #@@a, #$a

+ 类名的首字母一定要大写
+ 在 Ruby 中，类和方法也可以被当作常量。

### 运算符
ruby中的运算符和c中有很多相同的，在这里只列出与c用含义不同的运算符。

> ** 指数运算符， 10**3，结果为10的3次方

> <=> 联合运算符，相当于字符串里面的strcmp函数，会有0，1，-1三种返回值

> .eql?	如果接收器和参数具有相同的类型和相等的值，则返回 true。	1 == 1.0 返回 true，但是 1.eql?(1.0) 返回 false。

> \*\*= 例如：a\*\*=3 等价于 a=a**3

>..和...   这两个为范围运算符，（1..5）为1~5，(1...5)为1~4

>:: 调用类中的常量值，同时也可以用于将类中的常量值升级到全局变量，如

    情形一
    class Test
        VAR = 10
    end
    
    puts Test::VAR   #显示为10
    puts VAR  #报错
    
    情形二
    class Test
        ::VAR = 10
    end
    
    puts Test::VAR   #显示为10
    puts VAR #显示为10
注：ruby中没有自增和自减运算符
### 判断（3类5种）
3类判断方式分别为：if, unless, case
5种判断方式分别为：if和if后缀模式，unless和unless后缀模式，case
先来说一下非后缀模式的3种，和c语言很像。

##### if

    if condition
        code
    elsif
        code
    else
        code
    end
    
##### unless
    
    unless condition
        code
    else
        code
    end
    
##### case

    case args
    when a
        code
    when b
        code
    else
        code
    end
    
>注：

>+ unless和case中没有elsif关键字，unless的条件为假时执行他的代码

>+ if/elsif/unless/when的行尾可以添加then，但这不是必须的，then是可选关键字

2中后缀模式
    
    code if condition  #condition为true时执行code
    code unless condition  #condition为false时执行code

### 循环以及打断
循环3种，分别为：while，until和for

打断4种，分别为：break，next，redo，retry

##### while
    
    while condition [do]
        code
    end
    
##### until
    
    until condition [do]
        code
    end
    
##### for

    for arg in expression [do]
        code
    end
    
和判断一样，循环也拥有后缀模式

##### while后缀模式

    code while condition
    
或者

    begin
        code
    end while condition
这面这种情况会先执行一次code再判断，相当于c语言中的do...while结构

##### until后缀模式

    code until condition
    
或者

    begin
        code
    end until condition
    
##### break
同c语言中break语义一样

##### next
同c语言中continue语义一样

##### redo
重做，有点类似next，不同在于for循环中next会进入下一次循环，而redo还在此次循环中重新执行

##### retry
这个主要是和异常处理rescue关键字配合使用，当异常处理结束时可以使用retry重新执行之前的程序，具体详见后文。。。

### 方法
方法就是c语言中的函数，方法可以定义在类内，也可以定义在类外，类内的方法默认是public的属性，类外的方法默认是private的属性，如果想调用类内方法需要先实例化此类，或者使用如下方式定义此方法：

    class Test
        def Test.func
            code
        end
    end
    
    Test.func
    
用类名.方法名的方式定义一个类内方法，可以直接在类外不需实例化就可被调用

##### 别名
对方法可以定义一个别名，但只能在类外进行定义

    alias foo  bar
其中bar是方法名，foo是新定义的别名

##### 返回值
默认情况下，方法内的最后一条语句的值就是返回值，但也可以使用return进行返回，使用return可以返回多个值，比如

    def func
        code
        return a,b,c
    end
    
    d=func
会将a,b,c三个值进行返回，返回值的接收者d会接收最后一个值c

##### 可变数量参数
c语言中有可变数量的参数，ruby中也有

    def func(*test)
        code
        len=test.length
    end
    
    func "a" "b" 2
    
这里的test就是一个可变数量的参数，他其实是一个数组

##### 方法的首字母
方法的首字母一定要小写，不然会被解析器当作常量，导致出现问题

现在总结一下各个字母大小写的问题

> 类名首字母要大写

> 常量首字母要大写

> 变量首字母要小写

> 方法名首字母要小写

### 块
在ruby中，块是一个很重要的概念，他能让你更灵活的使用方法。

首先要记住一点，**块最终会运行在同名的方法中**。（BEGIN和END块除外）

下面是块的概念：

+ 块由大量的代码组成
+ 块需要一个名字
+ 块中的代码需要写在｛｝内
+ 块会在同名的方法中被运行
+ 可以在方法内通过yield来调用块

例如：

    def func1
        puts "here is first line"
        yield 10   #块在此处被调用，参数为10
        puts "end line"
    end

    func1 {|n| puts "milld line #{n}"}  #方法func1在此处被调用，n为参数
    
块中的参数要用||包起来，里面可以放多个参数，并用逗号分割

除了使用yield外还可以使用call来调用一个块，例如

    def func2(&arg)
        puts "start"
        arg.call(6,7)
        puts "end"
    end

    func2 {|n,m| puts "line #{n} and line #{m}"}

块中的内容作为参数被传入到了func2中，接收此内容的为arg,此参数必须为参数列别中的最后一个，并且有符号&作为修饰才能接收块的内容，然后参数arg调用方法call调用了块的内容，n和m为块的参数

方法的特殊参数;
+ *arg,  arg成为一个数组，用于可变参数
+ &arg,  用于接收块的内容，并通过call方法对块内容进行调用

### 模块
在我看来，模块的最大用处是提供了命令空间，并能被外部的类引用（或叫插入），模块的定义方式和类很像

    module Test
        code
    end    

和类一样，模块名的首字母需要大写。在模块内部可以定义常量和方法（函数），与类中方法不一样的地方是，在模块中需要如下定义一个方法：

    module Test
        def Test.func
            code
        end
    end
    
    a=Test.func
    
因为模块是不需要实例化的，所以只能如上定义一个模块方法（还记得如何定义一个不需实例化即可被调用的类方法吗）

##### 引入文件
类似c语言中include引入某个文件，在ruby中使用require引入某个文件

    require filename
    
此处需要注意的是引入文件的位置，我们可以如下指定一个位置

    $LOAD_PATH << "."   #意思为当前目录下的文件
    
或者使用

    require_relative file
    
来引入一个相对位置

注：一个文件可以包含多个模块

##### 在类中引入模块
可以在类中引入某个模块，但前提是已经引入了此模块文件

    include modulename
    
例如

    $LOAD_PATH << "."
    
    require "support.rb"
    
    class User
        include Test
        
        def func
            Test::VAR  #使用Test模块中的VAR常量
        end
    end
    
##### mixin机制
这个机制类似多重继承，但ruby不直接支持多重继承

    module A
        def A.func1
            code
        end
    end
    
    module B
        def B.func2
            code
        end
    end
    
    class User
        include A
        include B
        def func3
            code
        end
    end
    
    a=User.new
    a.func1
    a.func2
    a.cunf3
    
类User同时拥有了模块A和模块B的特性。
    
    


