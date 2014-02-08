#Menu Design Guidelines

可以让用户更加直接地选择和操作  无论是某个特定的菜单或者只是在一个经验——开发深度应用程序是一个极其重要的组成部分,确保最终用户控制。更重要的是,很多时候这些是第一个互动的人将会与您的软件。如果用户最初的经验基本菜单选择是混乱,令人沮丧或费力,他们可能会放弃之前的经验使您的应用程序的核心。显然这是值得花一些时间来得到这个权利!
Presenting users with intuitive options and controls – whether in the form of dedicated menus or simply throughout an experience – is an incredibly important component of developing deep applications and ensuring users ultimately feel in control. What’s more, many times these are the first interactions someone will have with your software. If a user’s initial experience with basic menu selection is confusing, frustrating or laborious, they might give up on the experience even before getting to the heart of your application. Clearly it’s worth spending some time to get this right!



Yet while some existing best practices can be easily applied to Leap Motion-enabled apps, there’s no reason to think that what works best in current desktop or mobile menus will be immediately transferable to three dimensional interactions. In working through some of these issues ourselves and seeing solutions from developer community we’ve established a few guidelines to consider when implementing menus and settings into your app experiences.

Menu Interactions
Interacting with a menu are actions that are often repeated and likely to be gating factors to other functionality – so clearly they need to feel natural, be incredibly reliable and not require too much of the user in learning a new or complicated sign language. To avoid user frustration, it is always better to err on the side of basic yet stable approaches while continuing to refine more innovative ones.

Menu Design and Layout
However you organize your menu to accommodate your experience and artwork, keep the usability, legibility and simplicity of interaction in mind. Be sure to space the buttons appropriately so that its easy for a user to select and tap a particular button without accidentally mis-tapping another.

image0
In this example menu you can see a number of best practices at work.
Buttons are large, well organized and include a clear highlight/depressed state
Buttons use high contrast colors and the text/font are very legible
The Exit button is easily accessible and clearly indicated
Required gestures are displayed using easy to read iconography. The recommended gesture for menus is the touch-zone or “poke”.
Proximity-Based Highlighting
Another way of simplifying the menu experience for your users is to provide a proximity-based highlighting scheme. This would highlight the closest item to the user’s cursor, without having to actually be over it. In the example below, the five possible actions are outlined to show how this might work. The user’s cursor is in the upper left quadrant so therefore the Play button is lit. Performing a tap gesture would activate the Play button. Anticipating what the user might want in these contexts can save time and eliminate frustration.

image1
Example of Proximity-Based Highlighting Scheme
Combining layout and proximity highlighting can result in some easy to use, not to mention great looking, Leap Motion enabled menus. Here are some examples using a radial and grid based menu generator we’ve provided to help you understand and interact with these concepts. These examples are using the tap-zone API (z-axis poke) for the activation method. Additionally they include clear visual feedback when highlighting and selecting each cell as well as audio feedback as you navigate from cell to cell and upon cell selection. Note the change in the cursor state as you translate the z-axis, showing your proximity to the “tap”.

Radial Menus
The following radial menus show how you can navigate a number of items quickly and accurately by arranging their layout in a ring or pie. The movement required to target one of the cells is extremely slight which reduces the amount of latency and physical strain. The menus with a central cell could use that area for the most important item in the menu or as a “neutral zone”.

You can find the live code example here.

 
5 Cell Radial Menu with Center Target	 
6 Cell Symmetrical Radial Menu
 
7 Cell Radial Menu with Center Target	 
4 Cell Symmetrical Radial Menu with 2 Columns
Grid Menus
Similar to the radial menus, these grid based menus provide a simple method of accessing items with a minimum of effort, but using a column/row/grid based layout.

You can find the live code example here.

 
6 Cell Symmetrical Row Menu	 
6 Cell Symmetrical Column Menu
 
9 Cell Symmetrical Grid Menu	 
5 Cell Row Menu with Right Zone
Menu Access and Exit
With this in mind, accessing these menus as well as exiting your application should be handled in the same, simple and foolproof manner.

For most games and applications, removing your hands from the field of view should pause the interaction and display a “Continue, Main Menu, Quit” dialog
Providing an explicit “Settings” or “Menu” button is another option
There should be an explicit “Exit” or “Quit” button
The Escape key should exit the app (on Mac and Windows)
The Command-Q (Mac) or Alt-F4 (Window) should also exit
May want to also make your menus accessible via mouse as well
image18
The first-person shooter, Dead Motion Prologue, displays this menu when a user removes their hands from the field of view.
Feedback. Feedback. Feedback.
For any selection approach you utilize, giving users proper cues and feedback is integral to ensuring they feel in complete control. It should first be immediately clear what elements are interactive – and it never hurts to give users unobtrusive graphical or textual cues, e.g. a simple illustration and “Tap to select an article.”

Once interacting with the element, it should respond fluidly with appropriate visual and auditory feedback. Buttons should be highlighted when hovered over and should respond with a “click” and indent as they are depressed; sliders should move freely; etc. The more information you can give to help orient the user and signal their selections, the easier it will be for them to complete each task.

image19
Frog Dissection uses large buttons that highlight and magnify when indicated and a simple z-axis poke gesture to tap/select.
image20
Sky Muffins uses buttons that “fill up” as you push forward on the z-axis. Filling the button selects it. This can be done quickly based on the speed of your push.
As recommended previously, these two applications use the Leap Motion Touch Zone API to handle the button “tap”. For more on the implementation of this API, please review this section of our documentation and this excellent blog post from one of our developers on how to implement touch zones.