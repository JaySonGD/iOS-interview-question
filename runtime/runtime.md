
补充：[浅析iOS高级编程-runtime](http://www.liuchendi.com/2014/08/26/iOS/3_runtime/)
##Runtime

1、每个类都有一个实例，isa指针就指向这个实例所属的类，每个类也是一个对象，类也有isa指针

2、类也是一个对象，这个对象是另外一个类的实例，这个类是元类

3、每个元类也是一个对象，这个对象指向根元类。

4、根元类的isa指针指向自己，形成了一个闭环。



##SEL
SEL只是一个指向方法的指针，准确说是一个根据方法名hash化了的key值，能唯一代表一个方法，存在只是为了加快方法的查询速度。

##IMP
是一个函数指针。可以通过SEL快速、准确的获得对应的IMP，获取到了函数指针，就或得了执行函数的如可，

	
	NSArray *array = [[NSArray alloc] init];
	
1、代码执行的过程是`[NSArray alloc]` 先被执行，因为NSArray没有这个alloc方法，所以会去父类NSObject查找

2、检测NSObject是否响应`allco`方法，发现响应，所以检查NSArray类，并根据其所需要的内存空间大小开始分配空间，然后把isa指针指向NSArray类。同时,alloc也被加进cache列表里面。

3、接着，执行init方法，如果NSArray响应该方法，直接加入cache，不响应则去父类查找。

4、后期操作中如果再调用[[NSArray alloc] init] 这种方式来创建数组，则会直接从Cache中读取相应的方法，直接调用。

SEL就是为了查找方法最终实现IMP

##类相关操作函数
类名操作的函数主要有：

*	类名

		const char *class_getName//获取类名

*	父类（super_class）和元类(meta-class)

		Class class_getSuperclass(Class cls)//获取父类
		BOOL class_isMetaClass		//判断是否是一个元类

*	获取实例变量的大小

		size_t class_getInstanceSize(Class cls)
		
*	成员变量(ivars)及属性
	
		
##方法与消息
两个类里面只要方法名字相同，那么方法的SEL就是一样的。每一个方法都对应着一个SEL，所以在同一个类中不存在2个同名的方法，即使参数类型不同也不行

3种方法获取SEL
1、sel_registerName函数
2、@selector()
3、NSSelectorFromString()方法
		
		
##动态方法解析
1、接收到未知的消息时，调用所属类的方法`resolveInstanceMethod`(实例方法)或者`resolveClassMethod`(类方法)
2、如果上一步无法处理消息，则会继续调用`- (id)forwardingTargetForSelector:(SEL)aSelector`方法
3、如果上一步不能处理未知消息，唯一能做的是启用完整的消息转发机制。调用`- (void)forwardInvocation:(NSInvocation *)anInvocation`

###Swizzling注意的方法总结

*	1、Swizzling应该总是在+load中执行

	*	在Objective-C中，运行时会自动调用每个类的两个方法。+load会在类初始加载时调用，+initialize会在第一次调用类的类方法或实例方法之前被调用。这两个方法是可选的，且只有在实现了它们时才会被调用。由于method swizzling会影响到类的全局状态，因此要尽量避免在并发处理中出现竞争的情况。+load能保证在类的初始化过程中被加载，并保证这种改变应用级别的行为的一致性。相比之下，+initialize在其执行时不提供这种保证—事实上，如果在应用中没为给这个类发送消息，则它可能永远不会被调用。

*	2、Swizzling应该总是在dispatch_once中执行
	*	与上面相同，因为swizzling会改变全局状态，所以我们需要在运行时采取一些预防措施。原子性就是这样一种措施，它确保代码只被执行一次，不管有多少个线程。GCD的dispatch_once可以确保这种行为，我们应该将其作为method swizzling的最佳实践。
	
	
	
	###super 
	
	self是类的一个隐藏参数，super是编译器标示符

	用`super`接收消息的时候，编译器会生成一个objc_super结构体；
	
			struct objc_super { id receiver; Class superClass; };
			
	发送消息的时候，会调用`objc_msgSendSuper`,第一个参数即为前面生成的objc_super结构体.这个函数的功能是从objc_super结构体指向的superClass方法列表里面寻找selector,找到以后通过objc->receiver去调用这个selector.
	
			id objc_msgSendSuper ( struct objc_super *super, SEL op, ... );
			
	
	所以，下面两个例子输出的结果的是自身使用的类
	 
	 		 NSLog(@"self class: %@", self.class);
    		NSLog(@"super class: %@", super.class);
			
	###库相关的操作
			NSLog(@"获取指定类所在动态库");

		NSLog(@"UIView's Framework: %s", class_getImageName(NSClassFromString(@"UIView")));

		NSLog(@"获取指定库或框架中所有类的类名");
		const char ** classes = objc_copyClassNamesForImage(class_getImageName(NSClassFromString(@"UIView")), &outCount);
		for (int i = 0; i < outCount; i++) {
    	NSLog(@"class name: %s", classes[i]);
		}
	
	
###load方法的解释
1、一个类的load方法会在其所有父类的load方法之后调用

2、分类的load方法会在对应类的load方法之后调用。


###Category实现

####1、Category使用场景：把类的实现分开在不同的文件里面。
 
 *	减少单个文件的体积
 *	把不同的功能组织到不同的category里
 *  由多个开发者共同完成一个类
 *  按需加载想要的category

####2、声明私有方法。
####3、衍生出其他几个使用场景:模拟多继承、把framework的私有方法公开。


####4、category和extension
extension是一个匿名的category，但是extension和有名字的category完全是两个东西

extension在编译期决定，是类的一部分。一般用来隐藏类的私有信息，必须有一个类的源码才能为一个类添加extension。

category则完全不一样，它是运行期决议的。

区别：extension能够添加实例变量，但是category是无法添加实例变量（运行期，对象的内存布局已经确定，如果添加实例变量会坡缓类的内部布局）。

同时在objc_class里面可以看见，方法列表的定义是一个指针的指针，通过修改这个指针的值，能够动态为某一个类增加成员方法，这也是Category的实现原理，也说明了Category只可为对象增加成员方法，不能增加成员变量。




	
