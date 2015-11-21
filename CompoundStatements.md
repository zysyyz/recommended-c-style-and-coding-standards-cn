## 9. 复合语句 ##



复合语句是一个由括号括起来的语句列表。有许多种常见的括号格式化方式。如果你有一个本地标准，那请你与本地标准保持一致，或选择一个标准，并持续地使用它。在编辑别人的代码时，始终使用那些代码中使用的样式。

```
control {
        statement;
        statement;
}
```

上面的风格被称为"K&R风格"，如果你还没有找到一个自己喜欢的风格，那么可以优先考虑这个风格。在K&R风格中，if-else语句中的else部分以及do-while语句中的while部分应该与结尾大括号在同一行中。而其他大部分风格中，大括号都是单独占据一行的。

当一个代码块拥有多个标签时，每个标签应该单独放在一行上。必须为C语言的switch语句的fall-through特性(即在代码段与下一个case语句之前间没有break)增加注释以利于后期更好的维护。最好是lint风格的注释/指示。

```
switch (expr) {
case ABC:
case DEF:
    statement;
    break;
case UVW:
    statement;
    /*FALLTHROUGH*/
case XYZ:
    statement;
    break;
}
```

这里，最后那个break是不必要的，但却是必须的，因为如果后续另外一个case添加到最后一个case的后面时，它将阻止fall-through错误的发生。如果使用default case，那么应该该default case放在最后，且不需要break，如果它是最后一个case。

一旦一个if-else语句在if或else段中包含一个复合语句，if和else两个段都应该用括号括上(称为全括号(fully bracketed)语法)。

```
if (expr) {
    statement;
} else {
    statement;
    statement;
}
```

在如下面那样的没有第二个else的if-if-else语句序列里，括号也是不必可少的。如果ex1后面的括号被省略，编译器解析将出错：

```
if (ex1) {
    if (ex2) {
        funca();
    }
} else {
    funcb();
}
```

一个带else if的if-else语句在书写上应该让else条件左对齐。

```
if (STREQ (reply, "yes")) {
    statements for yes
    ...
} else if (STREQ (reply, "no")) {
    ...
} else if (STREQ (reply, "maybe")) {
    ...
} else {
    statements for default
    ...
}
```

这种格式看起来像一个通用的switch语句，并且缩进反映了在这些候选语句间的精确切换，而不是嵌套的语句。

Do-while循环总是使用括号将循环体括上。

下面的代码非常危险：

```
#ifdef CIRCUIT
#    define CLOSE_CIRCUIT(circno)    { close_circ(circno); }
#else
#    define CLOSE_CIRCUIT(circno)
#endif

    ...
    if (expr)
        statement;
    else
        CLOSE_CIRCUIT(x)
    ++i;
```

注意，在CIRCUIT没有定义的系统上，语句++i仅仅在expr是假的时候获得执行。这个例子指出宏用大写命名的价值，以及让代码完全括号化的价值。

有些时候，通过break，continue，goto或return，if可以无条件地进行控制转移。else应该是隐式的，并且代码不应该缩进。

```
if (level > limit)
    return (OVERFLOW)
normal();
return (level);
```

平坦的缩进告诉读者布尔测试在密封块的其他部分是保持不变的。