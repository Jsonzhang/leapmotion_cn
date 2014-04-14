#API参考

API参考提供的leapmotion所有类的具体实现。

<table border="0" class="table">
<colgroup>
<col width="47%">
<col width="53%">
</colgroup>
<tbody valign="top">
<tr class="row-odd"><td><a class="reference internal" href="Leap.CircleGesture.html"><em>CircleGesture</em></a></td>
<td><a class="reference internal" href="Leap.KeyTapGesture.html"><em>KeyTapGesture</em></a></td>
</tr>
<tr class="row-even"><td><a class="reference internal" href="Leap.Controller.html"><em>Controller</em></a></td>
<td><a class="reference internal" href="Leap.Pointable.html"><em>Pointable</em></a></td>
</tr>
<tr class="row-odd"><td><a class="reference internal" href="Leap.Frame.html"><em>Frame</em></a></td>
<td><a class="reference internal" href="Leap.ScreenTapGesture.html"><em>ScreenTapGesture</em></a></td>
</tr>
<tr class="row-even"><td><a class="reference internal" href="Leap.Gesture.html"><em>Gesture</em></a></td>
<td><a class="reference internal" href="Leap.SwipeGesture.html"><em>SwipeGesture</em></a></td>
</tr>
<tr class="row-odd"><td><a class="reference internal" href="Leap.Hand.html"><em>Hand</em></a></td>
<td><a class="reference internal" href="Leap.Matrix.html"><em>Matrix math</em></a></td>
</tr>
<tr class="row-even"><td><a class="reference internal" href="Leap.InteractionBox.html"><em>InteractionBox</em></a></td>
<td><a class="reference internal" href="Leap.Vector.html"><em>Vector math</em></a></td>
</tr>
</tbody>
</table>

##Leap 命名空间

`Leap` 在 Leap API 里是一个全局的命名空间. 除了API里面的类之外,Leap这个命名空间包含了以下这些函数:

1. **Leap.loop(options, callback)**               
loop() 这个函数其实是一个定期执行的函数,里面设置好了程序对Leap控制器的连接,以及WebSocket连接和回调函数. 使用这个方法之后你就不用再自己实现你自己的程序控制器了.                         
默认情况下上面这个定期执行函数的回调函数调用间隔取决于浏览器本身,正常情况下应该是60帧/秒这个频率. 最新一帧里面的leap检测到数据会从参数传入到你的回调函数里面. 如果Leap产生数据的速度比浏览器刷新屏幕的速度要慢,那么Leap也会在连续动画更新的时候将同步的数据传回到回调函数中.                     
如果你准备好处理这些数据或者设置你自己的回调函数的时候,你也可以选择建立你自己的Controller()对象和控制器框架.

Arguments:	

 + options (Object) –
 Controller的配置对象,包括以下这些参数:
   + host — WebSocket服务器的主机名或者IP地址,让Leap Motion可以为其提供运动追踪的数据. 通常来说, 也可以设置本地IP地址: 127.0.0.1.
   + port —  WebSocket 服务器的监听端口. 默认端口是 6437.
   + enableGestures — 设置为True表明应用支持手势. 当然如果你的应用程序不使用手势那么你应该设置为false,默认是false.
   + frameEventName — 传递帧数据的类型.                       
      + animationFrame 表示使用浏览器默认的60帧/秒  ,默认使用的是animationFrame.                
      + deviceFrame 表示使用leap motion的硬件频率(从 20 到 200 fps 不等,取决于使用者的设置和电脑的性能).
   + useAllPlugins - 默认为true.这个选项告诉控制器是否使用页面内所有的插件. 关于这个选项更详细的内容可以参见 [https://github.com/leapmotion/leapjs/wiki/plugins](https://github.com/leapmotion/leapjs/wiki/plugins).
   + loopWhileDisconnected — 默认是 true, 如果设置为true意味着任何时候都会运行这个循环. 而如果你设置为false的话表明 If you set this to false, the animation loop does only runs when the Controller() object is connected to the Leap Motion service and only when a new frame of data is available. Setting loopWhileDisconnected to false can minimize your app’s use of computing resources, but may irregularly slow down or stop any animations driven by the frame loop. Added in LeapJS version 0.4.3.
   + callback (function) –
   当浏览器绘制完成之后的回调函数.包含最新的检测到的运动数据的Frame对象会作为参数传递给你的回调函数.

			var controller = Leap.loop({enableGestures:true}, function(frame){
			    var currentFrame = frame();
			    var previousFrame = controller.frame(1);
			    var tenFramesBack = controller.frame(10);
			});

Returns:	
Controller – the controller supplying tracking data to this loop function.