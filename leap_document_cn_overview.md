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
If it only detects one hand, then the Leap bases the frame motion factors on the movement of that hand. If it detects two hands, then the Leap bases the frame motion factors on the movement of both hands together. 

leap程序会整体分析从上一帧到这一帧的位移、旋转、放大缩小的变量。例如,如果您的双手移到leap motion检测范围的最左边缘这个过程中就包含了位移(translation),如果你想象你手中握着一个球,然后你抚摸这个球的边缘,那么你这个动作就包含了旋转(rotate)。如果你的两只手之间的距离发生改变,那么你这个动作就包含了放大缩小操作(scale)。leap中使用的检测范围内所有的对象来分析其运动。如果它只检测到一只手,然后leap基于帧运动运动的一方面因素。如果它检测到两只手,leap的每一帧分析就会基于 一起运动因素对双手的运动。你也可以从hand对象中获取到任何一只手的独立活动数据.

Frame motions are derived by comparing the current frame with a specified earlier frame. The attributes describing the synthesized motion include:

+ rotationAxis — A direction vector expressing the axis of rotation.
+ rotationAngle — The angle of rotation clockwise around the rotation axis (using the right-hand rule).
+ rotationMatrix — A transform matrix expressing the rotation.
+ scaleFactor — A factor expressing expansion or contraction.
+ translation — A vector expressing the linear movement.
+ You can apply the motion factors to manipulate objects in your application's scene without having to track individual hands and fingers over multiple frames.

####Hand model

The hand model provides information about the position, characteristics, and movement of a detected hand as well as lists of the fingers and tools associated with the hand.

The Leap API provides as much information about a hand as possible. However, the Leap may not be able to determine all hand attributes in every frame. For example, when a hand is clenched into a fist, its fingers are not visible to the Leap so the finger list will be empty. Your code should handle the cases where an attribute in the model is not available.

The Leap does not determine whether a hand is a left or right hand. More than two hands can appear in the hand list for a frame if more than one person's hands or other hand-like objects are in view. However, we recommend keeping at most two hands in the Leap Motion Controller's field of view for optimal motion tracking quality.

####Hand attributes
The Hand object provides several attributes reporting the physical characteristics of a detected hand:

palmPosition — The center of the palm measured in millimeters from the Leap origin.
palmVelocity — The speed of the palm in millimeters per second.
palmNormal — A vector perpendicular to the plane formed by the palm of the hand. The vector points downward out of the palm.
direction — A vector pointing from the center of the palm toward the fingers.
sphereCenter — The center of a sphere fit to the curvature of the hand (as if it were holding a ball).
sphereRadius — The radius of a sphere fit to the curvature of the hand. The radius changes with the shape of the hand.
The direction and palmNormal are unit direction vectors describing the orientation of the hand with respect to the Leap coordinate system.

![Leap 的右手笛卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Palm_Vectors.png)

The normal vector points perpendicularly out of the hand; the direction vector points forward.

The sphereCenter and sphereRadius describe a sphere that is placed and sized to fit into the curvature of the hand:

![Leap 的fsa卡尔坐标系统](https://developer.leapmotion.com/documentation/Common/images/Leap_Hand_Ball.png)

The size of the sphere decreases as the fingers are curled.

Hand motion
The Hand object also provides several attributes reporting the motion of a detected hand between frames. The Leap analyzes the motion of the hand, as well as its associated fingers and tools and reports representative translation, rotation, and scale factors. Moving your hand around the Leap field of view produces translation. Turning, twisting, or tilting your hand produces rotation. Moving fingers or tools toward or away from each other produces scaling.

Hand motions are derived by comparing the characteristics of the hand in the current frame to those in a specified earlier frame. The attributes describing the synthesized motion include:

rotationAxis — A direction vector expressing the axis of rotation.
rotationAngle — The angle of rotation clockwise around the rotation axis (using the right-hand rule).
rotationMatrix — A transform matrix expressing the rotation.
scaleFactor — A factor expressing expansion or contraction.
translation — A vector expressing the linear movement.
Finger and Tool lists
You can access the fingers and tools associated with a hand using one of three lists:

Pointables — Both fingers and tools as Pointable objects.
Fingers — Just the fingers.
Tools — Just the tools.
You can also find an individual finger or tool using an ID value obtained in a previous frame. Use the Hand.finger(), the Hand.tool(), or, if you don't need to distinguish between fingers and tools, the Hand.pointable() function. These functions return a reference to the corresponding object in the current frame if it exists. If a finger or tool is not associated with the hand in this frame, then an invalid object is returned.

Finger and Tool models

The Leap detects and tracks both fingers and tools within its field of view. The Leap classifies finger-like objects according to shape. A tool is longer, thinner, and straighter than a finger.

In the Leap model, the physical characteristics of fingers and tools are abstracted into a Pointable object. Fingers and tools are types of pointable objects. The physical characteristics of pointable objects include:

length — The length of the visible portion of the object (from where it extends out of the hand to the tip).
width — The average width of the visible portion of the object.
direction — A unit direction vector pointing in the same direction as the object (i.e. from base to tip).
tipPosition — The position of the tip in millimeters from the Leap origin.
tipVelocity — The speed of the tip in millimeters per second.
../../../Common/images/Leap_Finger_Model.png
Finger tipPosition and direction vectors provide the positions of the finger tips and the directions in which the fingers are pointing.

The Leap classifies a detected pointable object as either a finger or a tool. Use the Pointable.tool property to determine which one a Pointable object represents.

../../../Common/images/Leap_Tool.png
A tool is longer, thinner, and straighter than a finger.

Gestures

The Leap recognizes certain movement patterns as gestures which could indicate a user intent or command. The Leap reports gestures observed in a frame the in the same way that it reports other motion tracking data like fingers and hands. For each gesture observed, the Leap adds a Gesture object to the frame. You can get these Gesture objects from the Frame gestures list.

 The following movement patterns are recognized by the Leap:
Circle — A single finger tracing a circle.
Swipe — A linear movement of the hand.
Key Tap — A tapping movement by a finger as if tapping a keyboard key.
Screen Tap — A tapping movement by the finger as if tapping a vertical computer screen.
When the Leap first classifies a movement pattern as a gesture, it adds a Gesture object to the frame. If the gesture continues over time, the Leap adds updated Gesture objects to subsequent frame. The gestures Circle and Swipe are continuous. The Leap updates the progress of these gestures each frame. Taps are discrete gestures. The Leap reports each tap with a single Gesture object.

Important: before using gestures in your application, you must enable recognition for each gesture you intend to use. The Controller class has an enableGesture() method that you can use to enable recognition for the types of gestures you use.

Circle
The Leap recognizes the motion of a finger tracing a circle in space as a Circle gesture.

../../../Common/images/Leap_Gesture_Circle.png
A circle gesture with the forefinger.

You can make a circle with any finger or tool. Circle gestures are continuous. Once the gesture starts, the Leap will update the progress until the gesture ends. A circle gesture ends when the circling finger or tool departs from the circle locus or moves too slow.

See CircleGesture in the API reference for more information.

Swipe
The Leap recognizes a linear movement of a finger as a Swipe gesture.

../../../Common/images/Leap_Gesture_Swipe.png
A horizontal swipe gesture.

You can make a swipe gesture with any finger and in any direction. Swipe gestures are continuous. Once the gesture starts, the Leap will update the progress until the gesture ends. A swipe gesture ends when the finger changes directions or moves too slow.

See SwipeGesture in the API reference for more information.

Taps
The Leap recognizes two types of taps: the downward Key Tap and the forward Screen Tap.

Key Taps

The Leap recognizes a quick, downward tapping movement by a finger or tool as a Key Tap gesture.

../../../Common/images/Leap_Gesture_Tap.png
A key tap gesture with the forefinger.

You can make a key tap gesture by tapping downward as if pressing a piano key. Tap gestures are discrete. Only a single Gesture object is added per tap gesture.

See KeyTapGesture in the API reference for more information.

Screen Taps

The Leap recognizes a quick, forward tapping movement by a finger or tool as a Screen Tap gesture.

../../../Common/images/Leap_Gesture_Tap2.png
A screen tap gesture with the forefinger.

You can make a key tap gesture by tapping or pushing foward in space as if touching a vertical touch screen. Tap gestures are discrete. Only a single Gesture object is added per tap gesture.

See ScreenTapGesture in the API reference for more information.

Copyright © 2012-2013 Leap Motion, Inc. All rights reserved.

Leap Motion proprietary and confidential. Not for distribution. Use subject to the terms of the Leap Motion SDK Agreement available at https://developer.leapmotion.com/sdk_agreement, or another agreement between Leap Motion and you, your company or other organization.