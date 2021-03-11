# effective c++ 1-5

## 条款01：视C++为一个语言联邦

C++大体上由四个次语言组成：C、object-oriented C++、Template C++、STL



## 条款02：尽量以const、enum、inline替换 #define

### const替换

\#define x 1.653

当上述语句发生错误时，编译器只会报告1.653出错，而不是报告x出错。在大工程环境下，只凭1.653这个数字去debug十分费劲。建议使用const替换上述语句

### enum替换：the enum hack

![image-20210311224520223](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311224520223.png)

在类里面声明一个枚举类型，将枚举类型成员当作常量int初始化数组的维度

### inline替换宏表达式

![image-20210311224754394](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311224754394.png)

![image-20210311224813467](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311224813467.png)





## 条款03：尽可能使用const

### const与迭代器结合

![image-20210311225008615](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311225008615.png)

声明迭代器为const，迭代器的指向不能改变

取得const_iterator，迭代器所指的东西不能被改动

### 下标运算符的常量版本和非常量版本

![image-20210311225226312](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311225226312.png)

如果类中存在下标运算符的常量版本和非常量版本，在通过实例调用下标运算符时：

如果实例是const类型，则调用const版本的下标运算符

如果实例是普通类型，则调用普通版本的下标运算符

### mutable

用mutable修饰的变量，即使在const成员函数里面也可以进行修改

![image-20210311225752301](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311225752301.png)

### 在non-const 和 const 成员函数中避免重复

![image-20210311230027915](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311230027915.png)

如果存在诸如“校验数据完整性、边界检查”这些杂项任务要完成时，可以把这些操作放在const版本成员函数里面执行，执行完毕后再进行取下标值操作。

而为了避免这些杂项代码在non-const版本的成员函数中重复出现，非const版本函数可以直接调用const版本的成员函数。

## 条款04：确定对象被使用前已先被初始化

### 为内置类型手工初始化

内置类型需要手动初始化，而其它类型则由构造函数完成初始化

### 不要混淆赋值和初始化

在构造函数中，初始值列表进行的才是初始化。而函数体内的赋值语句进行的是赋值。所以**养成在初始值列表里列出所有成员的习惯**

使用copy赋值函数时：左边对象先进行默认初始化，然后再调用赋值的重载函数

### 类成员初始化次序问题

类内成员按照声明的先后次序依次进行初始化。

### 不同编译单元内定义之non-local static对象的初始化次序问题

![image-20210311232459346](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232459346.png)

**编译单元**：是指产出单一目标文件的那些源码，基本上它是单一源码文件加上其所引入的头文件

![image-20210311231904454](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311231904454.png)

实例：

![image-20210311231948883](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311231948883.png)

注意tfs用extern修饰，是一个外部文件。

![image-20210311232211313](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232211313.png)

Directory的构造函数用到tfs，当执行Directory的构造函数时：

![image-20210311232129342](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232129342.png)

![image-20210311232139710](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232139710.png)

**解决方法：将每个non-local static对象搬到自己的专属函数里面去**

![image-20210311232715131](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232715131.png)

![image-20210311232723863](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232723863.png)

![image-20210311232841235](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232841235.png)

![image-20210311232756247](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311232756247.png)

## 条款05：了解C++静默编写并调用哪些函数

### 默认拷贝构造函数工作方法

如果数据成员是类，调用相应的拷贝构造函数。

如果数据成员是内置类型，会**拷贝该成员内的每一位bit**

### C++不允许“引用类型”改指不同对象

![image-20210311233451256](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311233451256.png)

![image-20210311233508645](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210311233508645.png)

最后一行赋值语句会调用类的默认拷贝赋值函数，但这样会引发错误：

“引用不能改指”，p中的引用类型nameValue不能改指向s中的nameValue

“常量不可改变”，p中的常量objectValue不能赋s中的新值

除了这两种情况不能生成默认的拷贝赋值函数之外，还有一种情况：

“基类将拷贝构造函数声明为private”，派生类因为无法调用基类的拷贝构造函数而无法对基类部分赋值。





