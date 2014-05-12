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
###animations
A block object containing the changes to commit to the views. This is where you programmatically change any animatable properties of the views in your view hierarchy. This block takes no parameters and has no return value. This parameter must not be NULL.
__一个块(block)对象，该块包含了视图的状态变化结果。在这儿，放置你希望在视图结构中 可以进行动画展示的视图属性的变化。该块既没有参数，也没有返回值。该块不能是NULL。__  
> 注意，可以进行动画展示的变化。有一些变化是本方法没有办法自动完成动画展示的。  

###completion
A block object to be executed when the animation sequence ends. This block has no return value and takes a single Boolean argument that indicates whether or not the animations actually finished before the completion handler was called. If the duration of the animation is 0, this block is performed at the beginning of the next run loop cycle. This parameter may be NULL.  
__也是一个块(block)对象。该块在整个动画序列结束时执行。该块没有返回值，只有一个BOOL型的参数，该参数表示
##Availability	
Available in iOS 4.0 and later.
##Declared In	
UIView.h
##Reference	
UIView
##Sample Code	
AirDrop ExamplesUIImagePicker Video RecorderUnwindSegue
