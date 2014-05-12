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
###duration
The total duration of the animations, measured in seconds. If you specify a negative value or 0, the changes are made without animating them.
###delay
The amount of time (measured in seconds) to wait before beginning the animations. Specify a value of 0 to begin the animations immediately.
###options
A mask of options indicating how you want to perform the animations. For a list of valid constants, see UIViewAnimationOptions.
###animations
A block object containing the changes to commit to the views. This is where you programmatically change any animatable properties of the views in your view hierarchy. This block takes no parameters and has no return value. This parameter must not be NULL.
###completion
A block object to be executed when the animation sequence ends. This block has no return value and takes a single Boolean argument that indicates whether or not the animations actually finished before the completion handler was called. If the duration of the animation is 0, this block is performed at the beginning of the next run loop cycle. This parameter may be NULL.
##Availability	
Available in iOS 4.0 and later.
##Declared In	
UIView.h
##Reference	
UIView
##Sample Code	
AirDrop ExamplesUIImagePicker Video RecorderUnwindSegue
