因为自己在做iOS内购接口封装的时候用到了Block的回调，一位游戏GG郁闷了好几天，之前回调一直都是使用代理的，突然间来了一个奇怪的Block语法，还特意过来一起讨论如何调用的问题。所以今晚就顺便总结一下Block吧。

##谈谈眼中的Block



###Block是什么？
简单一点来说就是一个函数指针，把一个代码片段作为函数参数传递，可以在另外一个函数体内实现回调机制。就像下面的例子
    
    //定义
    - (void)run:(void (^)(int second)) completeBlock
	{
    	int second = 5;
    	completeBlock(second);
	}

	//调用
	[self run:^(int second) {
        
        NSLog(@"Second is %d",second);
    }];

其中`void (^)(int second)`就是一个Block，这个Block的返回值类型是`void`，参数是`second`，然后我们就可以在这个Block里面使用`second`这个参数了。

如果还不明白的话，我就再举个例子。小明的妈妈要小明去操场跑步，跑完后回来告诉妈妈跑步的时间（这里默认是5秒），去操场跑步就是`run函数`，回来告诉妈妈跑步时间（回调）就是在执行Block.

###Block的语法
1、基本用法：`返回值类型` + `(^名称)` + `参数`

2、使用`typedef`对Block进行别名,增加易读性
   
    typedef void(^CompleteBlock)(int second);

这样我们就可以直接使用`CompleteBlock`进行传递了

    - (void)run:(CompleteBlock) completeBlock
	{
    	int second = 5;
    	completeBlock(second);
	}
	
###Block功能
1、`变量的截取`。如果Block里面引用了变量，会给这些变量一个瞬时值，在执行Block之前即使改变了变量的值，也不会影响最初在Block里面引用变量的瞬时值。
   
    int x = 520;
    int y = 1314;
    
    void(^LoveBlock)(void) = ^{
        
        NSLog(@"LoveBlock :x=%d,y=%d",x,y);
    };
    
    x = 0;
    y = 0;
    NSLog(@"change after x=%d,y=%d",x,y);
    LoveBlock();
    
    //输出结果
    2015-03-11 00:05:37.736 CustomControl[1547:66112] change after x=0,y=0 	2015-03-11 00:05:37.737 CustomControl[1547:66112] LoveBlock :x=520,y=1314  

2、`__block`修饰符。既然Block能保存变量的瞬时值，那就应该有方法控制他不保存Block的瞬时值。

     __block int x = 520;
     __block int y = 1314;
    
    void(^LoveBlock)(void) = ^{
        
        NSLog(@"LoveBlock :x=%d,y=%d",x,y);
    };
    
    x = 0;
    y = 0;
    NSLog(@"change after x=%d,y=%d",x,y);
    LoveBlock();


	//输出结果
	2015-03-11 00:10:03.368 CustomControl[1582:67992] change after x=0,y=0 	2015-03-11 00:10:03.369 CustomControl[1582:67992] LoveBlock :x=0,y=0


###Block使用注意事项
1、不能在block里面对未加__block修饰符的对象进行赋值