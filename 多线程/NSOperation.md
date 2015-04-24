##NSOperation

NSOperation本身就是支持多核心的，可以在多线程中安全调用一个`NSOperaton`对象并且不用额外创建同步锁去访问资源。这是非常又必要的，因为operation默认运行在不同的线程中。

如果子类化`NSOperation`，必须保证覆盖的方法在多线程中仍然是安全调用的，如果增加了自定义的方法，例如一些数据的访问，也得保证这些方法是线程安全的。访问变量数据在operation中必须是同步的防止保护的数据被破坏。


##Asynchrons VS Synchrons 
如果手动的执行一个operation，并不添加到queue，则可以把operation设置成为同步或者异步的。


`Operation 对象默认是同步的`,在同步的operation中，并没又单独创建一个线程，当你调用`start`方法的时候，同步的operation直接在当前的线程中执行。直到operation返回的时候，这个任务就执行完成了。


当你调用`start`方法在异步的operation中，这个方法可能会在对应的任务完成前返回。

一个异步operation对象负责的是单独线程的调度任务。


##Override

*	在一个非并发的operation中需要覆盖	`main`方法
*	创建一个并发的operation中，需要覆盖以下方法
	*	start
	*	asynchronous
	*	executing
	*	finished
	
##States
Operation对象包含了状态信息决定了安全调用的时机在Operation的生命周期当中。

*	isRead
*	isExecuting
*	isFinished
*	isCancelled

##响应取消的命令
把operation对象添加到一个queue中，就脱离了控制。不过可以决定不执行后面所有的operation，调用`cancel`方法或者在`NSOperationQueue`忠调用`cancelAllOperations`方法。

取消并不是立马强制停止工作，在NSOperation默认实现中，包含了取消状态的检查。例如在start调用前取消了operation，则start方法永远不会调用




