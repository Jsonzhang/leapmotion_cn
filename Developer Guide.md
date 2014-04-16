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

All distances are expressed in millimeters, as floating point numbers. If you want to get relative (percent) measurements, use the InteractionBox.normalizePoint method. See the Leap.InteractionBox documentation for axis orientation. Coordinates for positions and directions are expressed as array objects containing three values ([x, y, z]). You can use the glMatrix library to perform vector and matrix math with these arrays.

All angles are measured in Radians. To convert to degrees, multiply by 180/Math.PI.

All time/timestamp measurements are given in microseconds; 1,000,000 microseconds = 1 second. Timestamps are relative measures of time since the Leap Motion software initialized. Subtract one timestamp from another to calculate the time that has elapsed between them.

Frame indexes reflect the LIFO nature of the Leap.Controller's frames collection.

my_controller.frames(0)  == my_controller.frames() == the most recent frame.
my_controller.frames(1) is the previous frame
my_controller.frames(2) is two frames ago
and so on. Keep in mind that frames are constantly being pushed onto the stack, so my_controller.frames(2) will be a different frame in a few milliseconds. When in doubt use frame.id to identify/compare frames.

All positions and directions are returned as an array of three coordinates (see above). See Leap.InteractionBox for methods on normalizing and denormalizing coordinates.

scaleFactor

Leap.Hands and Leap.Frames have scaleFactor functions. Scale factor indicates a change in distance between things over time. For hands, this means the change in distance between the fingers. The function has a parameter: a baseline frame.

Scale factor for hands
Jazz_hands
Hand scale factor reflects the relative "Jazz handiness" of your fingers, compared with the baseline spread in the reference frame.

If you pinch all your fingers together since the reference frame, your hand's scaleFactor will be < 1.
If you expand ("jazz hands") your fingers farther apart, your hands' scaleFactor will be > 1.
Scale factor for frames
Jazz_arms
When frame.scaleFactor(old_frame) is called, it reflects the relative distance between the hands -- the "fish measuring factor".

If you move your hands apart, the scale factor will be > 1
If you move your hands together, the scale factor will be < 1
If Leap cannot relate the baseline reference with the current frame/hand data, the scaleFactor will be 1.

Valid and Invalid function return results
Any function which returns an instance object (Pointable, Frame, Hand,...) will ALWAYS return an object, even under conditions where returning an object is impossible (bad parameters, your hands aren't in front of the detector, etc.). Instead of returning false, null, or throwing an error, an invalid instance will be returned.

This lets you access child objects, such as fingers, without first checking that the frame exists, and that the hand exists in the frame, and so on, but you should examine the valid property of the final, target object before using its properties and methods. The valid property is a boolean and all Leap instances (except Gesture objects) have one.

Leap Methods
Leap.Loop(callback)

This static method of the Leap global object fires repeatedly, approximately sixty times a second. See Getting Frame Data from your Leap Motion controller for other ways of accomplishing data monitoring.

The Callback receives a single Leap.Frame instance.

For additional information, see Leap.loop().

Leap Classes
Leap.Frame

A collection of state information fed back from the Leap Motion hardware. A frame is the "root" data unit; it contains all the positional data that streams from the Leap Motion detector, Fingers/Tools/Pointables at a given instant in time.

See Getting Frame Data from your Leap Motion controller for documentation on getting frames.

A note on Frame.Fingers, Frame.pointables, and Frame.tools:

Pointables
frame.fingers, frame.tools and frame.pointables are different collections of instances which are all instances of the Leap.Pointable base class. (there is no special Leap.Tools or Leap.Fingers class -- just Leap.Pointable.)

The pointables collections contains all the pointables also found in tools and fingers.
The tools collections and the fingers collections are exclusive; there is no pointer which can be found in both the fingers collection and the tools collection
All pointables in these root collections can be found in the frame.hands properties.
Any of these collections can be empty, if the user's hands/fingers/tools aren't picked up by the Leap Motion detector.
All fingers/tools/pointables from both hands are stored with no ordering in these root level collections.
Frame Properties:
currentFrameRate
fingers[]
hands[]
id
interactionBox
Invalid
valid
pointables[]
timestamp
tools[]
Frame Functions:
dump()
finger()
hand()
pointable()
rotationAngle()
rotationAxis()
rotationMatrix()
scaleFactor()
tool()
toString()
translation()

For more information, see Frame in the API reference.

Leap.Pointable

The Pointable class represents detected finger or tool **. Both fingers and tools are classified as Pointable objects. Use the Pointable.tool property to determine whether a Pointable object represents a tool or finger Note that Pointable objects can be invalid, which means that they do not contain valid tracking data and do not correspond to a physical entity. Invalid Pointable objects can be the result of asking for a Pointable object using an ID from an earlier frame when no Pointable objects with that ID exist in the current frame.

** The Leap classifies a detected entity as a tool when it is thinner, straighter, and longer than a typical finger.

Pointable Properties:
direction
id
Invalid
length
stabilizedTipPosition
timeVisible
tipPosition
tipVelocity
tool
touchDistance
touchZone
valid
width
Pointable Functions:
hand()
toString()

For more information, see Pointable in the API reference.

Leap.Controller

Represents the connection to a single Leap detector. You can have multiple controllers in a single run time. Note that Leap.Controller is a JavaScript class; whereas the Leap Motion controller refers to an actual piece of hardware -- the box.

The code below creates a Leap.Controller, with an option parameter object specifying to enable gestures and to use the browser animation loop:

  var controller = new Leap.Controller({
  enableGestures: true,
  frameEventName: 'animationFrame'
});
Any or all these properties are optional; you can also create a controller with a simpler parameter set:

  var controller = new Leap.Controller({enableGestures: true});
Controller Events
connect
The client is connected to the WebSocket server.
frame
An iteration of the frame loop is starting. This event is either driven by the animationFrame or the deviceFrame event, depending on how the Leap.Controller was created. The latest frame is passed as an argument to the event handler. This frame may or may not be a new frame.
gesture
Dispatched for each gesture object in a new frame of data received from the WebSocket.
disconnect
The client disconnects from the WebSocket server.
focus
The browser received focus.
blur
The browser loses focus.
deviceConnected
A Leap device has been connected.
deviceDisconnected
A Leap device has been disconnected.
protocol
The protocol has been selected for the connection. The protocol object is passed as an argument to the event handler.
Here is a typical initialization cycle for interacting with a Leap Motion controller:

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

Controller Properties:
frameEventName
Controller Functions:
connect()
disconnect()
frame()
inBrowser()
on()
plugin()
setBackground()
stopUsing()
use()

For more information, see Controller in the API reference.

Leap.Hand

The Hand class reports the physical characteristics of a detected hand. Hands are accessed as properties of the Leap.Frame's hand array; at this point, up to two hands can be simultaneously tracked and returned with frame data. Hand tracking data includes a palm position and velocity; vectors for the palm normal and direction to the fingers; properties of a sphere fit to the hand; and lists of the attached fingers and tools.

See the Leap.Frame class documentation for the difference between Pointables, Fingers and Tools.

The below illustration shows the hand's palmNormal and direction vectors emanating from the palmPosition:

Palm Vectors 
Hand Properties:
direction
fingers[]
id
Invalid
valid
palmNormal
palmPosition
palmVelocity
pointables[]
sphereCenter
sphereRadius
stabilizedPalmPosition
timeVisible
tools[]
Hand Functions:
finger()
pitch()
roll()
rotationAngle()
rotationAxis()
rotationMatrix()
scaleFactor()
toString()
translation()
yaw()

For more information, see Hand in the API reference.

Leap.InteractionBox

A representation of the "airspace" in which the Leap Motion controller can measure/see your hands and fingers. Note that the range of the interaction box may change with hardware or software advances. Use the interaction boxes' properties to scale your measurements -- don't hard code these values in your JavaScript.

The Axes' orientation
Leap_axes_annotated

The x-axis goes to the right. 0 on the x axis is the middle of the Leap Motion controller.
The y-axis goes up, from the Leap Motion controller; you will likely have to negate it to make it consistent with screen/DOM graphic coordinates. 0 on the y axis is very close to the Leap Motion detector.
The z-axis points towards you. 0 on the z axis is right above the detector. Your monitor will be at -Z, your head will have a +Z measurement.

InteractionBox Properties:
center
depth
height
Invalid
valid
width
InteractionBox Functions:
denormalizePoint()
normalizePoint()
toString()

For more information, see InteractionBox in the API reference.

Gesture

The Gesture class represents a recognized movement by the user. The Leap watches the activity within its field of view for certain movement patterns typical of a user gesture or command. For example, a movement from side to side with the hand can indicate a swipe gesture, while a finger poking forward can indicate a screen tap gesture. When the Leap recognizes a gesture, it assigns an ID and adds a Gesture object to the frame gesture list. For continuous gestures, which occur over many frames, the Leap updates the gesture by adding a Gesture object having the same ID and updated properties in each subsequent frame. Important: Recognition for each type of gesture must be enabled; otherwise no gestures are recognized or reported. Subclasses of Gesture define the properties for the specific movement patterns recognized by the Leap.

The Gesture subclasses for include:

CircleGesture
A circular movement by a finger
SwipeGesture
A straight line movement by the hand with fingers extended.
ScreenTapGesture
A forward tapping movement by a finger.
KeyTapGesture
A downward tapping movement by a finger.
Number of gestures produced

Circle and swipe gestures are continuous and these objects can have a state of start, update, and stop.

The screen tap gesture is a discrete gesture. The Leap only creates a single ScreenTapGesture object appears for each tap and it always has a stop state.

Getting Gesture instances from a Frame object.

You can get a list of gestures from the Frame gestures array. You can also use the Frame gesture() method to find a gesture in the current frame using an ID value obtained in a previous frame. Gesture objects can be invalid.

For more information, see the following classes in the API reference:

Gesture
CircleGesture
KeyTapGesture
ScreenTapGesture
SwipeGesture