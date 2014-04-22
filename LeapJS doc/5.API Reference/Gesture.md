# Gesture[](\#id34 "Permalink to this headline")

##_class _Gesture()[](#Gesture "Permalink to this definition")

`Gesture`类包含了用户发起的可识别的动作数据.

Leap 会观察其观测范围内的运动是否符合某个手势动作. 比如, 用手从一个边缘滑动到另一个边缘就是一个滑动(swipe)操作,用手指向前点击屏幕就是一个屏幕点击(screen tap)动作.

当Leap辨识出某一个手势以后, 它就会为这个手势分配一个ID并且在帧手势列表加上这个`Gusture`对象.对于一个连续性的手势,即是会在很多帧中连续发生的手势, Leap 会不断向每个后续的帧加入一个有相同ID的`Gesture`对象,并且根据追踪到的数据更新其各项属性.

**重要提示 :** 必须启用识别手势这个功能否则 **任何手势都识别不到**.

`Gesture`的子类定义了特殊动作模式的属性来让Leap Motion控制器方便识别手势\

`Gesture` 子类包括:

*   [CircleGesture()](Leap.CircleGesture.html#CircleGesture "CircleGesture") – 手指的圈圈动作.
*   [SwipeGesture()](Leap.SwipeGesture.html#SwipeGesture "SwipeGesture") – 用手做直线运动.
*   [ScreenTapGesture()](Leap.ScreenTapGesture.html#ScreenTapGesture "ScreenTapGesture") – 手指向前按.
*   [KeyTapGesture()](Leap.KeyTapGesture.html#KeyTapGesture "KeyTapGesture") – 手指向下按.

圆圈(Circle)手势和滑动(swipe)手势是连续性手势,所以有start,update,stop三个状态.

屏幕点击(screen tap)手势是一个瞬发而不连续的手势,所以Leap Motion软件只会为这个`ScreenTapGesture`对象的每次点击创建一个stop状态.

如果你已经从`Frame`对象中获取有用的手势(`Gusture`)对象,你可以从帧手势数组中获得一份手势的列表,你也可以给`Frame.gesture()`方法传入ID来在当前帧找到你想找的手势.

`Gesture`对象有时可能是无效的.举例来说, 如果你通过给`Frame.gesture()`传入一个ID来获得一个对应的手势对象,但是当前帧中没有这个ID的手势,那么`gesture()`就会返回一个无效的`Gesture`对象而不是返回一个null值.所以在使用你的`Gesture`对象前请检验其有效性.

一个未初始化的`Gesture`对象就是一个无效的手势对象.从`Gesture()`. 要从Frame对象下的Gestrue子类实例化出来的`Gesture`才是有效的.

###属性

####duration
类型 :number

手势的持续运行时间, 以毫秒为单位.

当然既然作为手势,就必须是幅度足够大的运动,否则Leap就不将其认定为手势.

####handIds[](#handIds "Permalink to this definition")
类型 :number[]

与手势相关的手对象的数据列表.

如果这个手势和任何手对象都没有关系,那么这个列表会是空的

如果控制器对象不是在设置使用`deviceFrame`模式循环下触发这一事件,那么设备关联的`frame`对象可能不是当前帧,也可能不会储存在历史缓冲区.
在帧与帧之间手和手指的物理位置信息可能已经改变了所以有的情况下拥有和前一帧中的手对象相同ID的手对象也有可能不会在当前帧出现.

####id
类型 :number

The gesture ID.

所有同一类型的`Gesture`对象都被分配了同一个ID值.将ID值传入`Frame.gesture()`方法可以找到你需要的手势对象在后续帧中的数据.

####pointableIds[](#pointableIds "Permalink to this definition")
类型: Array

与手势相关的手指,工具等指向型对的数据列表.

如果这个手势和任何指向型对象都没有关系,那么这个列表会是空的

如果控制器对象不是在设置使用`deviceFrame`模式循环下触发这一事件,那么设备关联的`frame`对象可能不是当前帧,也可能不会储存在历史缓冲区.
在帧与帧之间手和手指的物理位置信息可能已经改变了所以有的情况下拥有和前一帧中的指向型对象相同ID的指向型对象也有可能不会在当前帧出现.

####state
类型 :string

手势的ID.

众所周知每个动作都有开始,进行中和结束三种状态.`state`属性就是一个报告此时手势处于什么状态的属性.

这个属性的取值:

*   start
*   update
*   stop

####type
类型 :string

手势的类型.

 `type` 的值会是以下几个中的一个:

*   circle
*   swipe
*   screenTap
*   keyTap
