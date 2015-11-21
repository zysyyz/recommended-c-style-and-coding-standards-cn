## 17. 标准C ##

现代C编译器支持一些或全部的ANSI提议的标准C。无论何时可能的话，尽量用标准C编写和运行程序，并且使用诸如函数原型，常量存储以及volatile(易失性)存储等特性。标准C通过给优化器提供有有效的信息以提升程序的性能。标准C通过保证所有编译器接受同样的输入语言以及提供相关机制隐藏机器相关内容或对于那些机器相关代码提供警告的方式提升代码的可移植性。

### 17.1 兼容性 ###

编写很容易移植到老编译器上的代码。例如，有条件地在global.h中定义一些新(标准中的)关键字，比如const和volatile。标准编译器预定义了预处理器符号STDC(见脚注8)。`void*`类型很难简单地处理正确，因为很多老编译器只理解void，但不认识`void*`。最简单的方法就是定义一个新类型VOIDP(与机器和编译器相关)，通常在老编译器下该类型被定义为char**。**

```
#if __STDC__
    typedef void *voidp;
#    define COMPILER_SELECTED
#endif
#ifdef A_TARGET
#    define const
#    define volatile
#    define void int
    typedef char *voidp;
#    define COMPILER_SELECTED
#endif
#ifdef ...
    ...
#endif
#ifdef COMPILER_SELECTED
#    undef COMPILER_SELECTED
#else
    { NO TARGET SELECTED! }
#endif
```

注意在ANSI C中，#必须是同一行中预处理器指示符的第一个非空白字符。在一些老编译器中，它必须是同一行中的第一个字符。

当一个静态函数具有前置声明时，前置声明必须包含存储修饰符。在一些老编译器中，这个修饰符必须是"extern"。对于ANSI编译器，这个存储修饰符必须为static，但全局函数依然必须声明为extern。因此，静态函数的前置声明应该使用一个#define，例如FWD\_STATIC，并通过#ifdef适当定义。

一个"#ifdef NAME"应该要么以"#endif"结尾，要么以`"#endif /* NAME */`结尾，不应该用"#endif NAME"结尾。对于短小的#ifdef不应该使用注释，因为通过代码我们可以明确其含义。

ANSI的三字符组可能导致内容包含??的字符串的程序神秘的中断。

### 17.2 格式化 ###

ANSI C的代码风格与常规C一样，但有两点意外：存储修饰符(storage qualifiers)和参数列表。

由于const和volatile的绑定规则很奇怪，因此每个const或volatile对象都应该单独声明。

```
int const *s;        /* YES */
int const *s, *t;    /* NO */
```

具备原型的函数将参数声明和定义归并在一个参数列表中了。应该在函数的注释中提供各个参数的注释。

```
/*
 * `bp': boat trying to get in.
 * `stall': a list of stalls, never NULL.
 * returns stall number, 0 => no room.
 */
    int
enter_pier (boat_t const *bp, stall_t *stall)
{
    ...
```

### 17.3 原型 ###

Function prototypes should be used to make code more robust and to make it run faster. Unfortunately, the prototyped declaration

应该使用函数原型使得代码更加健壮并且运行时性能更好。不幸地是原型的声明

`extern void bork (char c);`

与定义不兼容。

```
    void
bork (c)
    char c;
 ...
```

原型中c应该以机器上最自然的类型传入，很可能是一个字节。而非原型化(向后兼容)的定义暗示c总是以一个整型传入。如果一个函数具有可类型提升的参数，那么调用者和被调用者必须以相等地方式编译。要么都必须使用函数原型，要么都不使用原型。如果在程序设计时参数就是可以提升类型的，那么问题就可以被避免，例如bork可以定义成接受一个整型参数。

如果定义也是原型化的，上面的声明将工作正常。

```
    void
bork (char c)
{
    ...
```

不幸地是，原型化的语法将导致非ANSI编译器拒绝这个程序。

但我们可以很容易地通过编写外部声明来同时适应原型与老编译器。

```
#if __STDC__
#    define PROTO(x) x
#else
#    define PROTO(x) ()
#endif

extern char **ncopies PROTO((char *s, short times));
```

注意PROTO必须使用双层括号。

最后，最好只使用一种风格编写代码(例如，使用原型)。当需要非原型化的版本时，可使用一个自动转换工具生成。

### 17.4 Pragmas ###

Pragmas用于以一种可控的方式引入机器相关的代码。很显然，pragma应该被视为机器相关的。不幸地是，ANSI pragmas的语法使得我们无法将其隔离到机器相关的头文件中了。

Pragmas分为两类。优化相关的可以被安全地忽略。而那些影响系统行为(需要pragmas)的Pragmas则不能忽略。需要的pragmas应该结合#ifdef使用，这样如果一个pragma都没有选到，编译过程将退出。

两个编译器可能通过两个不同的方式使用同一个给定的pragma。例如，一个编译器可能使用haggis发出一个优化信号。而另一个可能使用它暗示一个特定语句，一旦执行到此，程序应该退出。不过，一旦使用了pragma，它们必须总是被机器相关的#ifdef包围。对于非ANSI编译器，Pragmas必须总是被#ifdef。确保对#pragma的#进行缩进，否则一些较老的预处理器处理它时会挂起。

```
#if defined(__STDC__) && defined(USE_HAGGIS_PRAGMA)
    #pragma (HAGGIS)
#endif
```

> _"ANSI标准中描述的'#pragma'命令具有任意实现定义的影响。在GNU C预处理中，'#pragma'首先尝试运行游戏'rogue'；如果失败，它将尝试运行游戏'hack'；如果失败，它将尝试运行GNU Emacs显示汉诺塔；如果失败，它将报告一个致命错误。无论如何，预处理将不再继续。"_
> -- GNU CC 1.34 C预处理手册。