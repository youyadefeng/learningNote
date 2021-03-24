# Effective C++ 46-50 Note

## 条款46：需要类型转换时请为模板定义非成员函数

### 当我们编写一个class template，而它所提供之“于此template相关的”函数支持“所有参数之隐式类型转换”时，请将那些函数定义为“class template内部的friend函数”

先看一个编译错误：

![image-20210323223744063](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323223744063.png)

![image-20210323223752209](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323223752209.png)

上述的乘法无法通过编译，在调用模板函数operator*时，oneHalf的类型是Rational，2的类型是int。编译器不会执行隐式转换，所以它不知道该调用那个类型的具现化函数。

解决方法是将该函数声明为友元函数

![image-20210323224150316](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224150316.png)

![image-20210323224141091](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224141091.png)

![image-20210323224216789](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224216789.png)

而上述代码能够通过编译但是不能通过连接

![image-20210323224527378](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224527378.png)

![image-20210323224534608](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224534608.png)

解决方法：**将该函数定义为template内部的friend函数**

![image-20210323224615621](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323224615621.png)

### 条款47：请使用traits classes表现类型信息

### 5种迭代器类型

输入迭代器：向前移动，一次一步，可读不写，只能读一次（模仿输入文件的阅读指针）

输出迭代器：向前移动，一次一步，可写不读，只能写一次（模仿输出文件的涂写指针）

前向迭代器：向前移动，一次一步，可读可写，可多次读写

双向迭代器：双向移动，一次一步，可读可写，可多次读写（set、multiset、map、multimap的迭代器）

随机访问迭代器：双向移动，一次多步，可读可写，可多次读写（vector、deque、string的迭代器）

对于这些迭代器，C++标准程序库分别提供专属的卷标结构（tag struct）加以确认：

![image-20210323225547795](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323225547795.png)

### 条款48：认识template元编程

### 模板元编程可将工作由运行期移往编译期，因而得以实现早期错误侦测和更高的执行效率

### TMP可被用来生成“基于政策选择组合”的客户定制代码，也可用来避免生成对某些特殊类型并不适合的代码

## 条款49：了解new-handle的行为

### set_new_handler允许客户指定一个函数，在内存分配无法获得满足时调用

![image-20210323230615182](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323230615182.png)

![image-20210323230720531](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323230720531.png)

![image-20210323230732275](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323230732275.png)

一个设计良好的new-handle函数必须做以下事情：

1、让更多内存可被使用

2、安装另一个new-handle

3、写出new-handle，将null指针传给set_new_handler

4、抛出bad_alloc

5、不返回，通常调用abort或exit

### Nothrow new是一个颇为局限的工具，因为它只适用于内存分配；后续的构造函数调用还是可能抛出异常

![image-20210323231752317](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323231752317.png)

当分配内存失败时，不会抛出异常。当分配内存成功，构造函数失败时，也会抛出异常

### 了解new和delete的合理替换时机

### 有许多理由需要写个自定的new和delete

![image-20210323232225103](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232225103.png)

![image-20210323232233738](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232233738.png)

![image-20210323232241571](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232241571.png)

![image-20210323232249233](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232249233.png)

![image-20210323232258758](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232258758.png)

![image-20210323232306732](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323232306732.png)

