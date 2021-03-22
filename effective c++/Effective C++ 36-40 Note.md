# Effective C++ 36-40 Note

## 条款36：绝不重新定义继承而来的non-virtual函数

![image-20210321230315314](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321230315314.png)

如果D重新定义函数mf：

![image-20210321230341698](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321230341698.png)

非虚函数都是静态绑定的，指针是什么类型就调用什么类型。如果派生类重新定义了非虚函数，就会发生遮掩，导致派生类和基类调用的函数版本不一样！

## 条款37：绝不重新定义继承而来的缺省参数值

### 绝对不要重新定义一个继承而来的缺省参数值，因为缺省参数值都是静态绑定的，而virtual函数却是动态绑定的

因为函数参数是动态绑定的，如果在派生类的虚函数中重写了缺省参数值，当通过一个指向派生类的基类指针调用虚函数时，会传入基类版本的缺省值并调用派生类版本的函数体。

## 条款38：通过复合塑造出has-a或“根据某物实现出”

### 复合的意义和public继承完全不同

![image-20210322222850535](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322222850535.png)

复合是has-a关系，这里的person拥有Address和PhoneNumber，就是复合关系

### 在应用域，复合意味着has-a（有一个）。在实现域，复合意味着is-implemented-in-terms-of（根据某物实现出）

## 条款39：明智而审慎地使用private继承

### private继承意味“根据某物实现”，它通常比复合的级别低

由private base class继承而来的所有成员，在派生类中都会变成private属性，纵使它们原来在base class中是public或protected

如果你让class D以private形式继承class B，你的用意是为了采用class B内已经备妥的某些特性，D对象根据B对象实现而得。

private继承意味**只有实现部分被继承**，接口部分应略去（用户看不到继承而来的接口，全变成private了）

复合和private继承都意味着“根据某物实现出”，应尽可能使用复合，必要时才使用private继承。

“当派生类想要访问基类的protected成员”或者“为了重新定义virtual函数”时，应该使用private继承

### 和复合不同，private继承可以造成empty base最优化。这对致力于“对象尺寸最小化”的程序库开发者，可能很重要

![image-20210322225537975](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322225537975.png)

上图使用的是复合，holdsAnInt类含有一个empty对象。虽然empty不含数据成员，但是编译器会强迫安排一个char的大小放到上面，导致空间的浪费

![image-20210322225741042](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322225741042.png)

![image-20210322225810784](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322225810784.png)

对于接口类（只有方法，不含数据成员），考虑使用private继承

## 条款40：明智而审慎地使用多重继承

### 多重继承比单一继承复杂，它可能导致新的歧义性

多重继承可能会导致歧义，当多个基类存在同名函数时，重载的匹配程度是相同的，此时编译器会因为不知道该选择哪一个函数而报错

当出现“钻石型多重继承”时，缺省的方法是复制两次基类，此时需要考虑是否使用虚继承

![image-20210322230437726](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322230437726.png)

### 使用虚基类会增加成本，如果非要使用，最好不要往里面添加数据

使用虚继承会在一定程度上影响效率：使用虚继承的那些classes所产生的对象往往比使用非虚继承的那些类体积大，访问虚基类的成员变量时，也比访问非虚继承基类速度慢

非必要不要使用虚基类，如果必须使用，尽量不要在虚基类中放置数据

### 多重继承的其中一个使用情景是“public继承某个接口类，private继承某个协助实现的类”两者结合

![image-20210322231155973](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322231155973.png)

![image-20210322231227187](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322231227187.png)

![image-20210322231237483](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322231237483.png)