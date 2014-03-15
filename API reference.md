API Reference

The API Reference provides details on all the classes which make up the Leap Motion API.

CircleGesture	KeyTapGesture
Controller	Pointable
Frame	ScreenTapGesture
Gesture	SwipeGesture
Hand	Matrix math
InteractionBox	Vector math
Leap namespace
Leap is the global namespace of the Leap API. In addition to the classes in the API, the Leap namespace contains the following function:

Leap.loop(options, callback)
The loop() function sets up the Leap controller and WebSocket connection and invokes the specified callback function on a regular update intervall. You do not need to create your own controller when using this method.

By default, your callback function is called on an interval determined by the client browser. Typically, this is on an interval of 60 frames/second. The most recent frame of Leap data is passed to your callback function. If the Leap is producing frames at a slower rate than the browser frame rate, the same frame of Leap data can be passed to your function in successive animation updates.

As an alternative, you can create your own Controller() object and either poll the controller for frames when you are ready to process them or set up the callback loops yourself.

Arguments:	
options (Object) –
An object containing the option values for this Controller:
host — The host name or IP address of the WebSocket server providing Leap Motion tracking data. Usually, this is the local host address: 127.0.0.1.
port — The port on which the WebSocket server is listening. By default, this is port 6437.
enableGestures — Set to true to enable gesture recognition. Omit or set to false if your application does not use gestures.
frameEventName — The type of update loop to use for processing frame data. animationFrame uses the browser animation loop (generally 60 fps), while deviceFrame runs at the Leap Motion controller frame rate (about 20 to 200 fps depending on the user’s settings and available computing power). By default, loop() uses the animationFrame update mechanism.
useAllPlugins - True by default. This tells the controller to use all plugins included on the page. For more information, see https://github.com/leapmotion/leapjs/wiki/plugins.
callback (function) –
A function called when the browser is ready to draw to the screen. The most recent Frame object is passed to your callback function.
var controller = Leap.loop({enableGestures:true}, function(frame){
    var currentFrame = frame();
    var previousFrame = controller.frame(1);
    var tenFramesBack = controller.frame(10);
});
Returns:	
Controller – the controller supplying tracking data to this loop function.