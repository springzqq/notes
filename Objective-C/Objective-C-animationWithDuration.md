#官方说明
##Description	
```objc
+ (void)animateWithDuration:(NSTimeInterval)duration  
                      delay:(NSTimeInterval)delay    
                    options:(UIViewAnimationOptions)options  
                 animations:(void (^)(void))animations 
                 completion:(void (^)(BOOL finished))completion
```
Animate changes to one or more views using the specified duration, delay, options, and completion handler.  
__动画展示视图的变化。使用持续时间、延时、选项和完成句柄作为参数。__  
This method initiates a set of animations to perform on the view. The block object in the animations parameter contains the code for animating the properties of one or more views.  
__本方法初始化了一组要在视图上展示的动画。动画参数(animations)中的块对象包含了要进行动画展示的视图属性。__  
During an animation, user interactions are temporarily disabled for the views being animated. (Prior to iOS 5, user interactions are disabled for the entire application.) If you want users to be able to interact with the views, include the
UIViewAnimationOptionAllowUserInteraction constant in the options parameter.  
__在动画过程中，用户的交互行为被暂时禁止了。如果你需要让用户在此过程中与视图交互，则需要在选项(option)参数中增加UIViewAnimationOptionAllowUserInteraction常量。__  
##Parameters	
###duration 动画持续时间
The total duration of the animations, measured in seconds. If you specify a negative value or 0, the changes are made without animating them.  
__整个动画的持续时间，以秒记。如果是负值或者0的话，变化会直接展示，不会展示动画效果。__  
###delay
The amount of time (measured in seconds) to wait before beginning the animations. Specify a value of 0 to begin the animations immediately.  
__动画开始前的等待时间(以秒记)。如果要立即开始动画，则将该值置为0。__  
###options
A mask of options indicating how you want to perform the animations. For a list of valid constants, see UIViewAnimationOptions.  
__一组掩码选项指示出如何展示动画，具体掩码参数，参见UIViewAnimationOptions。__  
```c
enum {
   //最低10位为多选项
   UIViewAnimationOptionLayoutSubviews            = 1 <<  0,
   UIViewAnimationOptionAllowUserInteraction      = 1 <<  1,
   UIViewAnimationOptionBeginFromCurrentState     = 1 <<  2,
   UIViewAnimationOptionRepeat                    = 1 <<  3,
   UIViewAnimationOptionAutoreverse               = 1 <<  4,
   UIViewAnimationOptionOverrideInheritedDuration = 1 <<  5,
   UIViewAnimationOptionOverrideInheritedCurve    = 1 <<  6,
   UIViewAnimationOptionAllowAnimatedContent      = 1 <<  7,
   UIViewAnimationOptionShowHideTransitionViews   = 1 <<  8,
   UIViewAnimationOptionOverrideInheritedOptions  = 1 <<  9,
   
   //17,18位表示，这四个选项为单选项
   UIViewAnimationOptionCurveEaseInOut            = 0 << 16,
   UIViewAnimationOptionCurveEaseIn               = 1 << 16,
   UIViewAnimationOptionCurveEaseOut              = 2 << 16,
   UIViewAnimationOptionCurveLinear               = 3 << 16,
   
   //21,22，23位表示，这8个选项为单选项
   UIViewAnimationOptionTransitionNone            = 0 << 20,
   UIViewAnimationOptionTransitionFlipFromLeft    = 1 << 20,
   UIViewAnimationOptionTransitionFlipFromRight   = 2 << 20,
   UIViewAnimationOptionTransitionCurlUp          = 3 << 20,
   UIViewAnimationOptionTransitionCurlDown        = 4 << 20,
   UIViewAnimationOptionTransitionCrossDissolve   = 5 << 20,
   UIViewAnimationOptionTransitionFlipFromTop     = 6 << 20,
   UIViewAnimationOptionTransitionFlipFromBottom  = 7 << 20,
};
typedef NSUInteger UIViewAnimationOptions;
```
前十个选项为多选项

- UIViewAnimationOptionLayoutSubviews  
Lay out subviews at commit time so that they are animated along with their parent.
Available in iOS 4.0 and later.  
布局子视图，使它们可以和父视图一同参与动画。

- UIViewAnimationOptionAllowUserInteraction  
Allow the user to interact with views while they are being animated.
Available in iOS 4.0 and later.  
允许用户在动画展示时与视图交互。

- UIViewAnimationOptionBeginFromCurrentState
Start the animation from the current setting associated with an already in-flight animation. If this key is not present, any in-flight animations are allowed to finish before the new animation is started. If another animation is not in flight, this key has no effect.
Available in iOS 4.0 and later.  
从已经在运行中的动画相关的状态设置开始运行动画。如果没有提供这个选项，那么已经在运行中的动画可以在新的动画开始前结束。如果动画开始的时候，没有其他的动画在运行，那么这个选项值没有意义。

- UIViewAnimationOptionRepeat
Repeat the animation indefinitely.
Available in iOS 4.0 and later.  
无线循环的重复动画

- UIViewAnimationOptionAutoreverse
Run the animation backwards and forwards. Must be combined with the UIViewAnimationOptionRepeat option.
Available in iOS 4.0 and later.  
正向和反向的运行动画，必须和UIViewAnimationOptionRepeat选项一起使用

- UIViewAnimationOptionOverrideInheritedDuration
Force the animation to use the original duration value specified when the animation was submitted. If this key is not present, the animation inherits the remaining duration of the in-flight animation, if any.
Available in iOS 4.0 and later.  
动画提交时强制使用原始的持续时间选项。如果没有提供该选项，动画继承了正在运行的动画的剩余持续时间。(这个怎么理解)

- UIViewAnimationOptionOverrideInheritedCurve
Force the animation to use the original curve value specified when the animation was submitted. If this key is not present, the animation inherits the curve of the in-flight animation, if any.
Available in iOS 4.0 and later.  
当动画提交时，强制使用原始的曲线值。如果没有提供该选项，则动画继承了正在运行的动画的曲线。
- UIViewAnimationOptionAllowAnimatedContent
Animate the views by changing the property values dynamically and redrawing the view. If this key is not present, the views are animated using a snapshot image.
Available in iOS 4.0 and later.
通过动态改变视图的属性并且重绘视图来运行动画。如果没有提供该选项，视图会使用快照图像来显示。

- UIViewAnimationOptionShowHideTransitionViews
When present, this key causes views to be hidden or shown (instead of removed or added) when performing a view transition. Both views must already be present in the parent view’s hierarchy when using this key. If this key is not present, the to-view in a transition is added to, and the from-view is removed from, the parent view’s list of subviews.
Available in iOS 4.0 and later.
提供这个选项时，在子视图替换过程中就不再是替换，而是隐藏，在父视图的子视图列表中，不再是增加和移除，而是并存，

- UIViewAnimationOptionOverrideInheritedOptions
The option to not inherit the animation type or any options.
Available in iOS 7.0 and later.
不继承动画类型或选项（ios7.0新增）

以下4个选项为单选项
- UIViewAnimationOptionCurveEaseInOut
An ease-in ease-out curve causes the animation to begin slowly, accelerate through the middle of its duration, and then slow again before completing.
Available in iOS 4.0 and later.
- UIViewAnimationOptionCurveEaseIn
An ease-in curve causes the animation to begin slowly, and then speed up as it progresses.
Available in iOS 4.0 and later.
- UIViewAnimationOptionCurveEaseOut
An ease-out curve causes the animation to begin quickly, and then slow as it completes.
Available in iOS 4.0 and later.

- UIViewAnimationOptionCurveLinear
A linear animation curve causes an animation to occur evenly over its duration.
Available in iOS 4.0 and later.

以下8个选项为单选项
- UIViewAnimationOptionTransitionNone
No transition is specified.
Available in iOS 4.0 and later.
- UIViewAnimationOptionTransitionFlipFromLeft
A transition that flips a view around its vertical axis from left to right. The left side of the view moves toward the front and right side toward the back.
Available in iOS 4.0 and later.
- UIViewAnimationOptionTransitionFlipFromRight
A transition that flips a view around its vertical axis from right to left. The right side of the view moves toward the front and left side toward the back.
Available in iOS 4.0 and later.
- UIViewAnimationOptionTransitionCurlUp
A transition that curls a view up from the bottom.
Available in iOS 4.0 and later.
- UIViewAnimationOptionTransitionCurlDown
A transition that curls a view down from the top.
Available in iOS 4.0 and later.

- UIViewAnimationOptionTransitionCrossDissolve
A transition that dissolves from one view to the next.
(该参数为ios5.0新增)

- UIViewAnimationOptionTransitionFlipFromTop
A transition that flips a view around its horizontal axis from top to bottom. The top side of the view moves toward the front and the bottom side toward the back.
(该参数为ios5.0新增)
- UIViewAnimationOptionTransitionFlipFromBottom
A transition that flips a view around its horizontal axis from bottom to top. The bottom side of the view moves toward the front and the top side toward the back.
(该参数为ios5.0新增)

###animations
A block object containing the changes to commit to the views. This is where you programmatically change any animatable properties of the views in your view hierarchy. This block takes no parameters and has no return value. This parameter must not be NULL.  
__一个块(block)对象，该块包含了视图的状态变化结果。在这儿，放置你希望在视图结构中 可以进行动画展示的视图属性的变化。该块既没有参数，也没有返回值。该块不能是NULL。__    
> 注意，可以进行动画展示的变化。有一些变化是本方法没有办法自动完成动画展示的。  

###completion
A block object to be executed when the animation sequence ends. This block has no return value and takes a single Boolean argument that indicates whether or not the animations actually finished before the completion handler was called. If the duration of the animation is 0, this block is performed at the beginning of the next run loop cycle. This parameter may be NULL.    
__也是一个块(block)对象。该块在整个动画序列结束时执行。该块没有返回值，只有一个BOOL型的参数，该参数表示完成句柄被调用时，动画是否顺利完成。如果动画的持续时间参数(duration)设置为0，该块会在下一个运行循环的开始调用。该块参数可以为NULL。__  
> 这一段怎么理解，不大明白

##Availability	
Available in iOS 4.0 and later.
##Declared In	
UIView.h
##Reference	
UIView
##Sample Code	
AirDrop Examples  UIImagePicker Video RecorderUnwindSegue

##参考阅读
Animations
