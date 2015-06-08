##1、Notification调用的时机

问题：注册通知的时候一定要在`viewDidApper`,移除一定要在`viewDidDisAppear`吗？

不一定，因为viewDidApper和viewDidDisAppear并不一定是成对出现的，通过手势操作viewController可以让didAppear调用无数次

所以正确的做法是，每次注册通知前先移除，重复移除是安全的。