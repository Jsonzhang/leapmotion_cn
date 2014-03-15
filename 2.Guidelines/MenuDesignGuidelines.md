#菜单设计指南



无论从菜单的易用性上还是只是整个程序的使用体验来说,向用户提供更直观的控制方式确保最终用户简单地进行控制,这对于开发完善的程序来说是极其重要的。

更需要注意的是,很多时候这些人是第一次与你的软件进行互动,如果连在菜单上的简单交互体验都觉得非常混乱和费力,他们很可能就会开始不喜欢你的软件,甚至不会再使用你的软件了.
所以菜单设计方面显然需要花更多的心思来做得更好!

尽管到目前为止很多已有的最佳实践可以应用到leap motion apps的开发上, 但是这仍然没有让我们认为现在已经可以把桌面和移动菜单的交互转移到三维上,使用leapmotion来进行所有的交互.

所以当你在设计实现你的程序菜单时如果遇到交互问题,可以参考我们在开发者社区中建立的一些指南来增强您的应用程序的体验。


##菜单交互


使用菜单这个行为会在我们的应用程序中经常被重复,因为这是可能是所有功能连结的中介.因此菜单的交互需要让用户感觉更加自然和可靠,不需要让大多数用户为了操作去学习复杂的手势。为了避免用户使用菜单时出现沮丧情绪,宁可用最基础可靠的方法也不要用太过新颖独特的创意.


##菜单设计和布局




无论如何,您必须让你的菜单保持可用性、易读性和简单性的交互来适应你的作品.在不同的按钮中间必须留有空隙方便用户选择而不会很容易就错误点击到另一个.

![](https://developer.leapmotion.com/documentation/images/menu_left_align_withInfo.jpg)

在这个示例菜单中你可以看到工作中的最佳实践应该是什么样子的.




1. 应该保持按钮足够大,易于辨认而且有一个高亮/被点击的状态
2. 高亮的按钮可以使用对比(反向)颜色使得按钮中的文字更加清晰易读.
3. exit按钮在菜单界面中应该特别容易被找到.
4. 如果菜单使用了手势,那么这个手势必须非常简单,推荐是在区域中设置简单的敲击.






##亲近高亮法

另一种可以简化你的菜单设计的方法是你对菜单提供一种基于亲近而选择高亮的主题.这样做可以让离指针最近的菜单选项高亮,而不用必须将指针保持在选项上.在下面这个例子中
In the example below, the five possible actions are outlined to show how this might work. The user’s cursor is in the upper left quadrant so therefore the Play button is lit. Performing a tap gesture would activate the Play button. Anticipating what the user might want in these contexts can save time and eliminate frustration.

![](https://developer.leapmotion.com/documentation/images/Menus_Zones.jpg)

Example of Proximity-Based Highlighting Scheme
Combining layout and proximity highlighting can result in some easy to use, not to mention great looking, Leap Motion enabled menus. Here are some examples using a radial and grid based menu generator we’ve provided to help you understand and interact with these concepts. These examples are using the tap-zone API (z-axis poke) for the activation method. Additionally they include clear visual feedback when highlighting and selecting each cell as well as audio feedback as you navigate from cell to cell and upon cell selection. Note the change in the cursor state as you translate the z-axis, showing your proximity to the “tap”.

##Radial Menus
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