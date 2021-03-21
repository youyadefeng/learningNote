# Effective C++ 36-40 Note

## 条款36：绝不重新定义继承而来的non-virtual函数

![image-20210321230315314](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321230315314.png)

如果D重新定义函数mf：

![image-20210321230341698](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210321230341698.png)

非虚函数都是静态绑定的，指针是什么类型就调用什么类型。如果派生类重新定义了非虚函数，就会发生遮掩，导致派生类和基类调用的函数版本不一样！

