# Effective C++ 41-45 Note

## 条款41：了解隐式接口和编译器多态

###　Templates及泛型编程的领域中，隐式接口和编译期多态比较重要。

![image-20210322231545096](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322231545096.png)

隐式接口：w必须支持的接口由template中执行于w身上的操作决定：size、normalize、swap、copy构造函数、!=

编译期多态：凡涉及ｗ的任何函数调用，都可能造成template的具现化。不同template参数会导致调用不同的函数



## 条款42：了解typename的双重意义

声明template参数时，typename和class可以互换

### 请使用typename标识嵌套从属名称

从属名称、嵌套从属名称、非从属名称

从属名称：变量的类型取决于某个template参数

嵌套从属名称：从属名称在class呈嵌套状

非从属名称：变量的类型不依赖于template名称

![image-20210323084536683](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323084536683.png)

iter的类型依赖于模板参数C，所以它是从属名称。

iter的类型为C::const_iterator，在嵌套于C，所以iter是嵌套从属名称

value不依赖于模板参数，所以它是非从属名称



这段代码不能通过编译，嵌套从属名称的写法存在二义性:C::const_iterator

可能是C里面存在一个静态成员const_iterator，也可能是C里面存在一个类型const_iterator。编译器默认将其识别为静态成员。想要让编译器识别为类型，请使用typename进行标识：

![image-20210323085058865](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323085058865.png)

### 不得在基类列或者成员初始值内以typename作为基类修饰符

![image-20210323085334805](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323085334805.png)

## 条款43：学习处理模板化基类内的名称

![image-20210323085923006](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323085923006.png)

![image-20210323085935828](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323085935828.png)

![image-20210323090107719](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323090107719.png)

![image-20210323090550731](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323090550731.png)

上述模板皆不能通过编译，因为它试图调用基类中模板参数类型的函数，对于CompanyA的特化版来说是有效的，但是对于CompanyZ的特化版来说是无效的。编译器知道基类类型可能会特化，但那个特化版本可能不提供和一般性template相同的接口。因此**它往往拒接在模板化基类内寻找继承而来的名称**。

解决方法有三：

### 在基类函数调用动作之前加上"->this"

![image-20210323090822694](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323090822694.png)

### 使用using声明

![image-20210323090845972](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323090845972.png)

### 明白指出被调用的函数位于基类内

![image-20210323090931428](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323090931428.png)

这种方法不太好，当该函数为virtual函数时，不能进行动态绑定，一直都只能使用基类的版本

![image-20210323091118958](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323091118958.png)

## 条款44：将与参数无关的代码抽离template

### template生成多个classes和多个函数，所以任何template代码都不该与某个造成膨胀的template参数产生相依关系

![image-20210323091915321](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323091915321.png)

![image-20210323091938776](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323091938776.png)

考虑这两个类型参数：<double, 12> 和 <double, 10>

第一个版本代码，类型和大小共同决定了模板类型。所以会具现化出两个不同版本的invert函数。

第二个版本的代码，因为invert函数位于基类，而基类只有类型决定了模板类型。所以只会具现化出一个invert函数

### 因非类型模板参数而造成的代码膨胀，往往可以通过“以函数参数或成员变量替换template参数”的方法消除

### 因类型参数而造成的代码膨胀，往往可以降低。做法是让带有完全相同二进制表述的具现类型共享实现码

![image-20210323092757597](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323092757597.png)

## 条款45：运用成员函数模板接受所有兼容类型

同一template的不同具现体之间并不存在什么与生俱来的固有关系，这说明即使是基类B和派生类D，分别通过template具现化出来的template<typename B>和template <typename D>之间不存在基类和派生类的关系。

由于上述特性，在使用智能指针时，派生类的智能指针不能转换为基类的智能指针

### 使用成员函数模板生成“可接受所有兼容类型”的函数

![image-20210323223017254](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210323223017254.png)

拷贝构造函数接受一个其它类型的参数，然后在初始化列表中初始化原始指针，在列表中已经隐式要求了U必须能够转换成T

### 在class内声明泛化copy构造函数并不会阻止编译器生成默认的copy构造函数。

如果不想使用默认copy构造函数，需要自己额外再定义

