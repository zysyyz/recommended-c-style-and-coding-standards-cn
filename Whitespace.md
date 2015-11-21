## 6. 空格 ##
```
int i;main(){for(;i["]<i;++i){--i;}"];read('-'-'-',i+++"hell\
o, world!\n",'/'/'/'));}read(j,i,p){write(j/p+p,i---j,i/i);}
- 不光彩的事情，模糊C代码大赛，1984年。作者要求匿名。
```

通常情况下，请使用纵向和横向的空白。缩进和空格应该反映代码的块结构。例如，在一个函数定义与下一个函数的注释之间，至少应该有两行空白。

如果一个条件分支语句过长，那就应该将它拆分成若干单独的行。

```
    if (foo->next==NULL && totalcount<needed && needed<=MAX_ALLOT
        && server_active(current_input)) { ...
```

也许下面这样更好

```
    if (foo->next == NULL
        && totalcount < needed && needed <= MAX_ALLOT
        && server_active(current_input))
    {
        ...
```

类似地，复杂的循环条件也应该被拆分为不同行。

```
    for (curr = *listp, trail = listp;
        curr != NULL;
        trail = &(curr->next), curr = curr->next )
    {
        ...
```

其他复杂的表达式，尤其是那些使用了?:操作符的表达式，最好也能拆分成多行。

```
    c = (a == b)
        ? d + f(a)
        : f(b) - d;
```

当关键字后面有放在括号内的表达式时，应该使用空格将关键字与左括号分隔(sizeof操作符是个例外)。在参数列表中，我们也应该使用空格显式 的将各个参数隔开。然而，带有参数的宏定义一定不能在名字与左括号间插入空格，否则C预编译器将无法识别后面的参数列表。