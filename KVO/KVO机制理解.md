1、NSNumber优化在64bit上
2、runtime(对象模型、消息机制、Category实现、)
3、KVO实现;
3、block中的打印 和 GCD、多线程优缺点
5、NSRunloop
6、内存管理（autorelease实现）


###KVO的实现机制
*	观察一个对象时，一个新的类会动态被创建。这个类继承自该对象原本的类，并重写了被观察属性的setter方法。重写的setter方法会在调用原setter方法之前和之后,通知所有观察对象值的更改，最后把这个对象的isa指针指向这个新创建的子类，对象就变成了新创建子类的实例了。

这个中间类，继承自原本的那个类，还重写了class方法，企图欺骗我们这个类没有改变，就是那个类。

###KVO缺陷

*	1、只能通过重写`-boserveValueForKeyPath:ofObject:change:context:`方法来获得通知。
	想要提供自定义的selector,或者传一个block，门都没有

*	2、处理父类的情况，如果父类同样监听同一个对象的属性。有时候，不知道父类是不是对这个消息有兴趣。虽然`context`就是干这个的，也可以解决这个问题。在`addObserver:forKeyPath:options:context：`传进去一个父类不知道的context就可以。

*	3、用delegate，或者NotificationCenter比较常用

####自己实现KVO

*	检查对象的类有没有响应的setter方法，没有的话就跑出异常
*	检查对象的isa指向的类是不是一个kvo类，如果不是，新建一个继承原来类的子类，并把isa指针指向这个新建的子类
*	检查对象的KVO类有没有重写过这个setter方法。如果没有，添加重写的setter方法
*	添加这个观察者
*	需要注意线程安全的问题。
*	

ARC的优缺点
1、Core Foundation中的malloc()或者free()等，ARC就鞭长莫及了，这时候还是需要自己手动进行内存管理。在之后我们会看到一些这方面的例子。

需要注意的地方：

ARC确实可以在适当的地方为代码添加retain或者release，但是这并不意味着你可以完全忘记内存管理，因为你必须在合适的地方把strong指针手动设置到nil