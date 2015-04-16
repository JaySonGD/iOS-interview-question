##64bit适配

####1、注意`sizeof`,结果随编译器
	
数据类型		  |  	32-bit	  | 	64-bit
------------  | -------------| ------------
int			  | 	 4  	  | 	4
long		  | 	 4  	  | 	8
float		  | 	 4  	  | 	4
long		  | 	 8  	  | 	8

用下面的代替

*	int 		-> NSInteger
*	unsigned 	-> NSUinteger
*	float		-> CGFloat
*	动画时间		-> NSTimeInterval

####2、枚举使用OC自带的

```
typedef NS_ENUM(NSUInteger, PersonType) {
   		Man,
  	  Women,
};
```

####3、替代Format字符串

```
	NSArray *temp = @[@"A",@"B",@"C"];
    NSLog(@"数组元素 %lu",(unsigned long)temp.count);
```

用字面量语法代替解决`@`

```
	NSArray *temp = @[@"A",@"B",@"C"];
    NSLog(@"数组元素 %@",@(temp.count));
```

####4、BOOL
32-bit： BOOL定义成为signed char @encode(BOOL)结果是'c'
64-bit:  BOOL定义成为bool,@encode(BOOL)结果是'B'

####5、第三方库在target加上arm64编译

####6、编写代码需要注意的地方

*	长整型long不能赋值给int
*	指针类型不能赋值给int
*	

####影响
1、增加内存压力

2、计算可能不同结果

3、把大数据类型拷贝到小的数据类型可能被截断


