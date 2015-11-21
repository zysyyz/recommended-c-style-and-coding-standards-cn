## 15. 调试 ##

_"C代码。C代码运行。运行，代码，运行... 请运行!!!"_ -- Barbara Tongue

如果你使用枚举，第一个枚举常量应该是一个非零值，或者第一个常量应该指示一个错误。

```
enum { STATE_ERR, STATE_START, STATE_NORMAL, STATE_END } state_t;
enum { VAL_NEW=1, VAL_NORMAL, VAL_DYING, VAL_DEAD } value_t;
```

未初始化的值后续将会自己获取。

检查所有错误返回值，即使是那些"不能"失败的函数的返回值。考虑即使之前所有的文件操作都已经成功了，close()和fclose也可能失败。编写你自己的函数，使得它们以一种明确的方式测试错误、返回错误码或从程序中退出。包含大量调试和错误检查代码，并把其中大多数留在最终的产品中。甚至检查那些"不可能"的错误。

使用assert机制保证传给每个函数的值都是定义明确的，并且中间结果是形式良好的。

尽可能少的在调试代码中使用#ifdef。例如，如果mm\_malloc是一个调试用的内存分配器，那么MALLOC将挑选合适的分配器，避免使用#ifdef在代码中堆砌垃圾，并且使得分配之间的差异变得清晰，只是在调试期会分配些额外内存。

```
#ifdef DEBUG
#    define MALLOC(size)  (mm_malloc(size))
#else
#    define MALLOC(size)  (malloc(size))
#endif
```

对那些"不可能"溢出的对象做边界校验。一个向变长存储区写入的函数应该接受一个参数maxsize，该参数即目标内存区域的大小。如果有时候目标内存区域大小未知，一些maxsize的"魔数"值应该意味着"没有边界检查"。当边界检查失败，请确保这个函数做一些有用的事情，诸如退出程序或返回一个错误状态。

```
/*
 * INPUT: A null-terminated source string `src' to copy from and
 * a `dest' string to copy to.  `maxsize' is the size of `dest'
 * or UINT_MAX if the size is not known.  `src' and `dest' must
 * both be shorter than UINT_MAX, and `src' must be no longer than
 * `dest'.
 * OUTPUT: The address of `dest' or NULL if the copy fails.
 * `dest' is modified even when the copy fails.
 */
    char *
copy (dest, maxsize, src)
    char *dest, *src;
    unsigned maxsize;
{
    char *dp = dest;

    while (maxsize\-\- > 0)
        if ((*dp++ = *src++) == '\\\\0')
            return (dest);

    return (NULL);
}
```

总之，记住一个程序产生错误答案的速度快两倍(译注：是否有南辕北辙的意味)，实则是变得无限缓慢，这个道理对那些偶尔崩溃或打击有效数据的程序同样成立。