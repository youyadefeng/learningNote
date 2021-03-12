# Effective C++ 11-15 Note

## 条款11：在operator=中处理“自我赋值”

### 确保当对象自我赋值时，拷贝赋值函数有良好的行为。其中的技术包括比较“来源对象”和“目标对象”的地址、精心周到的语句顺序、以及copy-and-swap

比较“来源对象”和“目标对象”：

![image-20210312095803082](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210312095803082.png)

复制：

![image-20210312095932187](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210312095932187.png)

pb在new操作之后已经指向了一块新的内存地址，所以delete原来地址也是没问题的。

copy-and-swap：

![image-20210312100137094](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210312100137094.png)

![image-20210312100147959](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210312100147959.png)

第一个版本为引用版本，需要利用拷贝构造函数构造一个临时对象，然后让源数据与该临时对象进行交换。

第二个版本为值传递版本，此时传入的参数是拷贝而来的，所以直接让源数据与参数进行交换。

### 确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确