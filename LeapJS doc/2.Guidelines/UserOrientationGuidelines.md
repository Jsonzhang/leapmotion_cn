#用户定位指南

随着我们对程序引入了Leap Motion的控制,我们可以让用户更直接地和电脑进行交互.这是我们有史以来第一次用户可以仅仅用自己的手和手指来控制电脑.

虽然消费者们从前可能对于空间控制已经有一个基本的认识(比如 Microsoft Kinect™ 和 Nintendo Wii™),但,如果让他们每日每夜用这个来操控计算机,这无疑是一个全新的体验.即使你对用户进行适当的引导,这种全新的交互方式也需要时间来习惯,而这种新的交互方式一旦没有得到正确引导可能会使得用户在操作机器时感到沮丧和疑惑.

The following are a few different ways to approach the task of guiding users and teaching them specific interactions within your apps. Whichever you choose, make sure your orientations and tutorials are easily accessible from any point in the app.

##Create a Great Demo Video for Airspace

用户能看到关于你的app的第一个介绍应该是你放在airspace上详情页的截屏和视频,所以强烈建议您应该给您的app配备一段简短的介绍视频,不仅可以展示你的app的功能,也能向用户展示如何操作你的app.下面这几个app视频demo就能很好地告诉用户如何用leapmotion操控即将开始的游戏.

![](https://developer.leapmotion.com/documentation/images/BoomBall_TutorialVideo01.jpg)

[Watch the BoomBall video](https://airspace.leapmotion.com/apps/boom-ball/)

![](https://developer.leapmotion.com/documentation/images/Handwave_TutorialVideo_01.jpg)

[Watch the HandWAVE video](https://airspace.leapmotion.com/apps/handwave/)


![](https://developer.leapmotion.com/documentation/images/Pointables_TutorialVideo_01.jpg)

[Watch the Pointables video](https://airspace.leapmotion.com/apps/pointable/windows)


##交互的‘钥匙’

交互的重点在于app的不同场景中的内容(比如开始体验之前的菜单展示和'help'或者'info'菜单选项的内容)必须是有一致的交互效果. 最好是将所有的东西都在一屏以内显示出来 – 如果你想做好这一步你应该在草稿上安排你需要的内容的相应的动作. 当然如果你的应用操作中加入了一些简单的动画,用户能更容易理解每个不同的操作之间的区别.

![](https://developer.leapmotion.com/documentation/images/DeadMotion.jpg)

[Dead Motion: Prologue](https://airspace.leapmotion.com/apps/dead-motion-prologue/ "Dead Motion: Prologue") displays this thorough help menu to show all of the interactions and optimal Leap Motion controller positioning.


![](https://developer.leapmotion.com/documentation/images/Gravilux_tutorial.jpg)

[Gravilux](https://airspace.leapmotion.com/apps/gravilux/) 在交互方面也做得很好.

虽然你可能有足够的时间来设计自己的手势提示和图标,但是一些现有的设计可能有助于加快你的工作。其中的一些可能是为了特定的手势设计的,但是如果你想重新设计,可以以此为蓝图.

----------


+ [Gesturecons.com](http://www.gesturecons.com/)
+ [Gestureworks.com](http://www.gestureworks.com/): tap on the link for Individual Gesture Icons – Vector and Bitmap to download
+ [MobileTuxedo.com](http://www.mobiletuxedo.com/)

##Walk-Through Tutorials
A Walk-Through Tutorial is a series of screens, directions or interactive tasks that lead new users through each different part of your app. Often times, breaking more complicated interactions down into discrete parts can be a great way to introducing new users to the mechanics of your software. And, as always, letting users learn by doing is a great way to help them master controls in a safe environment and ultimately bolster their confidence as they dive into your app.

![](https://developer.leapmotion.com/documentation/images/DecoSketch001.jpg)

[Watch the BoomBall video](https://airspace.leapmotion.com/apps/boom-ball/)

![](https://developer.leapmotion.com/documentation/images/DecoSketch002.png)

[Watch the HandWAVE video](https://airspace.leapmotion.com/apps/handwave/)

![](https://developer.leapmotion.com/documentation/images/DecoSketch003.jpg)

[Watch the Pointables video](https://airspace.leapmotion.com/apps/pointable/windows)

This walk-through tutorial by [Deco Sketch](https://airspace.leapmotion.com/apps/deco-sketch/) shows how the user’s finger position over the Leap Motion Controller changes the interaction from “Menu” to “Draw” modes.
##In-App Cues / Hints

Giving hints or cues to a user while they are interacting with your software can be an elegant way to subtly direct behavior. Simple contextual graphics paired with instructions (e.g. a pointing hand + “Tap to Select”) can go a long way in guiding users in the right direction. It is important, however, to make sure that these cues are as unobtrusive as possible and only support the experience vs. distracting from it.

![](https://developer.leapmotion.com/documentation/images/JungleJumper_Tutorial02.jpg)

**[Jungle Jumper](https://airspace.leapmotion.com/apps/jungle-jumper/) displays in-app cues as well as a small HUD in the lower right corner to show you real-time feedback of your hand.**

------------------

在Jungle Jumper的右下角有一个平视显示器(HUD), 当用户在玩游戏时给一些额外的提示gives the user additional feedback as to their hand position and state. While not necessary for every app, this sort of visual feedback can be very helpful in describing to the user the extent of the Leap Motion interaction zone and what is being detected by the device.

![](https://developer.leapmotion.com/documentation/images/WoodenSensei_Tutorial02.jpg)

每次你得到一个新的技能, [Wooden Sen’SeY](https://airspace.leapmotion.com/apps/wooden-sen-sey/) 都会在游戏内显示一个提示教你如何使用.

##控制演示实例

Live controls in the tutorials provides an experiential approach to teaching interactions by requiring users to perform certain simple tasks in menu selections or discreet tutorial mode. This is truly ‘learning by doing’ and can be a creative way to reinforce gestures that are core to your app. Depending upon the complexity of your application, providing these interactive tutorials or a practice mode might be worthwhile.

![](https://developer.leapmotion.com/documentation/images/Handwave_Tutorial02.jpg)

HandWAVE provides a comprehensive interactivetutorial.

![](https://developer.leapmotion.com/documentation/images/Cyberscience_Tutorial.jpg)

Cyber Science 3D Motion 里配置有一段交互式教学教你如何操作这个app.

-------------

无论你最后在你自己的应用程序里面放置哪种教学指导方式,以身换位假设如果你是用户你会需要什么.As you can see from some of the above examples, additionally including some indication as to the best position of the user’s hands in relation to the Leap Motion Controller (this may differ depending on the application) can be extremely helpful.

##Help When and Where You Need It
Lastly, be sure that you enable the user to intuitively access these tutorials and help screens from anywhere within your application. We highly recommend that a tutorial is shown upon the very first entry into the app. Within the app, the tutorials could be accessed via an explicit and persistent “Help” or “?” icon or via the main menu, if applicable.

No matter how simple your app may be, a little help can go a long way. Set your users up to get the most out of your app and they will become your biggest fans.