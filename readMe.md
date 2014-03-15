#概述

leap是一种检测和跟踪的手、手指和类似手指的工具。
设备会对近距离的手或者手指进行高速率,高精度的跟踪。

leap软件可以分析跟踪在设备视野内的物体,而且能辨认出是手,手指还是其他工具,并在电脑中呈现出两个不同位置的手势和动作分析.
leap软件的分析视野如下图所示,是一个类似倒立的金字塔形状的区域.如下图所示,这个工具的有效区域大约是在设备以上25mm~600mm之间.

##主题:

+ leap的坐标系统
+ 手势的跟踪数据
	+ 帧
		+ 跟踪的数据列表
		+ 帧运动的数据
	+ 手模型
		+ 手的属性
		+ 手部运动捕捉
		+ 手指和工具列表
	+ 手指和工具模型
	+ 手势
		+ 绕圈手势
		+ 滑动手势
		+ 压键手势
	+ 模拟按键
		+ 屏幕按键

###leap的坐标系统

 leap使用的是毫米级精度的右手笛卡尔坐标系统,以设备中心为原点.x轴与z轴构成水平面,其中x轴是和长方形设备较长一边平行,而y轴是垂直的,坐标随着现实高度增加而增加(和大多数计算机内部的坐标系统不一样). 而z轴是随着电脑屏幕所面对的方向的距离增加而增加.如下图.


 ![Leap 的右手笛卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Axes.png)

  

###动作追踪数据

 当leap追踪手,手指或者是工具的时候,它会不断地为每一帧提供一份不同的追踪数据. 每份数据都包括这个帧所有运动数据,比如手,手指或者是工具的位置信息,是否识别出了手势的信息等等.

 当设备检测到手,手指,工具或者是类似的东西,leap就会给这个被检测到的物体标记一个独立的id指示符,只要这个物体不离开设备的检测范围,那么它的id就一直不会变.如果这个物体(手,手指)离开监测范围后返回,那么就算是同一个物体他的id也是会改变的.

 提示: 我们会在每次产品发布之前提供给各位开发者提供更准确的运动跟踪数据。而在leap SDK的将来版本中,我们计划引入一个手的骨骼模型来给开发者提供更详细的跟踪数据和连续性


####帧

一个帧对象中提供了跟踪数据、手势数据和因素描述的整体运动中观察到的视野。

追踪数据列表:

+ Hands — 所有被检测到的手.
+ Pointables — 所有手指和尖状的类似手指的东西.
+ Fingers — 所有手指.
+ Tools — 所有其他工具.
+ Gestures — 所有手势开始,结束或者更新时可以触发自定义事件的api.
+ 三个pointables列表(Pointables, Fingers, and Tools)包含在每一帧中检测到的每个pointable对象。您可以访问直接通过hand对象来访问pointables对象。注意,如果只是类似手指的工具或者真的只是一只手指(在leap的检测范围内没有手掌),那么leap就不会把这个pointable对象放到hand对象中.

如果你在每一帧中只是需要跟踪一个物体,比如一个手指,你就可以直接用这个对象的id来在众多的对象里直接找到他. 下面这些函数,传入id就可以返回对应的对象:

+ Frame.hand()
+ Frame.finger()
+ Frame.tool()
+ Frame.pointable()
+ Frame.gesture()


如果对象已经不复存在,那么会返回一个特殊的无效对象,这个无效对象是在leap源码中定义好的,不是Javascript中的null,这个对象包含有效的跟踪数据。这样你在返回对象数据的时候就不需要做null判断了.

####Frame motion
如果leap只检测到一只手,那么它就会根据这只唯一的手的动作来决定帧运动.而如果它检测到了两只手,那么这两只手的运动速率都会影响帧的刷新.

leap程序会整体分析从上一帧到这一帧的位移、旋转、放大缩小的变量。例如,如果您的双手移到leap motion检测范围的最左边缘这个过程中就包含了位移(translation),如果你想象你手中握着一个球,然后你抚摸这个球的边缘,那么你这个动作就包含了旋转(rotate)。如果你的两只手之间的距离发生改变,那么你这个动作就包含了放大缩小操作(scale)。leap中使用的检测范围内所有的对象来分析其运动。如果它只检测到一只手,然后leap基于帧运动运动的一方面因素。如果它检测到两只手,leap的每一帧分析就会基于 一起运动因素对双手的运动。你也可以从hand对象中获取到任何一只手的独立活动数据.

在帧刷新的过程中,会比较当前帧和之前的帧的区别,然后输出一些值来描述这些变化,这些值包括:

+ rotationAxis —  描述旋转坐标的矢量
+ rotationAngle — 右手坐标模型里的旋转角度.
+ rotationMatrix — 一个可以描述旋转运动的矩阵.
+ scaleFactor — 一个变量,表示对象的放大或者缩小.
+ translation — 一个表示运动轨迹的变量.
+ 你可以把这些运动因素用在您的应用程序里,就不用在每个帧都跟踪每个手和手指的位置.

####Hand model

Hand模块提供与手有关的所有信息,比如位置,形状等等.

Leap API对于手这个模型提供可尽可能详尽的数据,尽管这样,Leap可能还是不能保证说每一帧都能获取到有关于手的所有属性.比如当手的五指握紧成拳的时候,Leap可能分不清哪个部位是手指,这时候手指的数据列表就会是空的.您的代码应该足够健壮,保证说在模块里的某一个属性不可用之后代码依然能正确运行.

Leap也没有办法分辨一只手是左手还是右手,所以也不会提供相关的数据.所以当"第三只手"闯入leap的视野的时候leap也会读取并返回相关的数据,但是,因为检测的技术有限,我们建议视野内类似手的物体最好控制在两个以内,这样leap才能返回最佳最准确的检测数据.

####Hand attributes

Hand对象提供了几个属性来描述被检测到的手的物理运动:

+ palmPosition — 这个属性提供的数据是以手掌中心为准的坐标移动单位方向向量,精确到毫米.
+ palmVelocity — 这是手掌移动的矢量速度.
+ palmNormal —  这是一个垂直于手掌的矢量,方向是从手掌向手掌外延长(如图).
![Leap 的右手笛卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Palm_Vectors.png)
+ direction — 这个单位方向向量从手掌指向手指,表示手的方向(如图).
+ sphereCenter — 如下图所示,这个属性根据手弯曲的程度模拟出一个球,并表示球心的位置 (好像用户握着一个球).
+ sphereRadius — 还是上面那个球,这个属性表示的是这个球的半径,球体大小随着手弯曲的程度改变.
![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Hand_Ball.png)




####Hand motion
Hand 对象也为被检测到的手势提供不同的几组数据.Leap会分析手掌的运动, as well as its associated fingers and tools and reports 对应的运动轨迹,放大缩小程度以及旋转角度. 如果你在Leap设备的范围内移动你的手就会产生对应的transition数据. 翻转, 滑动, 或者倾斜你的手都会产生rotation数据, 将你的手指或者类似的工具相对彼此拉近或者拉远就会产生scaling数据.

Leap会比较特定时间段前的某一帧和当前帧上述数据的变化来得出关于手势的数据. 描述这种手势(合成运动)的相关属性有:

+ rotationAxis — 描述旋转运动中轴线的矢量.
+ rotationAngle — 描述旋转运动中角度的矢量(右手坐标系统).
+ rotationMatrix — 表示旋转的变换矩阵.
+ scaleFactor — 表示扩张或者收缩的属性.
+ translation — 表示位移的矢量.

####Finger and Tool lists
你可以下面三个列表其中之一来访问Hand对象上的 fingers 和 tools:

Pointables — 指向性的东西,包括手指和工具.
Fingers — 手指.
Tools — 指向性的工具比如木棒.

当然你也可以用ID来找到特定的某根手指或者工具. 

	Hand.finger()
	Hand.tools()
	Hand.pointables()

如果要访问的对象是存在的,上述三个函数会返回对应对象的引用,如果手指或者工具在当前帧并不在检测到的手掌里面,则会返回一个单独的对象.

####Finger and Tool models

无论是手指还是工具,只要在检测范围内Leap程序都会检测和跟踪其运动轨迹.而且Leap会根据形状将类似手指的东西分类出来.工具相对于手指来说更加长,细和直.

![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Tool.png)

在 Leap 程序模型里, 手指和工具的物理特征就是一个指向性的对象. 手指和工具都是可以用来"指向"的东西,这类可以指向的东西包括的属性如下:

+ length — 对象的可见部分的长度.
+ width — The average width of the visible portion of the object.
+ direction — A unit direction vector pointing in the same direction as the object (i.e. from base to tip).
+ tipPosition — The position of the tip in millimeters from the Leap origin.
+ tipVelocity — The speed of the tip in millimeters per second.

![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Finger_Model.png)

Finger tipPosition and direction vectors provide the positions of the finger tips and the directions in which the fingers are pointing.

The Leap classifies a detected pointable object as either a finger or a tool. Use the Pointable.tool property to determine which one a Pointable object represents.




Gestures

The Leap recognizes certain movement patterns as gestures which could indicate a user intent or command. The Leap reports gestures observed in a frame the in the same way that it reports other motion tracking data like fingers and hands. For each gesture observed, the Leap adds a Gesture object to the frame. You can get these Gesture objects from the Frame gestures list.

 The following movement patterns are recognized by the Leap:

+ Circle — A single finger tracing a circle.
+ Swipe — A linear movement of the hand.
+ Key Tap — A tapping movement by a finger as if tapping a keyboard key.
+ Screen Tap — A tapping movement by the finger as if tapping a vertical computer screen.

When the Leap first classifies a movement pattern as a gesture, it adds a Gesture object to the frame. If the gesture continues over time, the Leap adds updated Gesture objects to subsequent frame. The gestures Circle and Swipe are continuous. The Leap updates the progress of these gestures each frame. Taps are discrete gestures. The Leap reports each tap with a single Gesture object.

Important: before using gestures in your application, you must enable recognition for each gesture you intend to use. The Controller class has an enableGesture() method that you can use to enable recognition for the types of gestures you use.

###画圈圈
The Leap recognizes the motion of a finger tracing a circle in space as a Circle gesture.
![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Gesture_Circle.png)

A circle gesture with the forefinger.

You can make a circle with any finger or tool. Circle gestures are continuous. Once the gesture starts, the Leap will update the progress until the gesture ends. A circle gesture ends when the circling finger or tool departs from the circle locus or moves too slow.

See CircleGesture in the API reference for more information.

###滑动
The Leap recognizes a linear movement of a finger as a Swipe gesture.
![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Gesture_Swipe.png)
A horizontal swipe gesture.

你可以在任何方向上做一个滑动的手势,滑动手势是连续的,一旦手势开始,Leap在手势结束之前会一直更新进程.当手指改变了方向或者是移动得很缓慢,Leap就会认为滑动手势结束了.



###点击

Leap可以认出两种不同的点击: 向下的按键点击和向前的屏幕点击

####按键点击

The Leap 可以检测出某个手指或者工具快速向下的点击动作,并且认为这个动作就是一个按键点击动作.
![A key tap gesture with the forefinger.](https://developer.leapmotion.com/documentation/Common/images/Leap_Gesture_Tap.png)

如果你在模拟钢琴按键的敲击那么你可以使用按键点击手势,这种点击手势之间是独立的,只有一个简单的手势对象会被添加到每个点击手势里.

####屏幕点击

The Leap recognizes a quick, forward tapping movement by a finger or tool as a Screen Tap gesture.
![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Gesture_Tap2.png)

上图是点击(Tap)动作的触发方式.

只要你朝检测范围内与屏幕垂直的方向突然向屏幕方向移动你的手指就会触发一次按键点击.而点击手势是不连续的,就是说多次点击触发的事件是相互独立的,每次都会独立生成一个点击对象可供程序操作.
如果想知道更多以及具体使用方法可以看 API 文档中有关于屏幕点击的信息.