---
layout: post
title: "iOS开发过程中遇到的Bug处理集合"
date: 2015-03-27 15:00:07 +0800
comments: true
categories: iOS开发
---  

##iOS7 下UIScrollView 无法滑动的解决办法  

环境:Xcode6.2 iOS7 开启了AutoLayout 在Storyboard中拖入的一个UIScrollview  
接收的别人的code,在这个基础上开发了一个新的界面,然后手贱脑残的想用一下AutoLayout练练手(6和6+都出了,感觉以后使用AutoLayout会是主流),刚开始一切OK,跑起来顺畅的很,在3.5和4的屏幕上适应的都不错,后来不知道怎么调整了一下UI,结果悲剧发生了,Scrollview死活滑动不了了,导致Scrollview上的下拉刷新也不好用了.  
第一反应是AutoLayout的问题,禁用了AutoLayout,Run,一切OK,能滑动了.但是这丫的是Storyboard啊,一个ViewControll禁用课AutoLayout,整个文件都禁用.Google解决方案,结果果然受AutoLayout的影响,然后解决方案都是说要在viewdidAppear中重设contentSize,好吧,把contentSize的设定从viewdidload移到viewdidAppear中,Run,Fail... ...Fuck Google上清一水的解决方案都是这个,说好的滑动呢...  
<!--more-->
最后,纠结了1分钟,在viewdidload,viewwillapear,viewdidappear,viewlayoutsubviews中都设置一次contentSize,Run,神奇的事情发生了,Scrollview动起来了,然后一个一个的去除contentSize,最终确定,只有在viewlayoutsubviews设置才会有效,,坑爹啊.  
具体的code如下:  
{% codeblock lang:objc viewDidLayoutSubviews %}
// Called just before the view controller's view's layoutSubviews method is invoked. Subclasses can implement as necessary. The default is a nop.
- (void)viewWillLayoutSubviews{
    NSLog(@"viewWillLayoutSubviews");
    scrollview.contentSize = CGSizeMake(CGRectGetWidth(self.view.frame), CGRectGetHeight(self.view.frame)+1);
    
}
{% endcodeblock %}

坑爹啊,问题虽然解决了,但是这不科学啊,于是重新查看UIViewController的生命周期:  
```
2015-03-27 16:17:35.887 BugDemo[3195:937760] viewDidLoad
2015-03-27 16:17:35.888 BugDemo[3195:937760] viewWillAppear
2015-03-27 16:17:35.891 BugDemo[3195:937760] viewWillLayoutSubviews
2015-03-27 16:17:35.892 BugDemo[3195:937760] viewDidLayoutSubviews
2015-03-27 16:17:35.900 BugDemo[3195:937760] viewDidAppear
```  
发现生命周期的最后一个方法是viewDidAppear,只好理解为涉及了Autolayout,在viewDidAppear中设置已经晚了,可能是由于AutoLayout的某些原因,到了viewDidAppear的时候再设置contentSize属性已经不被scrllview接受或者被AutoLayout的某些机制给拦截了重设为另一个自适应的数值了.  
最后,总结,AutoLayout真是坑,界面还是乖乖的用code手写吧,这样出了问题好定位分析,解决.