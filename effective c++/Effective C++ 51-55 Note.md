# Effective C++ 51-55 Note

## 条款51：编写new和delete时需固守常规

### operator new应该内含一个无限循环，并在其中尝试分配内存，如果它无法满足内存需求，就该调用new-handler

![image-20210324104734083](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324104734083.png)

![image-20210324104743171](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324104743171.png)

### operator new应该有能力处理0bytes申请。Class专属版本还应该处理“比正确大小更大的申请”

![image-20210324104916004](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324104916004.png)

![image-20210324104926309](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324104926309.png)

这里的new操作是基类的定制操作，而不是在派生类上使用。当传入一个size时，与基类的大小比较，如果它比基类的大小要大，则判断传入的是派生类，此时要调用标准new而不是定制new

### operator delete应该在收到null指针时不做任何事。Class专属版本则还应该处理“比正确大小更大的（错误）申请”

当接受到的为null指针，就什么也不做

![image-20210324105304453](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324105304453.png)

当接受到的对象不是基类，则执行标准delete而不是定制delete

![image-20210324105501156](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324105501156.png)

### 条款52：写了placement new 也要写 placement delete

### 当你写一个placement operator new，请确定也写出了对应的placement operator delete。如果没有这样做，你的程序可能会发生内存泄漏

如果operator new接受的参数除了一定会有的那个size_t之外还有其它，这便是所谓个所谓的placement new：

![image-20210324110023977](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110023977.png)

![image-20210324110215692](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110215692.png)

对应的delete称为placement delete，如果没有相对应的delete，系统就会不知道如何取消并恢复原先对placement new的调用，于是什么也不做。这样会导致内存泄漏

### 当你写了placement new和placement delete，请确定不要无意识地遮掩了它们的正常版本

![image-20210324110749404](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110749404.png)

![image-20210324110817370](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110817370.png)

![image-20210324110834633](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110834633.png)

![image-20210324110900084](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324110900084.png)

### 条款53：不要轻忽编译器的警告

严肃对待编译器发出的警告信息。努力在你的编译器的最高警告级别下争取“无任何警告”的荣誉

不要过渡依赖编译器的报警能力，不同编译器对待事物的态度不同，一旦移植到另一个编译器上，你原本依赖的警告信息有可能消失

### 条款54：让自己熟悉包括TR1在内的标准程序库

### C++98列入的C++标准程序库的主要内容

STL（标准模板库）：覆盖容器（vector、string、map），迭代器、算法（find、sort、transform）、函数对象（less、greater）、容器适配器（stack、priority_queue）、函数对象适配器

IOstreams：cin、cout、cerr、clog...

国际化支持：多区域（国家）支持能力。wchar_t（16bits/char）、wstring等类型用于促进Unicode。

数值处理：复数模板、纯数值数组

异常阶层体系：base class exception及其派生类 logic_error和runtime_error等

C89标准程序库

### TR1内的组件库（现在已经不一定在tr1里面了）

![image-20210324111841819](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324111841819.png)

智能指针：shared_ptr、weak_ptr

function：表示任何可调用物（C++11 std::function 需要包括头文件functional）

![image-20210324112204118](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112204118.png)

返回值和参数共同决定function的类型



bind：绑定，实现函数的映射

Hash tables：unordered_multiset、unordered_set、unordered_map、unordered_multimap

正则表达式：用于字符串匹配

Tuples（变量组）：pair的升级版

array：STL化的数组

mem_fn：

![image-20210324112556685](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112556685.png)

reference_wrapper：

![image-20210324112626223](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112626223.png)

随机数生成工具：现在好像是随机数引擎和随机数分布类型

数学特殊函数：
![image-20210324112709282](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112709282.png)

C99兼容扩充：用来将C99程序库特性带进C++

Type Traits：

![image-20210324112813386](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112813386.png)

result_of：

![image-20210324112845536](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210324112845536.png)

## 条款55：让自己熟悉Boost

### Boost是一个社群，也是一个网站。致力于免费、源码开放、同僚复审的C++程序库开发。Boost在C++标准化过程中扮演深具影响力的角色

### Boost提供许多TR1组件实现品，以及其他许多程序库

