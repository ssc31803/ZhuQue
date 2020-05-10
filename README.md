# ZhuQue
朱雀是中国古代四大神兽之一，希望大家有兴趣的话可以了解一些古代神话故事，非常有想象力。

Simply speaking, ZhuQue supports Hot Reload of Objective-C code during iOS development.
ZhuQue works on iOS simulators and real iOS devices.
With ZhuQue, you can run your latest code without having to rebuilding and rerunning, which may costs you plenty of time. 

ZhuQue works based on the interpretive execution of LLVM IR code generated from Objective-C source file. 
LLVM has an built-in interpreter called lli. Unfortunately, lli does not fully support IR code from Objective-C.
ZhuQue embeds a customized LLVM IR interpreter built on lli.

Please notice that by Hot Reload and method swizzling, you actually change the behavior of a class, not a specific object. 
If you change the properties of an existing class, you need to recreate an object of the reloaded class instead of using the former one, otherwise memory issues or crashes will occur.

Feature:
1. Work on both simulator and real device.
2. Basic block support.
3. Both instance method and class method can be updated.
4. Propertied can be added to an existing class.
5. Methods can be added to an existing class.
6. If you change the code of the topmost ViewController, and if you implement a method named - (void)dynamicUpdate], this method will be called each time you change code.

Drawback:
1. You can only update an existing class rather than adding a new class.
2. Only Objective-C classes can be updated for now.
3. General c struct is not supported, except CGPoint, CGSize and CGRect.
4. The execution speed of IR code is much slower than normal, but must faster than rebuilding and rerunning.
5. Due to reasonable and unreasonable issues, there will be some memory leaks during the execution of IR code for now.
6. NSLog has not been supported yet.
7. If you use dispatch_once, static variable or other similar things, using ZhuQue may break the original code logic if you continue using an existing object whose class has been updated.
8. Xcode pch has not been supported yet.

Steps:
1. Install ZhuQue on your Mac.
2. Make sure your Mac and your device on the same LAN.
3. Open ZhuQue, then you can see the [ZQ] item in the status bar.
4. Click [ZQ] and select [Open…].
5. Chosse the path of your project. Please notice that this path should contain a valid xcodeproj or xcworkspace file.
6. Click [Project Path] and add directories that contain all code of your project if your project uses code placed somewhere else.
7. Emben ZhuQue.framework and libLLVM.dylib in your project. You can find these frameworks under /Applications/Zhuque.app/Contents/Resources after installing.
8. Call [[ZhuQue sharedInstance] showStartPopup] in didFinishLaunchingWithOptions.
9. Build your project and run the relative app on your device.
10. Click [ZQ] and select [Broadcast].
11. Click the [Connnect] button in your app.
12. Begin to work as normal without Command+R.

Warning:
1. Do not embed ZhuQue in release binaries, otherwise your app may be rejected by App Store. ZhuQue is made for developing and debugging.
2. If your app crash or work abnormally, stop using ZhuQue and wait for the next update.
3. If you meet linking errors caused by CocoaAsyncSocket, use ZhuQue(Clean).framework instead of ZhuQue.framework.


For example, if your app can presents a ViewController named BViewController from AViewController, and you want to modify the behavior of BViewController, you can change the implementation of BViewController at first and then present BViewController again after popping back to AViewController on the device. Or you can just implement a - (void)dynamicUpdate] method and make you modifications inside the method. In short, it is needed that the modified code has chance to be executed.
