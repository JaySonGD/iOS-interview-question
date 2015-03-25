##UITableView的优化

1、视图不透明
2、estimatedHeightForRowAtIndexPath:高度提前计算
3、后台取回数据的时候先计算好高度
4、从后台返回来的图片先后根据需要显示的图片大小切成合适大小的图片，每次只显示处理过大小的图片。