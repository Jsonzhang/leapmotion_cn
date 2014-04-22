# Frame[](\#frame "Permalink to this headline")

##_class _Frame()[](#Frame "Permalink to this definition")

The Frame class represents a set of hand and finger tracking data
detected in a single frame.

The Leap detects hands, fingers and tools within the tracking area,
reporting their positions, orientations and motions in frames at the
Leap frame rate.

Frame instances created with a constructor are invalid. Get valid
Frame objects by calling the Controller.frame() function.

###Attribute
--------------------------------
####currentFrameRate[](#currentFrameRate "Permalink to this definition")
类型 :float – frames per second

Leap Motion 控制器帧刷新速率.

需要注意的是 这个速率**不是**取决于你的应用程序接收帧的速率,也不是你的应用程序处理帧的速率,而是取决于动画的类型或你使用的循环类型,执行的环境,应用程序是否过载等等.

版本 : Leap Motion 1.0.8 and LeapJS 0.3.0

####fingers[][](#fingers[] "Permalink to this definition")
类型 :array of [Pointable()](Leap.Pointable.html#Pointable "Pointable") objects representing fingers

`Finger`对象的列表 objects detected in this frame, given in arbitrary
order. The list can be empty if no fingers are detected.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####gestures[][](#gestures[] "Permalink to this definition")
类型 :array of [Gesture()](Leap.Gesture.html#Gesture "Gesture") objects

The list of Gesture objects detected in this frame, given in arbitrary
order. The list can be empty if no gestures are detected.

Circle and swipe gestures are updated every frame. Tap gestures only
appear in the list for a single frame.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####hands[][](#hands[] "Permalink to this definition")
类型 :array of [Hand()](Leap.Hand.html#Hand "Hand") objects

The list of Hand objects detected in this frame, given in arbitrary
order. The list can be empty if no hands are detected.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####id
类型 :string

A unique ID for this Frame. Consecutive frames processed by the Leap
have consecutive increasing values.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####interactionBox[](#interactionBox "Permalink to this definition")
类型 :[InteractionBox()](Leap.InteractionBox.html#InteractionBox "InteractionBox")

The current [InteractionBox()](Leap.InteractionBox.html\#InteractionBox "InteractionBox") for the frame.

Use the interaction box to normalize Leap Motion position coordinates
from physical millimeters to dimensionless coordinates. These
normalized, dimensionless coordinates range from 0 at one side of the
interaction box to 1 at the opposite side.

The following example illustrates how to get take a point, normalize it,
and then scale it so that the interaction area maps to the inner dimensions
of an HTML window:

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


When interaction box height is set to _automatic_ in the Leap Motion control panel,
the position and size
of the interaction box can change as the user moves his or her hands within
the Leap Motion field of view. Thus a new `InteractionBox` object is included
in each frame.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####pointables[][](#pointables[] "Permalink to this definition")
类型 :array of [Pointable()](Leap.Pointable.html#Pointable "Pointable") objects

The list of Pointable objects (fingers and tools) detected in this
frame, given in arbitrary order. The list can be empty if no fingers or
tools are detected.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####timestamp
类型 :number

The frame capture time in microseconds elapsed since the Leap started.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####tools[][](#tools[] "Permalink to this definition")
类型 :array of [Pointable()](Leap.Pointable.html#Pointable "Pointable") objects representing tools

The list of Tool objects detected in this frame, given in arbitrary
order. The list can be empty if no tools are detected.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####valid[](#valid "Permalink to this definition")
类型 :boolean

Reports whether this Frame instance is valid.

A valid Frame is one generated by the Controller object that contains
tracking data for all detected entities. An invalid Frame contains no
actual tracking data, but you can call its functions without risk of a
undefined object exception. The invalid Frame mechanism makes it more
convenient to track individual data across the frame history. For
example, you can invoke:

    var finger = controller.frame(n).finger(fingerID);

for an arbitrary Frame history value, “n”, without first checking
whether frame(n) returned a null object. (You should still check that
the returned Finger instance is valid.)

版本 : Leap Motion 1.0 and LeapJS 0.3.0

</div>

####dump()[](#dump "Permalink to this definition")

Returns a JSON-formatted string containing the hands, pointables and
gestures in this frame.

Returns:`String` – A JSON-formatted string.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####finger**(**_id_**)**

The finger with the specified ID in this frame.

Use the Frame finger() function to retrieve the finger from this frame
using an ID value obtained from a previous frame. This function always
returns a Finger object, but if no finger with the specified ID is
present, an invalid Pointable object is returned.

Note that ID values persist across frames, but only until tracking of a
particular object is lost. If tracking of a finger is lost and
subsequently regained, the new Pointable object representing that
physical finger may have a different ID than that representing the
finger in an earlier frame.

Arguments:

*   **id** (_string_) – The ID value of a finger from a previous frame.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

[Pointable()](Leap.Pointable.html\#Pointable "Pointable") – The finger with the matching ID if one exists in this frame; otherwise,
an invalid Pointable object is returned.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####hand**(**_id_**)**

The Hand object with the specified ID in this frame.

Use the Frame hand() function to retrieve the Hand object from this
frame using an ID value obtained from a previous frame. This function
always returns a Hand object, but if no hand with the specified ID is
present, an invalid Hand object is returned.

Note that ID values persist across frames, but only until tracking of a
particular object is lost. If tracking of a hand is lost and
subsequently regained, the new Hand object representing that physical
hand may have a different ID than that representing the physical hand in
an earlier frame.

Arguments:

*   **id** (_string_) – The ID value of a Hand object from a previous frame.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

[Hand()](Leap.Hand.html\#Hand "Hand") – The Hand object with the matching ID if one exists in this frame;
otherwise, an invalid Hand object is returned.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####pointable**(**_id_**)**

The Pointable object with the specified ID in this frame.

Use the Frame pointable() function to retrieve the Pointable object from
this frame using an ID value obtained from a previous frame. This
function always returns a Pointable object, but if no finger or tool
with the specified ID is present, an invalid Pointable object is
returned.

Note that ID values persist across frames, but only until tracking of a
particular object is lost. If tracking of a finger or tool is lost and
subsequently regained, the new Pointable object representing that finger
or tool may have a different ID than that representing the finger or
tool in an earlier frame.

Arguments:

*   **id** (_string_) – The ID value of a Pointable object from a previous frame.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

[Pointable()](Leap.Pointable.html\#Pointable "Pointable") – The Pointable object with the matching ID if one exists in this frame;
otherwise, an invalid Pointable object is returned.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####rotationAngle**(**_sinceFrame_[, _axis_]**)**[](#rotationAngle "Permalink to this definition")

The angle of rotation around the rotation axis derived from the overall
rotational motion between the current frame and the specified frame.

The returned angle is expressed in radians measured clockwise around the
rotation axis (using the right-hand rule) between the start and end
frames. The value is always between 0 and pi radians (0 and 180
degrees).

The Leap derives frame rotation from the relative change in position and
orientation of all objects detected in the field of view.

If either this frame or sinceFrame is an invalid Frame object, then the
angle of rotation is zero.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – The starting frame for computing the relative rotation.
*   **axis** (_number[]_) – The axis to measure rotation around expressed as a directionvector stored in a 3-element array of numbers.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

`number` – A positive value containing the heuristically determined rotational
change between the current frame and that specified in the sinceFrame
parameter.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####rotationAxis**(**_sinceFrame_**)**[](#rotationAxis "Permalink to this definition")

The axis of rotation derived from the overall rotational motion between
the current frame and the specified frame.

The returned direction vector is normalized.

The Leap derives frame rotation from the relative change in position and
orientation of all objects detected in the field of view.

If either this frame or sinceFrame is an invalid Frame object, or if no
rotation is detected between the two frames, a zero vector is returned.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – The starting frame for computing the relative rotation.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

`number[]` – A normalized direction vector (as a 3-element array) representing the axis of the heuristically
determined rotational change between the current frame and that
specified in the sinceFrame parameter.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####rotationMatrix**(**_sinceFrame_**)**[](#rotationMatrix "Permalink to this definition")

The transform matrix expressing the rotation derived from the overall
rotational motion between the current frame and the specified frame.

The Leap derives frame rotation from the relative change in position and
orientation of all objects detected in the field of view.

If either this frame or sinceFrame is an invalid Frame object, then this
method returns an identity matrix.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – The starting frame for computing the relative rotation.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

`number[]` – A transformation matrix containing the heuristically determined
rotational change between the current frame and that specified in the
`sinceFrame` parameter.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####scaleFactor**(**_sinceFrame_**)**[](#scaleFactor "Permalink to this definition")

The scale factor derived from the overall motion between the current
frame and the specified frame.

The scale factor is always positive. A value of 1.0 indicates no scaling
took place. Values between 0.0 and 1.0 indicate contraction and values
greater than 1.0 indicate expansion.

The Leap derives scaling from the relative inward or outward motion of
all objects detected in the field of view (independent of translation
and rotation).

If either this frame or sinceFrame is an invalid Frame object, then this
method returns 1.0.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – The starting frame for computing the relative scaling.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

`number` – A positive value representing the heuristically determined scaling
change ratio between the current frame and that specified in the
`sinceFrame` parameter.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####tool**(**_id_**)**

The tool with the specified ID in this frame.

Use the Frame tool() function to retrieve a tool from this frame using
an ID value obtained from a previous frame. This function always returns
a Pointable object, but if no tool with the specified ID is present, an
invalid Pointable object is returned.

Note that ID values persist across frames, but only until tracking of a
particular object is lost. If tracking of a tool is lost and
subsequently regained, the new Pointable object representing that tool
may have a different ID than that representing the tool in an earlier
frame.

Arguments:

*   **id** (_String_) – The ID value of a Tool object from a previous frame.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

[Pointable()](Leap.Pointable.html\#Pointable "Pointable") – The tool with the matching ID if one exists in this
frame; otherwise, an invalid Pointable object is returned.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####toString()[](#toString "Permalink to this definition")

A string containing a brief, human readable description of the Frame
object.

Returns:`String` – A brief description of this frame.

版本 : Leap Motion 1.0 and LeapJS 0.3.0

####translation**(**_sinceFrame_**)**

The change of position derived from the overall linear motion between
the current frame and the specified frame.

The returned translation vector provides the magnitude and direction of
the movement in millimeters.

The Leap derives frame translation from the linear motion of all objects
detected in the field of view.

If either this frame or sinceFrame is an invalid Frame object, then this
method returns a zero vector.

Arguments:

*   **sinceFrame** ([_Frame_](#Frame "Frame")) – The starting frame for computing the relative translation.
</td>
</tr>
<tr class="field-even field"><th class="field-name">Returns:

`number[]` – A vector, expressed as a 3-element array, representing the heuristically determined change in position of
all objects between the current frame and that specified in the `sinceFrame` parameter.



版本 : Leap Motion 1.0 and LeapJS 0.3.0

####Invalid[](#Invalid "Permalink to this definition")
类型 :[Frame()](#Frame "Frame")

An invalid Frame object.

You can use this invalid Frame in comparisons testing whether a given
Frame instance is valid or invalid. (You can also check the
js:attr:<cite>valid</cite> property.)

版本 : Leap Motion 1.0 and LeapJS 0.3.0

 </div>