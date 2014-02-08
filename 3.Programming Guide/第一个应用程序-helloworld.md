# HelloWorld

这篇文章主要讲述Leap SDK里面的Javascript应用如何开发.读完这篇文章你就可以开始着说开发一个支持Leap的web应用程序.

在 Leap SDK 文件夹里,你可以找到下面这个文件,在这篇教学里面会用到:

***LeapSDK/samples/Sample.html*** — JavaScript 示范应用程序

同时你要在[Github上下载Leap Motion的LeapJS库](https://github.com/leapmotion/leapjs "Leapjs").

## 综述

简单来说,Leap Motion设备会检测并跟踪其视野范围内的手和手指.Leap会将这些数据全部放到每个时间点的帧对象里. Web applications can use the Leap Motion JavaScript API to access this data. The Leap Motion controller sends tracking information through the socket connection as a JSON formated message. The JavaScript API takes the JSON message and evaluates it into proper objects.

The sample application demonstrates how to use the Leap Motion JavaScript API. The example is contained in a single Web page, Sample.html. The application displays several properties from the key tracking data objects in the Leap Motion API, including:

+ Frame — contains a set of hand and pointable tracking data
+ Hand — contains tracking data for a detected hand
+ Pointable — contains tracking data for a detected finger or tool
+ Gesture — represents a recognized gesture

## JavaScript 库 (LeapJS)

Leap Motion的Javascript静态库可以直接从上面的Github地址下载.这个库实现了客户端连接Leapmotion服务或者是后台程序的功能,同时也提供了向你的应用程序传递帧数据的功能.
与Leap SDK不同的是,LeapJS是一个开源的项目,我们欢迎大家加入一起开发.

## Setting up the page
使用Javascript API的时候你必须引入Leap.js文件(或者该文件的压缩版本).Sample.html文件用一个简单的HTML结构来演示Leap的数据是如何传递和使用的:

	<html>
	  <head>
	    <title>Leap JavaScript Sample</title>
	    <script src="leap.js"></script>
	    <script>// JavaScript code goes here</script>
	  </head>
	  <body onload="checkLibrary()">
	    <h1>Leap JavaScript Sample</h1>
	    <div id="main">
	      <h3>Frame data:</h3>
	      <div id="frameData"></div>
	      <div style="clear:both;"></div>
	      <h3>Hand data:</h3>
	      <div id="handData"></div>
	      <div style="clear:both;"></div>
	      <h3>Finger and tool data:</h3>
	      <div id="pointableData"></div>
	      <div style="clear:both;"></div>
	      <h3>Gesture data:</h3>
	      <div id="gestureData"></div>
	    </div>
	  </body>
	</html>

##设置 event loop

由于Leap Motion控制器连续不断地提供数据,你需要设置一个event loop来操作每一帧相应的数据.

这个示例
This example demonstrates the second method since there is no need in this app to process the data faster than the browser can draw it to the screen. The Leap Motion API provides the Leap.loop() function, which invokes a callback function you provide whenever the browser is ready to draw. In this sample, the callback is an anonymous function that prints out the tracking data to the body of the web page. The first parameter of the loop() function contains optional settings parameters that are passed to the Controller object. This is the skeleton of the function:

	// Setup Leap loop with frame callback function
	var controllerOptions = {enableGestures: true};
	
	Leap.loop(controllerOptions, function(frame) {
	  // Body of callback function
	})


Leap.loop() 在内部调用了浏览器的 requestAnimationFrame() 方法来循环向应用程序传递当前帧的数据.

##访问跟踪到的数据

帧对象作为帧参数传递到loop()的回调函数上,而这个对象上存储了所有相关的跟踪手护具,并对这些数据分类后方便访问,如手、手指和工具。Sample示例将获取到的数据在页面上显示出来。

###Frame 数据

	var frameString = "Frame ID: " + frame.id  + "<br />"
	                + "Timestamp: " + frame.timestamp + " &micro;s<br />"
	                + "Hands: " + frame.hands.length + "<br />"
	                + "Fingers: " + frame.fingers.length + "<br />"
	                + "Tools: " + frame.tools.length + "<br />"
	                + "Gestures: " + frame.gestures.length + "<br />";
	

###Motion 数据

	// Frame motion factors
	if (previousFrame) {
	  var translation = frame.translation(previousFrame);
	  frameString += "Translation: " + vectorToString(translation) + " mm <br />";
	
	  var rotationAxis = frame.rotationAxis(previousFrame);
	  var rotationAngle = frame.rotationAngle(previousFrame);
	  frameString += "Rotation axis: " + vectorToString(rotationAxis, 2) + "<br />";
	  frameString += "Rotation angle: " + rotationAngle.toFixed(2) + " radians<br />";
	
	  var scaleFactor = frame.scaleFactor(previousFrame);
	  frameString += "Scale factor: " + scaleFactor.toFixed(2) + "<br />";
	}

###Hand 数据

	// Display Hand object data
	var handString = "";
	if (frame.hands.length > 0) {
	  for (var i = 0; i < frame.hands.length; i++) {
	    var hand = frame.hands[i];
	
	    handString += "Hand ID: " + hand.id + "<br />";
	    handString += "Direction: " + vectorToString(hand.direction, 2) + "<br />";
	    handString += "Palm normal: " + vectorToString(hand.palmNormal, 2) + "<br />";
	    handString += "Palm position: " + vectorToString(hand.palmPosition) + " mm<br />";
	    handString += "Palm velocity: " + vectorToString(hand.palmVelocity) + " mm/s<br />";
	    handString += "Sphere center: " + vectorToString(hand.sphereCenter) + " mm<br />";
	    handString += "Sphere radius: " + hand.sphereRadius.toFixed(1) + " mm<br />";
	
	    // And so on...
	  }
	}

###Finger and Tool data

	// Display Pointable (finger and tool) object data
	var pointableString = "";
	if (frame.pointables.length > 0) {
	  for (var i = 0; i < frame.pointables.length; i++) {
	    var pointable = frame.pointables[i];
	
	    pointableString += "Pointable ID: " + pointable.id + "<br />";
	    pointableString += "Belongs to hand with ID: " + pointable.handId + "<br />";
	
	    if (pointable.tool) {
	      pointableString += "Classified as a tool <br />";
	      pointableString += "Length: " + pointable.length.toFixed(1) + " mm<br />";
	      pointableString += "Width: "  + pointable.width.toFixed(1) + " mm<br />";
	    }
	    else {
	      pointableString += "Classified as a finger<br />";
	      pointableString += "Length: " + pointable.length.toFixed(1) + " mm<br />";
	    }
	
	    pointableString += "Direction: " + vectorToString(pointable.direction, 2) + "<br />";
	    pointableString += "Tip position: " + vectorToString(pointable.tipPosition) + " mm<br />";
	    pointableString += "Tip velocity: " + vectorToString(pointable.tipVelocity) + " mm/s<br />";
	  }
	}

###Gesture data

从Leap程序接收回来的数据会先分析有无手势存在,如果你需要程序分析手势的话那么你需要在Leap.loop()的第一个参数对象里将enableGestures设置为true:

	// Setup Leap loop with frame callback function
	var controllerOptions = {enableGestures: true};
	
	Leap.loop(controllerOptions, function(frame) {
	  // Body of callback function
	})

Leap Motion控制器中的Gesture对象包含了每一帧里被识别到的手势数据.下面范例代码用一个标准的for循环和一个switch语句来判断返回的数据中含有什么手势,并将这些信息打印出来.

手势循环的所有代码:
	
	// Display Gesture object data
	var gestureString = "";
	if (frame.gestures.length > 0) {
	  for (var i = 0; i < frame.gestures.length; i++) {
	    var gesture = frame.gestures[i];
	    gestureString += "Gesture ID: " + gesture.id + ", "
	                  + "type: " + gesture.type + ", "
	                  + "state: " + gesture.state + ", "
	                  + "hand IDs: " + gesture.handIds.join(", ") + ", "
	                  + "pointable IDs: " + gesture.pointableIds.join(", ") + ", "
	                  + "duration: " + gesture.duration + " &micro;s, ";
	
	    switch (gesture.type) {
	      case "circle":
	        gestureString += "center: " + vectorToString(gesture.center) + " mm, "
	                      + "normal: " + vectorToString(gesture.normal, 2) + ", "
	                      + "radius: " + gesture.radius.toFixed(1) + " mm, "
	                      + "progress: " + gesture.progress.toFixed(2) + " rotations";
	        break;
	      case "swipe":
	        gestureString += "start position: " + vectorToString(gesture.startPosition) + " mm, "
	                      + "current position: " + vectorToString(gesture.position) + " mm, "
	                      + "direction: " + vectorToString(gesture.direction, 2) + ", "
	                      + "speed: " + gesture.speed.toFixed(1) + " mm/s";
	        break;
	      case "screenTap":
	      case "keyTap":
	        gestureString += "position: " + vectorToString(gesture.position) + " mm, "
	                      + "direction: " + vectorToString(gesture.direction, 2);
	        break;
	      default:
	        gestureString += "unkown gesture type";
	    }
	    gestureString += "<br />";
	  }
	}

##Running the sample

1. 将Leapmotion设备插入USB口并且放在你面前.

2. 如果还没有安装SKD请先安装LeapSDK.

3. LeapMotion的软件会自动运行.    
The Leap Motion 控制面板图标会在通知栏出现,如果可用则会出现为绿色. A service or daemon runs in the background and provides data to client applications through the Leap Motion API. You can use the diagnostic visualizer to check whether the software is set up and running correctly.

4. 在支持 WebSockets 的浏览器里打开 Sample.html.

When you place a hand above the Leap, you should see the Frame, Hand, and Finger information printed out on the page. When you make a gesture, you should also see the appropriate Gesture information.

Now that you have seen how to access motion tracking data with the Leap Motion API, you can begin developing your own JavaScript applications that integrate the Leap Motion controller.