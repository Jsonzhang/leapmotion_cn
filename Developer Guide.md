#Developer Guide

下面的内容是对 Leap Motion JavaScript API 做一个简单的介绍. `Leap` 对象是一个独立的库, 就像 jQuery的 `$`. 你需要借助`Leap`对象这座桥梁才能获取到Leap检测器上的数据流.

<div class="row-fluid">
  <div class="span4">
    <h2>Methods</h2>
    <ul>
      <li><a href="#Leap_loop">Leap.Loop(callback)</a></li>
    </ul>
  </div>
  <div class="span8">
    <h2>Classes</h2>
    <div class="row-fluid">
      <div class="span6">
        <ul>
          <li><a href="#Leap_Frame">Leap.Frame</a></li>
          <li><a href="#Leap_Pointable">Leap.Pointable</a></li>
          <li><a href="#Leap_Hand">Leap.Hand</a></li>
          <li><a href="#Leap_InteractionBox">Leap.InteractionBox</a></li>
          <li><a href="#Leap_Controller">Leap.Controller</a></li>
        </ul>

      </div>
      <div class="span6">

        <ul>
          <li><a href="#Leap_Gesture">Leap.Gesture</a></li>
          <!--li>Leap.KeyTapGesture</li>
          <li>Leap.ScreenTapGesture</li>
          <li>Leap.SwipeGesture</li>
          <li>Leap.CircleGesture</li -->
        </ul>

      </div>
    </div>
  </div>
</div>


唯一需要开发者自己手动实例化的类就是 `Controller` 类. (对大多数程序来说你也只需要实例化一次就够了.) 其他的方法,对象和类都由`Controller`类来调用.

##如何从 Leap Motion Controller 获取数据

如果你要从Leap Motion controller获取数据,你要这样做:

1. 将 Leap Motion controller 硬件连接到你的电脑USB接口
2. 安装 Leap Motion 驱动
准备就绪你就可以获取到你想要的数据了,当然这里只介绍如何使用Javascript API.

有三种方法可以在你的JS应用中获取到你想要的数据:

1. 调用 Leap.loop()
2. 创建一个 controller 然后侦听帧事件
3. 创建一个 controller 然后将 controller.frame() 作为参数

###侦听 Leap.loop() 

`Leap.loop(handler)` 这个函数建立了一个`Controller`对象,这个对象会自动连接上后台的 Leap Motion 服务. 你只需要提供一个函数来处理获得的帧数据就可以了,像下面这样:

    Leap.loop(function(frameInstance){
      ...
    });

###侦听 Controller

你可以创建你自己的`Controller`对象,然后用这个对象来添加对帧事件的侦听. 像下面这么写,每一帧的数据都会从`Leap Motion controller`传播到你的控制器中:

      var my_controller = new Leap.Controller({enableGestures: true});
      // 关于参数请参照文档中对于 Controller 类的介绍 
      my_controller.on('frame', function(frame_instance){ ... });
      my_controller.connect();

###轮询 Leap.Controller

你也可以创建一个不侦听frame事件的 `Controller` 对象, 创建之后你只需要调用`Controller.frame()` 方法, 这样你就可以自己决定轮询的间隔:

      var my_controller = new Leap.Controller({enableGestures: true});
    
      my_controller.on('connect', function(){
        setInterval(function(){
          var frame = my_controller.frame();
        }, 500);
      });
    
      my_controller.connect();

这样做其实有几个潜在的优点:

1. 你可以自己控制轮询间隔; 这样做可能可以根据实际的需求节省资源提高性能, 或者是表现出更细致的动画.
2. 你可以在不同的轮询间隔中测试哪一个可以让你的应用有最佳的表现.你可以在应用最后八帧做抽样调查,取出性能最好获得最多数据的一帧按照这个参数去设置你的应用.
3. 你可以选择是否接受信号和数据. 比如在用户按下某一个你设置的虚拟按钮后就停止数据的监听,而再次点击后就开始监听,诸如此类的操作.










##计量单位

长度单位都是以毫米为单位,数值类型是浮点数. 如果你想获得一个相对的测量值(百分比),可以使用 `InteractionBox.normalizePoint` 这个方法. 如果你有看轴方向上`Leap.InteractionBox `文档. 坐标系上的位置和方向都是用包含三个数值的数组对象来表示的([x, y, z]). 你可以使用 glMatrix 这个库来用数学向量和矩阵的原理操作这些三维数组.

所有角度都是用弧度来作为单位, 如果需要转换为角度数,只需要乘以 180/Math.PI.

所有的时间单位都是毫秒;1000000毫秒 = 1 秒. 时间戳是相对于Leap Motion软件初始化后的时间.两个时间戳相减就能得到他们间的时间距离.

Frame indexes reflect the LIFO nature of the Leap.Controller's frames collection.

+ my_controller.frames(0)  == my_controller.frames() == the most recent frame.
+ my_controller.frames(1) is the previous frame
+ my_controller.frames(2) is two frames ago

诸如此类. 要记住`frames`这个对象是会被不断地推入到栈中的,所以`my_controller.frames(2)`在几毫秒后就会是一个不同的 `frame` 对象 , 如果需要进一步确认可以使用`frame.id`来识别和比较不同的帧.


所有 **positions** 和 **directions** are returned as an array of three coordinates (see above). See Leap.InteractionBox for methods on normalizing and denormalizing coordinates.

###scaleFactor

`Leap.Hands` 和 `Leap.Frames` 都有一个 `scaleFactor` 函数. Scale factor indicates a change in distance between things over time. 对于手对象 `hands`来说, this means the change in distance between the fingers. The function has a parameter: a baseline frame.

####比例因子对手的影响
![](https://di4564baj7skl.cloudfront.net/assets/leapjs/pointables/jazz_hands-f65fc3f5c55251061637dabb0e778b09.png)

手比例因子会影响映射后手和手指的相对位置,如上图,如果比例因子是0.5 手会被平行拉伸,反之为2则被平行压缩.

+ 如果你把手指并拢,你的手的比例因子应该小于 1.
+ 如果你把手指张开,你的手的比例因子则应该大于 1.
####比例因子对帧(frame)的影响
![](https://di4564baj7skl.cloudfront.net/assets/leapjs/pointables/jazz_arms-2724ffe5e992e50b4c46701706568243.png)






当`frame.scaleFactor`(old_frame) 被调用时它会 it reflects the relative distance between the hands -- the "fish measuring factor".

+ 如果你分开你的双手,这个比例因子应该大于 1.
+ 如果你将双手向彼此靠拢,这个比例因子则应该小于 1.
+ 如果Leap程序不能够找到当前帧的参考基线,这个比例因子则为1

##函数返回的结果

可以返回实例对象(Pointable, Frame, Hand,...)的函数始终返回的都会是对象而不会是其他,即使在参数错误或者你的手不在检测设备上的时候也不会返回false,null或者抛出错误,这个时候它会返回一个无效的示例对象,但是依旧是返回一个对象.

这样的机制让你可以在任何时候都能访问到其子对象,如fingers, 不用每次都做这个frame是否存在,或者frame中是否存在hand之类的检查,但是你应该检查你要获取的对象是否存在,否则你在使用它的属性和方法的时候也会出现错误. 所有的Leap实例都有类似Boolean值这样有效的值(除了 `Gesture` 对象).

##Leap Methods

###Leap.Loop(callback)

这是一个会在Leap全局对象上大约每秒60次重复调用的静态方法,
This static method of the Leap global object fires repeatedly, approximately sixty times a second. 也可以看看 [从你的Leap Motion控制器获取数据]() 的其他方法.

回调函数接收一个 `Leap.Frame` 实例,更多详见 [`Leap.loop()`]()


##Leap Classes

###Leap.Frame

这个对象包含的是硬件反馈的信息汇总. 一个`frame`对象就是一个 "根" 数据; 这个对象包含了Leap Motion检测器中的所有位置信息,包括Fingers/Tools/Pointables等.

在文档中我们可以知道从Leap Motion控制器获取数据就意味着取得其中`frames`这个部分.

`Frame.Fingers`, `Frame.pointables`, 和 `Frame.tools`简介:

![](https://di4564baj7skl.cloudfront.net/assets/leapjs/pointables/pointables-3f978e63923a623262d4422999a20e67.png)


`frame.fingers`, `frame.tools` 和 `frame.pointables` 都是`Leap.Pointable`的实例对象,但是又都是包含着不同数据的实例. (没有像 `Leap.Tools` 或者 `Leap.Fingers` 这样的类,只有`Leap.Pointable`.)

+ `pointables` 集合包括检测到的所有指向型物体,包括手指和工具.
+ `tools` 集合 和`fingers` 集合中的对象是互斥的,就是说两者中的对象组合成了`pointables`对象且两者之间没有任何一个重叠共同拥有的对象.
+ 根数据中的所有` pointables` 数据都被放在 `frame.hands` 对象下面.
+ 以上提到的这些数据集合可以是一个空集合, 比如使用者的手不在检测设备上时,那么就会返回一个空集合.
+ 所有的手指,工具或者类似的指向性物体的数据都被无序存放在根数据集合中.

####Frame Properties:
+ [currentFrameRate]()
+ [fingers[]]()
+ [hands[]]()
+ [id]()
+ [interactionBox]()
+ [Invalid]()
+ [valid]()
+ [pointables[]]()
+ [timestamp]()
+ [tools[]]()
####Frame Functions:
+ [dump()]()
+ [finger()]()
+ [hand()]()
+ [pointable()]()
+ [rotationAngle()]()
+ [rotationAxis()]()
+ [rotationMatrix()]()
+ [scaleFactor()]()
+ [tool()]()
+ [toString()]()
+ [translation()]()

更多详见, API 指引中关于 Frame 的介绍.

###Leap.Pointable

`Pointable` 类代表了被探测到的手指或者工具.手指和工具都是被当作指向类的对象. 用 `Pointable.tool` 属性来确认一个指向类对象代表的是手指还是工具,当前前提是这个Pointable 对象是可用的, which means that they do not contain valid tracking data and do not correspond to a physical entity. 比如你使用一个之前帧内出现过的Pointable对象的ID来获取当前帧内对应的 Pointable 对象,那么就会返回一个无效的Pointable 对象.

** The Leap 会将一个比普通手指细,长,直的物体分类为工具(tools).

####Pointable Properties:
+ [direction]()
+ [id]()
+ [Invalid]()
+ [length]()
+ [stabilizedTipPosition]()
+ [timeVisible]()
+ [tipPosition]()
+ [tipVelocity]()
+ [tool]()
+ [touchDistance]()
+ [touchZone]()
+ [valid]()
+ [width]()
####Pointable Functions:
+ [hand()]()
+ [toString()]()

更多详见 API 文档中对 Pointable 的介绍.

###Leap.Controller

这个控制器函数代表Leap设备和程序之间的连接. 你可以在一个应用内激活几个控制器.但是要注意的是 `Leap.Controller` 是一个 JavaScript 类;而 Leap Motion controller 表现的数据是来自硬件设备的.

下面这段代码创建了一个 `Leap.Controller` ,可以传入一个可选的对象参数 , 这个对象里可以传入是否启用手势和浏览器循环之类等参数.

      var controller = new Leap.Controller({
      enableGestures: true,
      frameEventName: 'animationFrame'
    });

这些传入的参数都是可选的; 所以像下面这样也是可以运行的:

    var controller = new Leap.Controller({enableGestures: true});


####Controller 事件

+ **[connect]()** 侦听客户端是否与WebSocket服务器端连接成功.
+ **[frame]()** 侦听内部循环事件的开始. 这个事件会被`animationFrame`或者 `deviceFrame` 触发, 取决于` Leap.Controller` 何时被创建. 而最近一帧的数据就会不断作为参数传入到这个事件的回调函数里,而程序只能保证这是最近一帧的数据,这一帧数据可能是新的,也可能不是.
+ **[gesture]()** 侦听从WebSocket接收到的每一个手势对象.
+ **[disconnect]()** 侦听客户端与浏览器连接不成功.
+ **[focus]()** 侦听浏览器获得焦点.
+ **[blur]()** 侦听浏览器失去焦点.
+ **[deviceConnected]()** 侦听Leap设备连接.
+ **[deviceDisconnected]()** 侦听Leap设备断开.
+ **[protocol]()** The protocol has been selected for the connection. The protocol object is passed as an argument to the event handler.

下面介绍一个Leap程序正常来说如何初始化:
    
    var controller = new Leap.Controller();
    
    controller.on('connect', function() {
      console.log("Successfully connected.");
    });
    
    controller.on('deviceConnected', function() {
      console.log("A Leap device has been connected.");
    });
    
    controller.on('deviceDisconnected', function() {
      console.log("A Leap device has been disconnected.");
    });

    controller.connect();

####Controller 属性:
+ [frameEventName]()
####Controller 函数:
+ [connect()]()
+ [disconnect()]()
+ [frame()]()
+ [inBrowser()]()
+ [on()]()
+ [plugin()]()
+ [setBackground()]()
+ [stopUsing()]()
+ [use()]()

更多详见API文档中的 Controller 部分.

###Leap.Hand

Hand类代表被检测到的手的物理信息.手的信息是以`Leap.Frame`下的 `hand`数组这样的形式存在的.两只手是可以同时被追踪并且记录在frame的数据中.手的追踪数据包括了位置信息和方向向量;方向向量分为手掌方向和手指方向; hand对象的属性来自于模拟的手中球; `hand`对象上绑定有手指和工具的数据信息.

看文档中的`Leap.Frame`类能知道更多关于 Pointables, Fingers 和 Tools 的区别在哪.

下面这张图展示了关于手的两个方向向量 `palmNormal`和`palmPosition`:

![Palm Vectors](https://di4564baj7skl.cloudfront.net/assets/leapjs/Leap_Palm_Vectors_annotated-0d234a44a0f753f262ca31ede87882f0.png)

####Hand Properties:

+ [direction]()
+ [fingers[]]()
+ [id]()
+ [Invalid]()
+ [valid]()
+ [palmNormal]()
+ [palmPosition]()
+ [palmVelocity]()
+ [pointables[]]()
+ [sphereCenter]()
+ [sphereRadius]()
+ [stabilizedPalmPosition]()
+ [timeVisible]()
+ [tools[]]()
+ Hand Functions:
+ [finger()]()
+ [pitch()]()
+ [roll()]()
+ [rotationAngle()]()
+ [rotationAxis()]()
+ [rotationMatrix()]()
+ [scaleFactor()]()
+ [toString()]()
+ [translation()]()
+ [yaw()]()

更多详见API文档中的 Hand 部分.

###Leap.InteractionBox

A representation of the "airspace" in which the Leap Motion controller can measure/see your hands and fingers. Note that the range of the interaction box may change with hardware or software advances. Use the interaction boxes' properties to scale your measurements -- don't hard code these values in your JavaScript.

####The Axes' orientation
![Leap_axes_annotated](https://di4564baj7skl.cloudfront.net/assets/leapjs/Leap_Axes_annotated-2155d4518ff426c9316abc13d88042dc.png)

+ X轴向右伸展,原点在Leap Motion控制器的中心.
+ Y轴向上伸展,原点在Leap Motion控制器的中心; 可能相对于你的屏幕来说不是这样,但是Y轴的零点的确非常接近Leap Motion 控制器.
+ Z轴朝使用者的身体伸展,原点在Leap Motion控制器的中心. 所以你的显示器在-Z的位置而你的身体在+Z的位置.

####InteractionBox 属性:
+ [center]()
+ [depth]()
+ [height]()
+ [Invalid]()
+ [valid]()
+ [width]()
####InteractionBox 方法:
+ [denormalizePoint()]()
+ [normalizePoint()]()
+ [toString()]()

更多详见API文档中的`InteractionBox`.

###手势

`Gesture` 类包含了被探测到辨识出的所有手势数据.  Leap 会监测视野范围内物体的运动和是否构成手势并将这些数据返回到程序.比如用手做出一个左右摇摆的动作就会被视为是一个滑动(swipe)手势,当一个手指向前戳屏幕的时候就会被视为一个屏幕点击(screen tap)手势.当Leap检测辨认到手势的出现时,程序会自动为这个手势注册一个ID,并将这个手势对象添加到帧的手势列表中.持续的手势会在很多帧里被触发,对于一个这样的手势 Leap 会不断向这个手势对象更新数据,但是这个手势结束之前这个对象的ID是不会变的. 需要提醒的一点是: 手势识别需要在参数里开启,否则任何手势都不会被监测到;,`Gesture`的子类定义了每个手势特殊的运动模式,这些运动模式能够被Leap辨识到.

手势分为四类:

+ **圆圈手势** 手指绕圈动
+ **滑动手势** 五指张开,手掌线性滑动.
+ **屏幕点击手势** 手指向前点击.
+ **按键点击手势** 手指向下点击.

###手势状态

圆圈手势和滑动手势是连续手势,所有会有一个开始的状态,更新的状态和停止的状态.

屏幕点击手势是一个瞬间的非连续的手势,所以Leap只会创建一个`ScreenTapGesture`对象,而这个对象之会有一个停止的状态可供侦听.

###从`Frame` 对象中获取到手势数据

你可以从帧的手势数组中获取到一个关于手势数据的列表.你也可以使用`Frame gesture()`这个方法去按照过去某一帧中获取到的ID找到一个当前帧的对应手势. `Gesture` 对象可以是空的.

更多详见API文档中的解释:

+ [Gesture]()
+ [CircleGesture]()
+ [KeyTapGesture]()
+ [ScreenTapGesture]()
+ [SwipeGesture]()