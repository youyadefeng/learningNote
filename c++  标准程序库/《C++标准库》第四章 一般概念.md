# 《C++标准库》第4章 一般概念

### 头文件

引入c标准头文件时，采用前缀字符c，不再采用扩展名.h

![image-20210328085743306](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328085743306.png)

为了向后兼容于C，旧式的C标准头文件仍然有效

![image-20210328085848199](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328085848199.png)

### 差错与异常处理

![image-20210328085911575](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328085911575.png)

#### 针对语言支持而设计的异常类

![image-20210328090005195](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090005195.png)

#### 针对逻辑差错而设计的异常类

![image-20210328090103047](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090103047.png)

#### 针对运行期差错而设计的异常类

![image-20210328090159330](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090159330.png)

![image-20210328090208719](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090208719.png)

#### 异常类的头文件

![image-20210328090227399](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090227399.png)

### 以Class exception_ptr传递异常

C++11以后，可以将异常存储在exception_ptr对象中，稍后再在其他情景下使用

![image-20210328090414871](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090414871.png)

![image-20210328090459342](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090459342.png)

### 可被调用对象

当使用bind绑定类成员函数时，需要附带而外的对象指针，表示调用的是哪个对象的成员函数

![image-20210328090633421](C:\Users\94375\AppData\Roaming\Typora\typora-user-images\image-20210328090633421.png)

如果想声明可调用对象，可以使用class std::function<>（返回值和函数参数作为模板）





