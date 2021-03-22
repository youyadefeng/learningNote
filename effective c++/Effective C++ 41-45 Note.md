# Effective C++ 41-45 Note

## 条款41：了解隐式接口和编译器多态

###　Templates及泛型编程的领域中，隐式接口和编译期多态比较重要。

![image-20210322231545096](https://yydf-1305206966.cos.ap-nanjing.myqcloud.com/image-20210322231545096.png)

隐式接口：w必须支持的接口由template中执行于w身上的操作决定：size、normalize、swap、copy构造函数、!=

编译期多态：凡涉及ｗ的任何函数调用，都可能造成template的具现化。不同template参数会导致调用不同的函数



