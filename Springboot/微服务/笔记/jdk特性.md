# JDK特性
## JDK9
- jdk9支持jshell，类似于python的交互式编程，可以直接在命令行中输入代码，不需要写main方法，直接运行
- jdk9支持模块化，使用module-info.java来声明一个模块一个模块只能有一个文件，且在顶层包同目录下。使用exports来声明可以被外部引用的包可以有多个exports语句。使用requires来声明依赖的外部的模块可以有多个requires语句
## JDK10
- jdk10支持局部变量类型推断，使用var来声明局部变量，但是必须初始化，且不能为null
## JDK11
- 单文件程序，使用java文件直接运行，不需要编译
- shebang #!符号叫做shebang音译成"释伴"，即"解释伴随行"<br>/bin/bash+以此开头的文件，在执行时会实际调用/bin/bash程序来执行<br>#!D:\tools\jdk-17.0.5\bin\java --source 11
## JDK14
- 文本块，使用"""来声明文本块，可以直接换行，不需要使用\n
- instanceof模式匹配，使用instanceof来判断类型，如果是则直接强转，不需要再次强转
- 空指针提示，使用Objects.requireNonNull来判断是否为空，如果为空则抛出空指针异常
## JDK16
- record类，使用record来声明一个类，可以直接使用构造方法，不需要再写get/set方法
## JDK17
- switch表达式，使用switch表达式，不需要再写break，直接写->返回值
- sealed类，使用sealed来声明一个类，可以使用permits来声明可以继承的类，不可以继承的类使用final来修饰
