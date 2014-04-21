# Controller[](\#id30 "Permalink to this headline")

##_class _Controller()[](#Controller "Permalink to this definition")

`Controller` 类是Leap Motion Controller的主要控制接口.

实例化得到一个`Controller`的对象,通过这个对象我们才能获取和操纵到机器追踪的数据. 任何时间使用frame() 函数就能获得帧里的运动数据. 调用 frame() 或者 frame(0)就可以拿到最新一帧的数据. 将传入的参数设置为正数可以得到更早之前的帧的数据,一个控制器对象里最多存储60个帧数据.

创建完一个 `Controller` 对象后你可以选择性地传入一些控制器的参数:

    var controller = new Leap.Controller({
     host: '127.0.0.1',
     port: 6437,
     enableGestures: true,
     frameEventName: 'animationFrame',
     useAllPlugins: true
    });


Leap Motion 控制器类继承于 Node.js 的_`EventEmitter` <http://nodejs.org/api/events.html>_ 类.

####参数:

*   **options** (_Object_) – 控制器的参数对象,包含各个具体参数:

    *   `host` — WebSocket服务的主机名或者IP地址.通常这里设置的应该是本地地址(localhost): `127.0.0.1`.
    *   `port` — WebSocket 服务侦听的端口. 默认端口是 `6437`.
    *   `enableGestures` — 设置为`true` 则开启手势识别. 如果你的应该不需要手势则该选项忽略不设置或者设置为 `false` .
    *   `frameEventName` — 用于处理帧数据的循环类型.
        *   `animationFrame` 使用浏览器动画循环(默认为 60 fps).
        *   `deviceFrame` 使用 Leap Motion 控制器的帧速率更新 (取决于用户设置和电脑性能,从20到200fps不等).
    *   **`useAllPlugins`** - 默认为`false`.让控制器使用页面上载入的所有插件.详见:<https://github.com/leapmotion/leapjs/wiki/plugins>.
    *   `loopWhileDisconnected` — 默认是`true`, 设置为`true`意味着即使leap设备连接断开,程序主循环也会不断执行. 如果设置为 `false`, 则动画循环只会在[Controller()](#Controller "Controller") 对象能连接到 Leap Motion 服务,能传来新一帧的数据时才会执行. 设置 `loopWhileDisconnected` 为 `false` 能降低你的应用对于设备资源的消耗,但是也有可能因此帧循环的动画有时会变得卡顿,视具体情况设置.

参数设置在 LeapJS version 0.4.3 后加入.


##frameEventName[](#frameEventName "Permalink to this definition")

类型 :string –  `"animationFrame"` 或 `"deviceFrame"`

这个属性用于在控制器创建时读取循环的类型,所以对于一个已存在的控制器对象没有必要去修改这个属性的值.

##connect()[](#connect "Permalink to this definition")

让控制器对象和Leap Motion WebSocket 服务器端建立起连接. 如果连接成功则控制器开始追踪数据,这时可以使用`frame()`或者[on()](\#on "on")函数来控制追踪的函数.

你可以用 on() 函数注册一个回调函数来让程序在连接成功时提醒你.

返回值:[Controller()](#Controller "Controller")

版本 : Leap Motion 1.0 和 LeapJS 0.3.0

##connected()[](#connected "Permalink to this definition")

验证控制器对象和Leap Motion WebSocket服务器是否连接成功.

返回值:true or false

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##disconnect()[](#disconnect "Permalink to this definition")

与WebSocket服务器端断开连接. 可以调用connect()来重连.

返回值:[Controller()](#Controller "Controller")

##frame([_history_])

从 Leap 返回某一帧的数据.

使用可选的`history`参数可以获得你想要的那一帧数据.调用 frame() 或者 frame(0) 来操作最新的帧; 调用 frame(1) 来进入之前那一帧,以此类推. 当然如果你是用的参数值大于缓存中的帧数,那么控制器就会返回一个无效的帧对象.而当你使用 `animationFrame`  循环模式时, 每个循环只有一帧会被存储起来.在循环间隙发生的帧刷新的数据都会被丢弃掉.

参数:

*   **history** (_int_) – 需要返回之前的第几帧, 传入的应该是一个 0 ~ 200 的数字. 而现在缓存中存储的最大数量帧数据还是一个未明确的事情,很有可能在未来修改,现在暂定为200.

返回值:[Frame()](Leap.Frame.html\#Frame "Frame") – 返回某一帧,如果没有传入参数则返回最新的一帧.如果没有设置这个参数则返回最新的一帧. 如果参数无效或者该帧丢失则会返回一个帧对象, 但是这个对象是无效的.

版本:Leap Motion 1.0 和 LeapJS 0.3.0

##inBrowser()[](#inBrowser "Permalink to this definition")

这个函数用来测试应用当前是否在浏览器环境中运行.

返回值 : `True` 则是在浏览器中运行; `false` 则是在node平台运行.

版本 : Leap Motion 1.0.8 和 LeapJS 0.3.0

##on(_eventName_, _callback_)[](#on "Permalink to this definition")

用这个控制器对象给已经绑定的事件增加一个回调函数.通过 Node.js 的`EventEmitter`定义,更多详见`emitter.on` <http://nodejs.org/api/events.html\#events_emitter_on_event_listener>.你也可以使用其他事件注册类,这里不再赘述.

参数:

*   **eventName** (_string_) – 值是下面九个中的一个:

    *   blur
    *   connect
    *   deviceConnected
    *   deviceDisconnected
    *   disconnect
    *   focus
    *   frame
    *   gesture
    *   protocol

*   **callback** (_function_) – 回调函数,会在事件被侦听到执行时被触发. 如果是协议,帧和手势的侦听事件回调函数需要带参数,其他的回调函数则不需要.

        var controller = new Leap.Controller();
        controller.connect();
        controller.on('frame', onFrame);
        function onFrame(frame)
        {
         console.log("Frame event for frame " + frame.id);
        }

返回值 : [Controller()](\#Controller "Controller")



版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##streaming()[](#streaming "Permalink to this definition")

验证控制器是否正从 Leap Motion 服务接收追踪到的数据.

返回值:true or false

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##setBackground(_state_)[](#setBackground "Permalink to this definition")

告诉 Leap Motion WebSocket 服务端你的应用程序需要时刻接收到追踪数据,即使是你的应用程序在后台或者没有被激活的时候你都需要这些数据. 需要注意的是设置这个选项为`true`可能会导致应用程序接收到一些意想不到的输入,因为即使用户在操控其他应用程序时你的应用程序都会收到用户输入的追踪数据.大多数应用程序不应该设置这个选项.


参数:

*   **state** (_boolean_) – 设置为 `True` 来随时随地接收所有帧数据; 否则为`false` (默认)则只在程序激活时接收数据.

返回值:[Controller()](\#Controller "Controller")

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##blur[](#blur "Permalink to this definition")

当浏览器页面失去焦点时触发. 通常来说这个函数是在用户在一个浏览器里面切换tab页时触发,而不是在用户切换到另一个浏览器或者其他windows窗口时触发.

    var controller = new Leap.Controller();
    controller.connect();

    controller.on('blur', onBlur);
    function onBlur()
    {
     console.log("Blur event");
    }


版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##connect

当控制器与Leap Motion WebSocket 服务器端建立起连接的时候触发.

这个事件的触发并不意味着数据可用,因为Leap Motion可能会被用户手动暂停,或设备本身不插电.


    var controller = new Leap.Controller();
    controller.connect();

    controller.on('connect', onConnect);
    function onConnect()
    {
     console.log("Connect event");
    }

返回值 :[Controller()](#Controller "Controller")

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##deviceAttached[](#deviceAttached "Permalink to this definition")

当Leap Motion设备插入,打开的时候触发.

所有 `device` 事件都包含有一个设备信息对象:

* attached 
    * 类型 : boolean
    * 值 :  true or false

* streaming
    * 类型 : boolean
    * 值 :  true or false

* id 
    * 类型 : string
    * 值 : the device hardware ID

* type 
    * 类型 : string
    * 值 : 下面值其中一个:
        *   peripheral
        *   keyboard
        *   laptop
        *   unknown
        *   unrecognized

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##deviceConnected[](#deviceConnected "Permalink to this definition")

在Leap Motion 设备插件和用户手动确认继续跟踪数据时触发.

需要注意的是这个事件在控制器启动你的应用开始后不会触发.

    var controller = new Leap.Controller();
    controller.connect();

    controller.on('deviceConnected', onDeviceConnected);
    function onDeviceConnected(evt)
    {
     console.log("Device Connect event");
    }

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##deviceDisconnected[](#deviceDisconnected "Permalink to this definition")

当用户手动断开Leap Motion设备或者设备没电断开时触发.

    var controller = new Leap.Controller();
    controller.connect();

    controller.on('deviceDisconnected', onDeviceDisconnect);
    function onDeviceDisconnect()
    {
     console.log("Device disconnect event");
    }


版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##deviceRemoved[](#deviceRemoved "Permalink to this definition")

Leap Motion 设备断开时触发.

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##deviceStopped[](#deviceStopped "Permalink to this definition")

Leap Motion 设备停止提供数据时触发.

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##deviceStreaming[](#deviceStreaming "Permalink to this definition")

Leap Motion 设备开始提供数据时触发.

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##disconnect

Leap Motion服务和控制器断开连接时触发.


    var controller = new Leap.Controller();
    controller.connect();

    controller.on('disconnect', onDisconnect);
    function onDisconnect()
    {
     console.log("Disconnect event");
    }

返回值 :[Controller()](#Controller "Controller")

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##focus[](#focus "Permalink to this definition")

浏览器获得焦点时触发.


    var controller = new Leap.Controller();
    controller.connect();

    controller.on('focus', onFocus);
    function onFocus()
    {
     console.log("Focus event");
    }


版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##frame

每次循环不断触发的函数.

如果`frameEventName`的值是 `animationFrame`,则帧速率会被设置为浏览器动画循环帧速率,这个值通常是60帧/秒.因为浏览器的动画循环速度和Leap Motion设备的刷新速度不一致,所以在接收数据时你可能活有好几帧都接收到一样的数据或者是有某一帧数据被跳过了这样的情况发生.

如果把 `frameEventName` 设置为 `deviceFrame`,这个事件会在每一帧Leap Motion数据接收到的时候触发.但是设备的帧速率又取决于用户的设置和用户电脑的性能.

设置控制器里的帧循环的类型.

最新的[Frame](Leap.Frame.html\#Leap.Frame "Leap.Frame")对象会被传入到你的回调函数中.


    var controller = new Leap.Controller();
    controller.connect();

    controller.on('frame', onFrame);
    function onFrame(frame)
    {
     console.log("Frame event for frame " + frame.id);
    }

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##gesture[](#gesture "Permalink to this definition")

当手势更新时触发.

手势相关的[Gesture](Leap.Gesture.html\#Leap.Gesture "Leap.Gesture")对象和当前[Frame](Leap.Frame.html\#Leap.Frame "Leap.Frame") 对象也会传入到你的回调函数中.

当控制器开始执行默认的 `animationFrame` 循环时,  在`animationFrame`循环间隔中检测到的手势 都会被存贮起来,并在下一个间隔中删除. 这意味着传进你的手势侦听函数的帧对象并不一定总是那个侦听到了手势的帧对象. 手和手指的位置还有其他物理信息如果能形成一个手势则在帧与帧的变化中一定能检测到.在某些情况下, 有着相同ID的手或者其他指向性的对象如果被检测出来手势,这些相同ID的对象可能不会在所有帧里都被传入到你的回调函数中.

    var controller = new Leap.Controller({enableGestures:true});
    controller.connect();

    controller.on('gesture', onGesture);
    function onGesture(gesture,frame)
    {
     console.log(gesture.type + " with ID " + gesture.id + " in frame " + frame.id);
    }

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##protocol[](#protocol "Permalink to this definition")

侦听WebSocket服务器端和客户端的通讯协议建立完成这个动作.

一个`Protocol`对象会被传入到你的回调函数中. 使用这个对象的 `version` 或者 `versionLong` 属性可以确实该协议是否正被使用,如果用户使用旧版的Leap Motion 硬件的话这个值可能会比请求协议的版本号更低.

    var controller = new Leap.Controller();
    controller.connect();

    controller.on('protocol', onProtocol);
    function onProtocol(protocol)
    {
     console.log("Protocol event: " + protocol.version);
    }

版本 : Leap Motion 1.0.9 和 LeapJS 0.3.0

##streamingStarted[](#streamingStarted "Permalink to this definition")

侦听 Leap Motion 服务/后台进程开始提供数据这个动作.

版本 : Leap Motion 1.2 和 LeapJS 0.5.0

##streamingStopped[](#streamingStopped "Permalink to this definition")

侦听 Leap Motion 服务/后台进程停止提供数据这个动作.

版本 : Leap Motion 1.2 和 LeapJS 0.5.0


##plugin(_pluginName_, _factory_)[](#plugin "Permalink to this definition")

注册一个插件,让其可以通过`controller.use()` 来调用.

工厂函数返回的对象的有效值包括 `frame`, `hand`, `finger`, `tool`, 和 `pointable`.每一个键对应的值都可以是一个对象或者是一个函数.如果传入一个函数作为键的值,那么这个函数每次调用都应该返回一个新的实例, 那么这个返回的示例才是真正传入plugin函数的参数. 这样做的好处是我们可以在返回的对象上添加我们想要的额外的数据:


    Leap.Controller.plugin('testPlugin', function(options){
     return {
     frame: function(frame){
     frame.foo = 'bar';
     }
     }
    });


当 `hand`被调用时,回调函数会调用`frame.hands`中的所有hand对象.需要注意的是hand对象在每一个新的帧中都会被重新创造; 所以存储在hand中的数据是不能跨帧保存的.

    Leap.Controller.plugin('testPlugin', function(){
     return {
     hand: function(hand){
     console.log('testPlugin running on hand ' + hand.id);
     }
     }
    });


工厂模式会返回一个可以向`Frames`, `Hands`,或者 `Pointables` 添加自定义功能的对象.返回的对象如果需要某个特定的方法,可以直接在对应的类的原型中添加.在这里应该用`Pointable`来代替`Finger` 和 `Tool` 类.
当然也不是任何一帧都需要用工厂模式来运算获得实例.
我们也鼓励开发者使用[Memoization](http://en.wikipedia.org/wiki/Memoization), 因为有的情况下一个方法确实会在同一帧内被多次调用:

    // 这个插件允许 hand.usefulData() 稍后被调用.
    Leap.Controller.plugin('testPlugin', function(){
     return {
     hand: {
     usefulData: function(){
     console.log('usefulData on hand', this.id);
     // memoize the results onto the hand, preventing repeat work:
     this.x || this.x = someExpensiveCalculation();
     return this.x;
     }
     }
     }
    });

需要注意的是需要用工厂模式为每个插件封装实例:


    Leap.Controller.plugin('testPlugin', function(options){
     options || options = {}
     options.center || options.center = [0,0,0]

     privatePrintingMethod = function(){
     console.log('privatePrintingMethod - options', options);
     }

     return {
     pointable: {
     publicPrintingMethod: function(){
     privatePrintingMethod();
     }
     }
     }
    });

参数:

*   **pluginName** (_string_) – 插件名.
*   **factory** (_function_) – 构建函数,返回插件的实例.工厂函数本身可以通过`controller.use()`接受一个可选的哈希形式的参数..


版本 : Leap Motion 1.0 和 LeapJS 0.4.0

##use(_pluginNameOrFactory_[, _options_])[](#use "Permalink to this definition")

开始使用一个注册插件. 插件会被添加到所有帧中, 会返回一个控制器实例和帧里指定的对象.

*   插件执行顺序取决于 `use` 的触发顺序.
*   插件在`deviceFrames` 和 `animationFrames`都能运行.

参数:

*   **pluginNameOrFactory** (_string_) – 注册插件的名字. 插件工厂也能直接用于Node环境.
*   **options** (_hash_) – 将插件传递给插件工厂时附带的参数.

返回值:[Controller()](\#Controller "Controller")

版本 : Leap Motion 1.0 和 LeapJS 0.4.0

##stopUsing(_pluginName_)[](#stopUsing "Permalink to this definition")

停止使用某个插件. 会将每一帧中的这个插件函数移除,同时也会移除从这个插件的类继承拓展的所有方法.

参数:

*   **pluginName** (_string_) – 插件的注册名.

返回值:[Controller()](\#Controller "Controller")


版本 : Leap Motion 1.0 和 LeapJS 0.4.0
