在现实世界中，人们在工作中处理某些事情的时候，常常需要严格遵守某些流程。比方说，执法人员在进行例行询问和收集证据的时候就需要“遵守协议”。

而在面向对象的世界里，能够为对象在特定的某种情况下定义一组特定行为也是很重要的。比如，桌面视图(table view)要能够与数据源对象通信以便知道自己需要显示什么。这就意味着数据源应该能够对一组桌面视图可能发送的消息做出反应。

数据源可能是任意类的实例，比方说视图控制器(view controller, NSVviewController的子类)或者一个可能从NSObject继承而来的专用的数据源类。为了使桌面视图知道一个对象是否能够作为数据源，很重要的一点就是，要能够声明该对象实现了某些必要的方法。

OBJ-C提供了“协议”(protocol)的定义。定义协议将声明一组方法。这组方法将用于某些特定情况下。本章介绍了定义协议的语法，以及当类接口实现了协议规定的方法时，如何标记该类接口遵循该协议，

#协议定义了消息传递的契约
类接口声明了和类相关的方法和属性。相应的，协议用来声明和类无关的方法和属性。

定义协议的基本语法如下：
```objc
@protocol ProtocolName
//list of methods and properties
@end
```
协议可以定义类方法、实例方法、类属性、实例属性。如，一个用来显示饼图(pie chart)的用户视图类。

为了使该视图尽可能的被复用，所有有关信息的处理应该交给另一个对象--数据源--来处理。这样同一个视图类的多个实例通过与不同的数据源通信就可以显示不同的信息。

饼图视图所需要的最基本的信息包括：区域个数、每个区域的大小，每个区域的标题。因此，饼图的数据源协议应该是这样：

```objc
@protocol XYZPieChartViewDataSource
- (NSUInteger)numberOfSegments;
- (CGFloat)sizeofSegmentAtIndex:(NSUInteger)segmentIndex;
- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
@end
```
> 注意：该协议使用了NSUInteger类来表示无符号整数值，在下一章节中会对该类进行详细讨论。

