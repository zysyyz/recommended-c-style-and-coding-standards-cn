## 20. Make ##

另外一个非常有用的工具是make。在开发过程中，make只会重新编译那些上次make后发生了改变的模块。它也可以用于自动化其他任务。一些 常见的约定包括：

```
all
     执行所有二进制文件的构建过程

clean
     删除所有中间文件

debug
     构建一个测试用二进制文件a.out或debug

depend
     制作可传递的依赖关系
    
install
     安装二进制文件，库等

deinstall
     取消安装

mkcat
     安装手册

lint
    运行lint工具

print/list
    制作一个所有源文件的拷贝

shar
    为所有源文件制作一个shar文件

spotless
     执行make clean，并将源码存入版本控制工具。注意：不会删除Makefile，即便它是一个源文件。

source
     撤销spotless所做的事情。
 
tags
     运行ctags(建议使用-t标志)

rdist
     分发源码到其他主机

file.c
     从版本控制系统中检出这个文件
```

除此之外，通过命令行也可以定义Makefile使用的值(如"CFLAGS")或源码中使用的值(如"DEBUG")。