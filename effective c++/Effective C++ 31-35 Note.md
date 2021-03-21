# Effective C++ 31-35 Note

## 条款31：将文件间的编译依存关系降至最低

如果没有很好的实现“将接口从实现中分离”，那么在后续修改类时，重新编译会很耗时间。

当引入头文件时，会出现编译依存关系：

![image-20210321221814722](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321221814722.png)![image-20210321221759841](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321221759841.png)

在使用标准库时，引入标准头文件不会引发编译的相互依赖，可以放心引入标准头文件

编译器必须在编译期间知道对象的大小

### 支持“编译依存最小化”的一般构想是：相依于声明式，不要相依于定义式。具体实现依赖于Handle classes和Interface classes

![image-20210321222357346](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321222357346.png)

![image-20210321222513738](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321222513738.png)

![image-20210321222522813](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321222522813.png)

这里有两个类，一个类是接口类，提供了各种函数的定义及其实现，以及存放了指向实现类的指针

另一个是实现类，存放着所有的数据成员。

### 程序库头文件应该以“完全且仅有声明式”的形式存在。这种做法不论是否设计template都适用

## 条款32：确定你的public继承塑模出is-a关系

public继承意味着is-a。适用于基类身上的每一件事也一定适用于派生类，因为每一个派生类也是一个基类

## 条款33：避免遮掩继承而来的名称

### 派生类的名称会遮掩基类的名称

![image-20210321223354572](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321223354572.png)

注意mf1函数，在基类的两个mf1函数将全部被遮掩。即使派生类中mf1的函数签名和基类完全不一样。

### 为了让被遮掩的名称重见天日，可适用using声明式或转交函数

![image-20210321223614908](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321223614908.png)

using声明式：使用using后，所有同名的mf1函数全都能够使用

![image-20210321223709871](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321223709871.png)

转交函数：派生类函数调用基类的同名函数

## 条款34：区分接口继承和实现继承

### 接口继承和实现继承不一样，在public继承下，派生类总是继承基类的接口

### 声明一个pure virtual函数的目的是为了让派生类只继承接口

### 声明 non-pure virtual函数的目的，是让派生类继承该函数的接口和缺省实现

### 声明 non-virtual函数的目的是为了令派生类继承函数的接口及一份强制性实现

## 条款35：考虑virtual函数以外的选择

### 藉由 Non-Virtual Interface 手法实现 Template Method模式

![image-20210321224707583](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321224707583.png)

public成员函数调用private虚函数

这个方法的好处是可以在调用virtual函数前后做事前和事后工作，如果直接调用virtual函数则做不到

### 藉由 Function Pointers 实现 Strategy 模式

![image-20210321225114644](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321225114644.png)

private数据成员中含有函数指针，在执行构造函数时传入一个函数对其进行初始化。

在public成员函数执行时调用该函数。

这样做的好处是：

同一类型的不同对象可拥有不同的函数，弹性很强

可以提供一个该函数的set函数，让客户自由选择替换

### 藉由 tr1::function 完成 Strategy 模式

![image-20210321225708634](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321225708634.png)

![image-20210321225717865](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321225717865.png)

是上一个方案的升级版，将函数指针变成了function，这样可接收范围就变得更加宽泛了！

### 古典的 Strategy 模式

![image-20210321225945668](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321225945668.png)

![image-20210321225956732](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321225956732.png)

![image-20210321230018275](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321230018275.png)