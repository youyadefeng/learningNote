# Effective C++ 21-25 Note

## 条款21：必须返回对象时，别妄想返回其reference

### 在看到一个reference表达式的时候，应该立即问自己：它的另一个名称是什么？

### 绝不要返回pointer或reference指向一个local stack对象

local stack对象就是在函数中不用new得到的局部变量，这个局部变量会在退出函数时自动销毁。返回的指针或引用会指向一个不存在的对象。

### 绝不要返回以一个reference指向一个heap-allocated对象

heap-allocated对象就是new出来的对象，new出来的对象需要delete。而有时候对返回的引用做delete操作十分困难：

![image-20210314111655786](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314111655786.png)

![image-20210314111710349](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314111710349.png)

### 绝不要返回pointer或reference指向一个local static对象

因为每次调用该函数的时候，返回的都是同一个值。

![image-20210314112021852](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314112021852.png)

![image-20210314112036654](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314112036654.png)

![image-20210314112120311](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314112120311.png)

综上所述，还是返回一个对象吧，不要返回引用了

## 条款22：将成员变量声明为private

### 将成员变量声明为private，这样可以赋予客户访问数据的一致性、可细微划分访问控制、允诺约束条件获得保证，提供class作者以充分的实现弹性

**访问数据的一致性**：如果public中有些成员是函数，有些成员是变量。客户在使用时就会搞不清楚什么时候加调用符，什么时候不加。如果所有成员都是函数，就全部都加访问调用符

**细微划分访问控制：**如果将所有变量声明为public，那每个人都可以读写它。如果将变量声明为private，可以通过setter和getter函数控制访问权限为：“可读可写”、“只读不写”、“不读只写”、“不读不写”

**给予class作者充分的实现弹性：**将变量声明为private之后，就实现了封装。封装隐藏了具体功能的实现，当我们修改底层代码时，用户其实毫不知情。

### protected并不比public更具封装性

![image-20210314113238871](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314113238871.png)

只有private提供封装，其它不提供封装

### 条款23：宁以 non-member、non-friend 替换 member 函数

**提高封装性**：如果某些东西被封装，它就不再可见。愈多东西被封装，越少人能够看见，我们能够有更大的弹性去修改。这会使我们能够改变事物而只影响有限客户。member成员能够访问private数据，而另外两者不能，所以另外两者可以提高封装性。

**提高包裹弹性：**这些函数既可以是普通函数，也可以是另一个类的member函数。只要满足non-member、non-friend的条件即可。

**提高机能扩充性：**C++的处理方法是将这些函数与类放在同一个命名空间，而命名空间可以跨越多个源码文件。如果想要增加新的机能，只需要在新文件中打开命名空间然后加上新的函数即可。

![image-20210314115003936](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314115003936.png)

上面就运用了这种方法，三个函数放在三个不同的文件中，但都处于同一命名空间。

![image-20210314115106368](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210314115106368.png)

## 条款24：若所有参数皆需类型转换，请为此采用non-member函数

只有当参数被列于参数列内，这个参数才是隐式类型转换的合格参与者

如果采用成员函数：

![image-20210320214514375](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320214514375.png)

![image-20210320214525531](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320214525531.png)

oneHalf重载了*，而2则没有重载。当2位于参数列表时，它会隐式转换为oneHalf类型，而反过来则不行

## 条款25：考虑写出一个不抛异常的swap函数

### 当std::swap对自定义类型的效率不高时，提供一个swap成员函数，并确定这个函数不抛出异常

当出现“以指针指向一个对象，内含真正数据”（pimpl：point to implementation）的类型时，系统提供的swap函数就不够高效了。因为它还是会去复制这个对象的值，然后进行交换。而更高效的办法是交换两个对象的指针：

![image-20210320222833529](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320222833529.png)

### 如果你提供一个member swap，也该提供一个non-member swap用来调用前者。对于classes，也请特化std::swap

![image-20210320223056123](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320223056123.png)

在自定义类中加入特定的swap函数，在命名空间std中对swap函数进行指定类型的特化

对于模板类，在类中加入特定的swap函数，在相同命名空间里再定义一个非成员版本的swap函数

![image-20210320223754441](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320223754441.png)

### 调用swap时应针对std::swap使用using声明式，然后调用swap并且不带命名空间资格修饰

![image-20210320223859165](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320223859165.png)

![image-20210320223925185](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320223925185.png)

### 对“用户自定义类型”进行std template全特化是好的，但千万不要尝试在std内加入某些对std而言全新的东西

![image-20210320224145314](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210320224145314.png)