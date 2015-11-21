## 4. 声明 ##

全局声明应该从第一列开始。在所有外部数据声明的前面都应该放置extern关键字。如果一个外部变量是一个在定义时大小确定的数组，那么这个数组界限必须在extern声明时显示指出，除非数组的大小与数组本身编码在一起了(例如，一个总是以0结尾的只读字符数组)。重复声明数组大小对于一些使用他人编写的代码的人特别有益。

指针修饰符`*`应该与变量名在一起，而不是与类型在一起。

```
char        *s, *t, *u;
```

替换

```
char*    s, t, u;
```

后者是错误的，因为实际上t和u并未如预期那样被声明为指针。

不相关的声明，即使是相同类型的，也应该独立占据一行。我们应该对声明对象的角色进行注释，不过当常量名本身足以说明角色时，使用#define定义的常量列表则不需要注释。通常多行变量名、值与注释使用相同缩进，使得他们在一列直线上。尽量使用Tab字符而不是空格。结构体和联合体的声明时，每个元素应该单独占据一行，并附带一条注释。{应该与结构体的tag名放在同一行，}应该放在声明结尾的第一列。

```
struct boat {
    int        wllength;    /* water line length in meters */
    int        type;        /* see below */
    long        sailarea;    /* sail area in square mm */
};

/* defines for boat.type */
#define    KETCH    (1)
#define    YAWL        (2)
#define    SLOOP    (3)
#define    SQRIG    (4)
#define    MOTOR    (5)
```

这些defines有时放在结构体内type声明的后面，并使用足够的tab缩进到结构体成员成员的下一级。如果这些实际值不那么重要的话，使用enum会更好。

```
enum bt { KETCH=1, YAWL, SLOOP, SQRIG, MOTOR };
struct boat {
    int        wllength;    /* water line length in meters */
    enum bt    type;        /* what kind of boat */
    long        sailarea;    /* sail area in square mm */
};
```

任何初值重要的变量都应该被显式地初始化，或者至少应该添加注释，说明依赖C的默认初始值0。空初始化"{}"应该永远不被使用。结构体初始化应该用大括号完全括起来。用于初始化长整型(long)的常量应该使用显式长度。使用大写字母，例如2l看起来更像21，数字二十一。

```
int        x = 1;
char        *msg = "message";
struct boat    winner[] = {
    { 40, YAWL, 6000000L },
    { 28, MOTOR, 0L },
    { 0 },
};
```

如果一个文件不是独立程序，而是某个工程整体的一部分，那么我们应该最大化的利用static关键字，使得函数和变量对于单个文件来说是局部范畴的。只有在有清晰需求且无法通过其他方式实现的特殊情况时，我们才允许变量被其他文件访问。这种情况下应该使用注释明确告知使用了其他文件中的变量；注释应该说明其他文件的名字。如果你的调试器遮蔽了你需要在调试阶段查看的静态对象，那么可以将这些变量声明为STATIC，并根据需要决定是否#define STATIC。

最重要的类型应该被typedef，即使他们只是整型，因为独立的名字使得程序更加易读(如果只有很少的几个integer的typedef)。结构体在声明时应该被typedef。保持结构体标志的名字与typedef后的名字相同。

```
typedef struct splodge_t {
    int    sp_count;
    char    *sp_name, *sp_alias;
} splodge_t;
```

总是声明函数的返回类型。如果函数原型可用，那就使用它。一个常见的错误就是忽略那些返回double的外部数学函数声明。那样的话，编译器就会假定这些函数的返回值为一个整型数，并且将bit位逐一尽职尽责的注意转换为一个浮点数(无意义)。

_"C语言的观点之一是程序员永远是对的"_  -- Michael DeCorte