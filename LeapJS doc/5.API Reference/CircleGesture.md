## 圆圈手势[ ](\#id43 "Permalink to this headline")

### _class_ CircleGesture()[ ](#CircleGesture "Permalink to this definition")

 `CircleGesture` 这个类表示一个手指圆圈的动作.
 Leap的检测范围内用手指做画圈圈这个动作能够被Leap辨认出来.
 
 ![CircleGestureImage](https://developer.leapmotion.com/documentation/images/Leap_Gesture_Circle.png)
 
 画圈圈这个手势是连续的。`CircleGesture`对象的手势有三种可能的状态:
 
 *   start(开始) – 手势开始. 圆圈的距离不能太小否则Leap也没有办法辨认出这个手势.
 *   update(更新) – 手势持续进行中.
 *   stop(停止) – 手势结束.
 
 未初始化的 `CircleGesture` 对象是无效的. 从 `Frame` 对象中的`CircleGesture`获取的实例才是有效的.
 
 _更多_ [Gesture()](Leap.Gesture.html\#Gesture "Gesture")





##属性

###center
+ 类型 : number[] 

+ 说明 : 一个拥有三个元素的数组,圆圈手势的中心坐标.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.2.0

###duration
+ 类型 :number

+ 说明:运动持续时间,单位是毫秒.一般时间序列中第一个手势的持续时间(状态为start)通常会是一个小的正数,因为动作本身需要一个足够大的幅度Leap才能辨认出这个手势.

+ 版本Leap Motion 1.0 和 LeapJS 0.2.0

###h和Ids[](#h和Ids "Permalink to this definition")

+ 类型 : array

+ 说明 : 与当前手势有关的手的ID,如果当前手势与手无关或者没有手,那么这个数组为空.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###id
+ 类型 : number

+ 说明 : 手势的ID.`Gesture` 对象中的几个手势如果被认定属于同一个动作中的手势,那么这几个手势会有一样的ID,比如手指不断地转圈,多个圆圈手势的ID就是一样的.可以把 ID 传入 Frame.gesture() 方法来取得这个手势在当前帧的数据.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###normal
+ 类型 : number[] – 一个拥有三个元素的数组,代表手势的法线方向矢量.


如果你顺时针画一个圆,那么法线向量应该和画圆的指向型物体的方向是一致的.如果你逆时针画,那么就应该是相反的.如果法线和画圆的物体的角度小于90度,那么这个圆应该是顺时针的。你可以使用这两个向量的点积来确认这两个向量受否大致在一个方向上：

    var clockwise = false;
    var pointableID = gesture.pointableIds[0];
    var direction = frame.pointable(pointableID).direction;
    var dotProduct = Leap.vec3.dot(direction, gesture.normal);
    if (dotProduct > 0) clockwise = true;

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###pointableIds[](#pointableIds "Permalink to this definition")

+ 类型 : Array

+ 说明 : 如果有手势被检测到发生,那么此时检测到的手指或者工具的数据列表都是被绑定在`Gestrue`对象上的.当然如果此时检测没有手指和工具对象,那么`Gestrue`下的这个列表就是空的.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###progress

+ 类型 : number

+ 说明 : The number of times the finger tip has traversed the circle.

Progress is reported as a positive number of the number. For example, a
progress value of .5 indicates that the finger has gone halfway around,
while a value of 3 indicates that the finger has gone around the the
circle three times.

Progress starts where the circle gesture began. Since the circle must be
partially formed before the Leap can recognize it, progress will be
greater than zero when a circle gesture first appears in the frame.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###radius
+ 类型 :number

+ 说明 : 单位是 mm,数值是圆的半径.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###state
+ 类型 : string – “start”, “update” 和 “stop” 三者取值,其中一个

+ 说明 : The gesture state.

Recognized movements occur over time 和 have a beginning, a middle, 和
an end. The ‘state’ attribute reports where in that sequence this
Gesture object falls.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0

###type

+ 类型 : string

+ 说明 : 手势类型; 比如圆圈手势,就是 “circle”.

+ 版本 : Leap Motion 1.0 和 LeapJS 0.3.0
