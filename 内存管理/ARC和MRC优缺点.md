arc 与 mrc的优缺点

优点：

*	1、ARC省去了release autorelease，自动帮我们添加，减少手动管理内存出现的问题。

*	2、代码效率提高了，由于使用了编译器管理计数

*	3、不用重载dealloc了

缺点：

*	Core Foundation中的malloc()或者free()等，ARC就鞭长莫及了，这时候还是需要自己手动进行内存管理。在之后我们会看到一些这方面的例子。