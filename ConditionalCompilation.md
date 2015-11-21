## 14. 条件编译 ##

条件编译在处理机器依赖、调试以及编译阶段设定特定选项时十分有用。不过要小心条件编译。各种控制很容易以一种无法预料的方式结合在一起。如果使用#ifdef判断机器依赖，请确保当没有机器类型适配时，返回一个错误，而不是使用默认机器类型(使用#error并缩进一级，这样它可以一些老旧的编译器下工作)。如果你#ifdef优化选项，默认情况下应该是一个未经优化的代码，而不是一个不兼容的程序。确保测试的是未经优化的代码。

注意在#ifdef区域内的文本可能会被编译器扫描(处理)，即使#ifdef求值的结果为假。但即使文件的#ifdef部分永远不能被编译到(例如，#ifdef COMMENT)，这部分也不该随意的放置文本。

尽可能地将#ifdefs放在头文件中，而不是源文件中。使用#ifdef定义可以在源码中统一使用的宏。例如，一个用于检查内存分配的头文件可能这样实现：(省略了REALLOC和FREE)：

```
    #ifdef DEBUG
        extern void *mm_malloc();
    #    define MALLOC(size) (mm_malloc(size))
    #else
        extern void *malloc();
    #    define MALLOC(size) (malloc(size))
    #endif
```

条件编译通常应该基于一个接一个的特性的。多数情况下，都应该避免使用机器或操作系统依赖。

```
    #ifdef BSD4
        long t = time ((long *)NULL);
    #endif
```

上面代码之所以糟糕有两个原因：很可能在某个4BSD系统上有更好的选择，并且也可能存在在某个非4BSD系统中上述代码是最佳代码。我们可以通过定义诸如TIME\_LONG和TIME\_STRUCTD等宏作为替代，并且在诸如config.h的配置文件中定义一个合适的宏。