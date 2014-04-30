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

饼图类的接口需要一个属性来跟踪数据源对象。这个对象可以是任何类，所以该属性的基本类型应该是id，其他唯一需要知道的是该属性要遵守相关的数据源协议。

因此，在饼图类中声明数据源属性的语法应该是这样：
```objc
@interface XYPPieChartView : UIView
@property (weak) id <XYZPieChartViewDataSource> dataSource;
...
```
OBJ-C中，使用尖括号表示遵守某一协议。上述例子声明了一个遵守XYZPieChartViewDataSource协议的泛型对象指针的弱属性。
>注意：因为之前讨论过的，出于对象图管理的原因，委托和数据源属性通常标记为弱(weak)。参见“避免强引用循环”。

通过为属性规定其应该遵守的协议，即使属性的类型为泛型，当编译器检测到你试图将不符合该协议的对象设置为该属性时，也会产生警告(warning)。对象是UIViewController的实例还是NSObject的实例都无关紧要。只要对象能符合协议，也就是说，饼图类知道自己能得到自己想要的信息。

#协议可以有可选方法
默认情况下，协议中声明的所有方法都为必要方法。也就是说遵守协议的类需要实现协议中规定的所有方法。

但是，也可以在协议中规定一些可选方法。类可以根据自己的需要选择是否实现他们。

比方说，饼图是否显示标题就可以是可选的。如果数据源没有实现方法titleForSegmentAtIndex:，视图上就不会显示标题。

通过在协议方法前使用@optional指令来指明可选方法。如下：
```objc
@protocol XYZPieChartViewDataSource
- (NSUInteger)numberOfSegments;
- (CGFloat)sizeOfSegmentAtIndex:(NSUInteger)segmentIndex;
@optional
- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
@end
```
上述情况下只有titleForSegmentAtIndex这一方法是可选的，之前的方法没有设置指令，因此是必要的。

@optional指令对之后所有的方法起作用，直到到达协议定义结尾或者出现了另一个指令(如，required)。所以我们可以继续为协议添加更多的方法，如下：
```objc
@protocol XYZPieChartViewDataSource
- (NSUInteger)numberOfSegments;
- (CGFloat)sizeOfSegmentAtIndex:(NSUInteger)segmentIndex;
@optional
- (NSString *)titleForSegmentAtIndex:(NSUInteger)segmentIndex;
- (BOOL)shouldExplodeSegmentAtIndex:(NSUInteger)segmentIndex;
@required
- (UIColor *)colorForSegmentAtIndex:(NSUInteger)segmentIndex;
@end
```

