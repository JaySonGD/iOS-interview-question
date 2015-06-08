##CALayer
1、UIView能够显示在屏幕上，因为内部的一个层。

2、在创建UIView对象的时候，内部会自动创建一个层，当UIView要显示到屏幕上的时候，会调用drawRect方法进行绘图，并且将所有的内容都绘制在自己的层上，绘制完毕好，系统会将层拷贝到屏幕上，所以就完成了UIView的显示。


position是layer中的anchorPoint点在superLayer中的位置坐标
##CALayer常用属性
*	contents
	*	是id类型的，实际情况是如果不是CGImage，得到的图层将是空白的。

			layer.contents = (__bridge id)(image.CGImage);
*	contentsGravity
	
	*	kCAGravityCenter
	*	kCAGravityTop
	*	kCAGravityBottom
	*	kCAGravityLeft
	*	kCAGravityRight
	*	kCAGravityTopLeft
	*	kCAGravityTopRight
	*	kCAGravityBottomLeft
	*	kCAGravityBottomRight
	*	kCAGravityResize
	*	kCAGravityResizeAspect
	*	kCAGravityResizeAspectFill
				
			contentsGravity对应了view当中的contentMode属性。
			
			contentsGravity的目的是为了决定内容在图层的边界中怎么对齐
	
*	maskToBounds
	*	对应UIView的clipsToBounds，决定超出的内容是否需要显示。
	
*	contentsRect
	*	允许图层边框里面显示图层的一个子域。	和bounds、frame不同，contentsRect不是按点来计算的，它使用了单位坐标，单位坐标指定在0到1之间，是一个相对值（像素和点就是绝对值）
	* 点 -- 点是虚拟的像素，也成为逻辑像素。在标准的设备上，一个点就是一个像素，在Retina设备上，一个点等于2*2个像素。
	* 像素-- 
	* 单位--
	
			默认的contentsRect是{0，0，1，1},默认整个图层是可见的。
			
*	contentsCenter
	
	*	contentsCenter的类型是`CGRect`,定义了一个固定的边框和一个图层上
		
		可拉伸的区域。改变这个值并不会影响到图层的显示，除非这个图层的大小改变了，才能看到效果


##坐标系

`frame`其实改变的是CAL

##为什么CALayer中使用CGColorRef和CGImageRef这两种数据类型，而不使用UIColor和UIImage
1、因为CALayer是定义在QuartzCore框架中的；CGColorRef和CGImageRef是在CoreGraphics框架中的，UIColor和UIImage是定义在UIKit中的。

2、QuartzCore和CoreGraphics框架是可以跨平台使用的。在iOS 和Mac OS X都能使用，但是UIKit只能在iOS上使用。

3、 为了保证可移植性。

4、不过很多情况下，可以通过UIKit对象的特定方法，得到CoreGraphics对象。


##UIView和CALayer的选择

1、UIView多了一个事件处理的功能。也就是说，CALayer不能处理用户的触摸事件，UIView则可以

2、如果显示出来的东西要跟用户交互的话，得用UIView;如果不用的话，两个都可以。

3、CALayer的性能会高一些，因为少了事件的吹功能，更加轻量级。


##UIView的显示过程

*	当UIView需要显示的时候，内部的层会准备好一个CGContextRef图形上下文,然后调用delegate的drawlayer:incontext:方法，并且传入已经准备好的CGContextRef对象。

*	而UIView在drawlayer:incontext:方法中又会调用自己的drawRect方法。

*	平时drawRect:中通过UIGraphicsGetCurrentContext()获取的就是层传入的CGContexRef对象，完成绘制后填入层的CGContextRef中，然后拷贝至屏幕。


##Core Animation
动画的执行过程都是在后台操作的，不会阻塞主线程。  作用在Layer，并非UIView;

##CAAnimation常用属性
所有动画的父类，不能直接使用的，用子类。

*	duration:动画持续时间
*	repeatCount:动画的重复次数
*	timingFunction：控制动画的节奏
	*	kCAMediaTimingFunctionLinear （线性）：匀速的，相对静态的感觉。
	*	kCAMediaTimingFunctionEaseIn （渐进）：动画缓慢进入，加速离开。
	*	kCAMediaTimingFunctionEaseOut（渐出）: 动画全速进入，减速到达
	*	kCAMediaTimingFunctionEaseInEaseOut （渐进渐出）：缓慢进入，中间加速，然后减速到达。
	*	kCAMediaTimingFunctionDefault  （默认的）
*	delegate:动画代理。
	
		- (void)animationDidStart:(CAAnimation *)anim;

		- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag;
		
##CAPropertyAnimation常用属性
CAPropertyAnimation是CAAnimation的子类，但是不能直接使用，要想创建动画对象，应该使用它的两个子类：`CABasicAnimation`和`CAKeyframeAnimation`

*	keyPath属性，指定某个属性名为keyPath，并且对这个属性的值进行修改，达到动画的效果。
