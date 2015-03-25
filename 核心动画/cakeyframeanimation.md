##CAKeyFrameAnimation

*	value属性
	*	指明整个动画过程中关键帧点，起点必须作为values的第一个值。如果写一个闭合的图形，有两种方法可以参考：
		
			1、最后一个value写原点
			2、全部点设置完后调用closePath方法，就能闭合。
			
* path属性
	
	*	与values属性一样，指定整个动画所经过的路径。需要注意的是values属性和path是互斥额,当values和path同时指定时候，path会覆盖values，
	
* keyTimes属性
	*指定每个子路径的时间。如果没有显示地对keyTimes进行设置，系统会默认每条子路径的时间为：`t = duration / (values.count - 1)`
	
* timeFunctions属性
	
	*	指定时间函数，具体类型查看文档

*	calculationMode属性
	*	该属性决定了物体在每个子路径下是跳着走还是匀速走
		
		*	kCAAnimationLinear
		*	kCAAnimationDiscrete
		*	kCAAnimationPaced
		*	kCAAnimationCubic
		*	kCAAnimationCubicPaced
		
*	与`CABaseAnimation`区别，CABaseAnimation只定义了开始和结束的两种状态，而CAKeyFrameAnimation可以定义开始和结束两种状态的中间状态。