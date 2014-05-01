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
上述例子定义了三个必要方法和两个可选方法。

##在运行时检查可选方法的实现
如果一个方法在协议中定义为可选的，那么在尝试调用它之前*必须*检查对象是否实现了该方法。比方说，饼图视图必须测试区域标题方法：
```objc
NSString *thisSegmentTitle;
if ([self.dataSource respondsToSelector:@selector(titleForSegmentAtIndex:)])
{
    thisSegmentTitle = [self.dataSource titleForSegmentAtIndex:index];
}
```
respondsToSelector方法使用了一个选择器(selector)，该选择器引用了方法的标识。使用@selector()指令并指定方法名来提供正确的标识。

该例子中，如果数据源实现了titleForSegmentAtIndex方法，则thisSegmentTitle取到标题，否则thisSegmentTitle仍为nil。
>注意：本地对象变量会自动初始化为nil。

实际上，如果你试图对一个遵守上文中定义协议的id调用上述respondsToSelector:方法时, 编译器会报错。因为没有已知的实例方法。一旦你确定一个遵守某协议的实例方法，会重新进行静态类型检查(static type-checking)。调用任何协议中没有定义的方法都会出错。避免编译错误的方法是设置用户协议继承自NSObject协议.

##协议的继承
就和类能从超类(superclass)中继承一样，协议也可以继承自其他协议。

比如，让你自定义的协议继承自NSObject协议就是一个很好的工程实践(NSObject中的一部分行为从类接口中分离出来形成了一个独立的协议，NSObject类采用了NSObject协议)。

通过指明你自己的协议遵守NSObject协议，实际上要求所有采用用户协议的对象都必须实现NSObject协议的方法。但是通常我们自己的类都是NSObject的子类，所以我们一般不需要再去另行实现这些方法(因为NSObject类已经实现了啊)。协议的继承是非常有用的。就像上述情况一样。

指定协议的继承语法如下，用尖括号将要继承的上层协议括起来：
```objc
@protocol MyProtocol <NSObject>
...
@end
```
在这个例子中，所有使用MyProtocol协议的对象都能有效的使用声明在NSObject协议中的方法。

#遵守协议
指明一个类是遵守某个协议的，采用如下方式，也是用尖括号将协议括起来：
```objc
@interface MyClass: NSObject <MyProtocol>
...
@end
```
这意味着MyClass的所有实例不仅能够使用接口中定义的方法，还能使用MyProtocol中定义的必要方法。在类声明中，不需要再重复声明接口中声明过的方法。

如果类需要使用多个协议，那么将它们仿佛一个逗号列表中，如下：
```objc
@interface MyClass : NSObject <MyProtocol, AnotherProtocol, YetAnotherProtocol>
...
@end
```
>tips: 如果你发现你的类使用了一大堆协议，恐怕这意味你把类设计得过于复杂，应该对其重构。将复杂的类划分成多个小一些的类。每个类都有更加清晰的方法。对OSX和IOS开发新手来说，有一个相对普遍的陷阱就是，使用单个应用程序委托类来包含大多数的程序功能(管理相关数据结构，将数据提供给多个用户接口元素，对手势和其他用户接口做出反应)。随着复杂度的增加，类越来越难以维护。

一旦你指明类遵守某一协议，类至少要提供协议要求必须提供的方法。并根据你自己的需要选择性的实现可选方法。否则编译器会给出警告。
>注意：协议中的方法声明和其他声明一样，实现中的方法名和参数类型必须和协议中的声明一致。

##Cocoa和Cocoa Touch 定义了大量的协议
Cocoa和Cocoa Touch对象在各种不同情况下使用了大量的协议。比方说，桌面视图类(OSX下的NSTableView和IOS下的UITableView)使用了数据源对象来为自己提供必要的信息。他们定义了自己的数据源协议，使用方法和我们之前例子中的XYZPieChartViewDataSource差不多。这些桌面视图类也允许你设置一个委托对象，这个对象也必须遵守相应的NSTableViewDelegate或者UITableViewDelegate协议。“委托”用来处理用户的交互行为或者个性化显示。

某些协议用来指明类之间的非结构化相似性。协议倾向于满足会被许多毫不相关的类所采用的更普遍的Cocoa或CocoaTouch通信机制，而不是满足特定的类需求。

比方说，许多框架模型对象(例如像NSArray和NSDictiionary等集合类(collection class))支持NSCoding协议，这意味着他们可以对他们的属性以元数据(raw data)的方式进行编码或解码以归档或分发。NSCoding使得将整个对象图写入磁盘变得相对简单，让图中的每个对象采用了协议。

一些OBJ-C 语言级的特性也依赖于协议。为了能快速枚举，比如说一个集合必须采用(adopt)NSFastEnumeration协议，正如“快速枚举是枚举集合变得更容易”一节提到的那样。除此之外，许多对象可以被复制，如“复制属性维护他们的副本”一节中描述的，使用一个有副本性质的属性。你试图复制的任何对象都必须采用NSCopying协议，否则会发生运行时错误。

#为匿名而使用的协议
协议还用于这种情况：对象的类型是不知道的，或者是需要被隐藏的。

比方说，框架的开发人员
