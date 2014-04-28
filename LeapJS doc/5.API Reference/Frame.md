# Frame[](\#frame "Permalink to this headline")

##_class _Frame()[](#Frame "Permalink to this definition")

`Frame` 类(的实例)包含了某一帧里对于所有被追踪到的手和手指的数据.

Leap 可以在限定范围内检测到手,手指和工具,然后以当前的帧频率报告这些被检测对象的位置,方向和动作.

由控制器创造出来的`Frame` 实例是无效的.正确的方法应该是调用`Controller.frame()`方法.

###Attribute
--------------------------------
####currentFrameRate[](#currentFrameRate "Permalink to this definition")

类型 :float – 每秒的帧数量,也叫fps.

Leap Motion 控制器帧刷新速率.

需要注意的是 这个速率**不是**取决于你的应用程序接收帧的速率,也不是你的应用程序处理帧的速率,而是取决于动画的类型或你使用的循环类型,执行的环境,应用程序是否过载等等.

版本 : Leap Motion 1.0.8 and LeapJS 0.3.0

####fingers[][](#fingers[] "Permalink to this definition")
类型 :[Pointable()](Leap.Pointable.html#Pointable "Pointable") 对象的数组,表示此帧中检测到的手指的数据.

此帧中检测到`Finger`对象列表,的列表是乱序的,如果没有对应的对象这个列表也可以是空的.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####gestures[][](#gestures[] "Permalink to this definition")
类型 :[Gesture()](Leap.Gesture.html#Gesture "Gesture") 数组对象

此帧中检测到的`Gesture`对象列表, 列表是乱序的,如果没有对应的对象这个列表也可以是空的.

圈圈手势和滑动手势会在每一帧中不断更新.而点击手势只会存在于某一个帧中的手势列表中.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####hands[][](#hands[] "Permalink to this definition")
类型 :array of [Hand()](Leap.Hand.html#Hand "Hand") objects

此帧中检测到的`Hand`对象列表, 列表是乱序的,如果没有对应的对象这个列表也可以是空的.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####id
类型 :string

此帧的 ID ,每一个ID都是独一无二的. 连续的帧的ID值连续增长.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####interactionBox[](#interactionBox "Permalink to this definition")

类型 :[InteractionBox()](Leap.InteractionBox.html#InteractionBox "InteractionBox") - 当前帧的[InteractionBox()](Leap.InteractionBox.html\#InteractionBox "InteractionBox").

我们需要使用交互盒来矫正 Leap Motion 位置坐标系统 , 使得真实物理上的毫米坐标系统和Leap Motion虚拟的坐标系统匹配.这些匹配化操作后从一侧到另一侧的距离就是代码里面的0到1的距离.

下面的例子示范了如何获得一个点坐标并且将其规范化,按比例缩放后使得使用者的操作区域和html窗口的距离等同:

    <p id="lmCoordinates">...</p>
    <p id="normalizedCoordinates">...</p>
    <p id="windowCoordinates">...</p>

    <script>
    var leapCoordinates = document.getElementById('lmCoordinates');
    var normalizedCoordinates = document.getElementById('normalizedCoordinates');
    var windowCoordinates = document.getElementById('windowCoordinates');

    Leap.loop(function(frame){
     var interactionBox = frame.interactionBox;

     if(frame.pointables.length > 0){
     //Leap coordinates
     var tipPosition = frame.pointables[0].tipPosition;
     leapCoordinates.innerText = vectorToString(tipPosition,1);

     //Normalized coordinates
     var normalizedPosition = interactionBox.normalizePoint(tipPosition, true);
     normalizedCoordinates.innerText = vectorToString(normalizedPosition,4);

     //Pixel coordinates in current window
     var windowPosition = [normalizedPosition[0] * window.innerWidth, 
     window.innerHeight - (normalizedPosition[1] * window.innerHeight), 
     0];
     windowCoordinates.innerText = vectorToString(windowPosition, 0); 
     }
    });

    function vectorToString(vector, digits) {
     if (typeof digits === "undefined") {
     digits = 1;
     }
     return "(" + vector[0].toFixed(digits) + ", "
     + vector[1].toFixed(digits) + ", "
     + vector[2].toFixed(digits) + ")";
    }
    </script>


如果在Leap的控制面板中把`interaction box`的高度设置为 _automatic_ , 那么Leap就会根据用户操作的区域来调整可操作识别的范围和规范化的比例. 这时每一帧中的`InteractionBox`对象就没什么用了.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####pointables[][](#pointables[] "Permalink to this definition")
类型 :[Pointable()](Leap.Pointable.html#Pointable "Pointable")数组对象

当前帧中检测到的指向型对象(手指和工具)列表, 这个列表是乱序的. 如果没有检测到任何指向型物体则这个列表可以是空的.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####timestamp
类型 :number

从Leap启动后到此帧捕获之间的时间,以毫秒为单位.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####tools[][](#tools[] "Permalink to this definition")

类型 :[Pointable()](Leap.Pointable.html#Pointable "Pointable") 表示工具对象的数组列表

列表包含当前帧中被检测到的工具对象,而这些对象是无序的.如果没有检测到任何工具对象这个列表会是空的.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####valid[](#valid "Permalink to this definition")

类型 :boolean

这个属性值表明这个帧实例是否可用.

只有`Controller` 对象生成的`Frame`对象才是可用的帧对象,一个可用的帧对象中会包含所有检测到的实体的数据.而不可用的帧对象中没有相应的追踪数据,但是你可以访问这个valid属性来确认帧是否可用,防止程序出现故障和异常.这种无效帧的机制是为了让我们能更方便地获取到一些帧数据,比如你可以这样调用:

    var finger = controller.frame(n).finger(fingerID);

你可以不用先检查`frame(n)` 是否返回一个空对象,只需要传入一个随意的历史帧数值 "n". (但是你还是应该在finger对象返回后检查其是否是一个有效的对象.)

版本 : Leap Motion 1.0 和 LeapJS 0.3.0


####dump()[](#dump "Permalink to this definition")

返回一个JSON格式的字符串,其中包括了这一帧中关于hands,pointables,gestures的信息

返回值:`String` – 一个JSON格式的字符串.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####finger**(**_id_**)**

当前帧中带有ID的手指对象.

使用`finger()` 函数可以检索寻找返回当前帧中某一个之前帧也存在的手指对象,传入的参数就是ID. 这个函数永远都是返回`pointable`对象,如果找不到对应ID的`pointable`对象则会返回一个无效的`pointable`对象.

需要注意的是不同帧中同一个物体的 ID 值是不变的, 除非这个被检测到的物体离开检测区域. 如果被检测到的物体离开了检测区域,即使这个物体马上回来,也会生成一个新的`pointable`对象,一个新的ID.

Arguments:**id** (_string_) – 需要寻找的`pointable`对象的ID值.


返回值:[Pointable()](Leap.Pointable.html\#Pointable "Pointable") – 返回该帧中对应ID的`pointable`对象,如果没有则返回一个无用的`pointable`对象.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####hand**(**_id_**)**

当前帧中带有ID的`Hand`对象.

使用`hand()` 函数可以检索寻找返回当前帧中某一个之前帧也存在的`Hand`对象,传入的参数就是ID. 这个函数永远都是返回`hand`对象,如果找不到对应ID的`Hand`对象则会返回一个无效的`Hand`对象.

需要注意的是不同帧中同一个物体的 ID 值是不变的, 除非这个被检测到的物体离开检测区域. 如果被检测到的物体离开了检测区域,即使这个物体马上回来,也会生成一个新的`Hand`对象,一个新的ID.

Arguments: **id** (_string_) – 需要寻找的`hand`对象的ID值.

返回值: [Hand()](Leap.Hand.html\#Hand "Hand") – 返回该帧中对应ID的`hand`对象,如果没有则返回一个无用的`hand`对象.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####pointable**(**_id_**)**

当前帧中带有ID的`pointable`对象.

使用`pointable()` 函数可以检索寻找返回当前帧中某一个之前帧也存在的`pointable`对象,传入的参数就是ID. 这个函数永远都是返回`pointable`对象,如果找不到对应ID的`pointable`对象则会返回一个无效的`pointable`对象.

需要注意的是不同帧中同一个物体的 ID 值是不变的, 除非这个被检测到的物体离开检测区域. 如果被检测到的物体离开了检测区域,即使这个物体马上回来,也会生成一个新的`pointable`对象,一个新的ID.

Arguments: **id** (_string_) – 需要寻找的`pointable`对象的ID值.

返回值: [pointable()](Leap.pointable.html\#Hand "pointable") – 返回该帧中对应ID的`pointable`对象,如果没有则返回一个无用的`pointable`对象.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####rotationAngle**(**_sinceFrame_[, _axis_]**)**[](#rotationAngle "Permalink to this definition")

参数帧与当前帧之间旋转轴旋转的角度.

返回的角度值用弧度法(右手顺时针法则)表示了当前帧和目标帧之间的角度,值为0~PI (即0度~180度).

Leap会根据检测范围内所有对象位置和方向的相对变化来确认两帧之间的旋转角度.

如果当前帧或者参数帧是无效的帧对象,那么这个方法会返回的值为0.

参数:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – 传入的帧对象
*   **axis** (_number[]_) – 旋转的参考轴线,用一个三元素的数组表示的向量来表示这条轴线的方向.


返回值:`number` – 表示参数帧与当前帧之间旋转轴旋转的角度,该数字为正数.



版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####rotationAxis**(**_sinceFrame_**)**[](#rotationAxis "Permalink to this definition")

表示当前帧和参数帧之间的轴旋转程度.

返回一个带方向的规范的矢量.

Leap会检查检测范围内所有的对象的位置移动程度和方向移动程度来确认轴的变化程度.

如果当前帧或者参数帧是无效的帧对象,那么这个方法会返回一个零向量.

参数:

  **sinceFrame** ([_Frame_](#Frame "Frame")) – 传入供比对的参数帧对象.

返回值:

`number[]` – 一个向量, 带有三个元素的数组, 代表当前帧与参数帧之间的轴旋转程度



版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####rotationMatrix**(**_sinceFrame_**)**[](#rotationMatrix "Permalink to this definition")

这个转换矩阵表示当前帧和参数帧之间的角度变化矩阵.

Leap会检查检测范围内所有的对象的位置移动程度和方向移动程度来确认角度的变化程度.

如果当前帧或者参数帧是无效的帧对象,那么这个方法会返回一个单位矩阵.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – 传入供比对的参数帧对象.

返回值:

`number[]` – 一个变化矩阵,表示物体从参数`sinceFrame`帧到当前帧包含物体的矢量变化.


版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####scaleFactor**(**_sinceFrame_**)**[](#scaleFactor "Permalink to this definition")

这个方法可以返回当前帧到某一帧之间比例因子的变化值.

而这个值只能是正数,1.0 代表既不放大也不缩小, 0~1 代表不同程度的缩小 , 反之大于1则代表不同程度的放大.

Leap会检查检测范围内所有的对象的放大缩小程度来确认比例因子的变化程度(和位移还有旋转角度没关系).

如果当前帧或者参数帧是无效的帧对象,那么这个方法会返回1.0.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – 目的帧,可以计算当前帧和目的帧的比例因子

返回值:

`number` – 返回一个正的数值来表示当前帧和参数帧的比例因子变化.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####tool**(**_id_**)**

当前帧中带有ID的手指对象.

使用`tool()` 函数可以检索寻找返回当前帧中某一个之前帧也存在的手指对象,传入的参数就是ID. 这个函数永远都是返回`pointable`对象,如果找不到对应ID的`pointable`对象则会返回一个无效的`pointable`对象.

需要注意的是不同帧中同一个物体的 ID 值是不变的, 除非这个被检测到的物体离开检测区域. 如果被检测到的物体离开了检测区域,即使这个物体马上回来,也会生成一个新的`pointable`对象,一个新的ID.

Arguments:**id** (_string_) – 需要寻找的`pointable`对象的ID值.


返回值:[Pointable()](Leap.Pointable.html\#Pointable "Pointable") – 返回该帧中对应ID的`pointable`对象,如果没有则返回一个无用的`pointable`对象.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####toString()[](#toString "Permalink to this definition")

返回一串关于该`Frame`对象的可读的说明字符串.

返回值:`String` – 对于此帧的简明的描述.

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####translation**(**_sinceFrame_**)**

当前帧和传入帧的线性位置变动.

返回的矢量表示了当前帧与参数帧之间的位置变动,以毫米为单位.

Leap会检查检测范围内所有的对象的线性移动程度来确认位置的变化程度.

如果当前帧或者参数帧是无效的帧对象,那么这个方法会返回一个零向量.

参数: **sinceFrame** ([_Frame_](#Frame "Frame")) – 目的帧,可以计算当前帧和目的帧的位置移动大小.

返回值: `number[]` – 一个向量, 带有三个元素的数组, 代表当前帧与参数帧之间的位置变动.



版本 : Leap Motion 1.0 和 LeapJS 0.3.0

####Invalid[](#Invalid "Permalink to this definition")

类型 :[Frame()](#Frame "Frame")-一个无效的`Frame`对象.

你可以使用这个无效的`Frame`对象来检测某个`Frame`对象是否可用. (也可以通过这个JS对象中的 `valid`属性来检查)

版本 : Leap Motion 1.0 和 LeapJS 0.3.0
