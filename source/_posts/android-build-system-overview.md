title: Android build 系统概况
date: 2015-09-08 09:12:49
categories:
- Android
tags:
- Android
---

译：[Android build system overview](http://developer.android.com/sdk/installing/studio-build.html#detailed-build)

Android build系统是用于生成、测试、运行和打包你应用的工具套件。该系统可以作为Android Studio菜单的中间件工具运行，也可以直接使用命令行运行。通过该build系统，你可以：
* 自定义、配置和扩展build流程
* 在使用同样的工程和模块的情况下为你的应用创建多个不同特性的APK
* 在代码层上做到代码和资源的重用
Android build系统的灵活性可以让你在不需要修改应用的核心代码文件的情况下做到上述内容。如何build一个Android Studio项目，参考[ Building and Running from Android Studio](http://developer.android.com/tools/building/building-studio.html)。在Android Studio中如何自定义build系统的配置，参考[Configuring Gradle Builds](http://developer.android.com/tools/building/configuring-gradle.html)。

## 详细的构建过程
build流程中涉及多个工具和程序，它们勇于生成最终在.apk文件中用到的中间文件。如果你使用Android Studio进行开发，那每次你为项目或者模块执行Gradle的build任务时，就会走一遍完整的build流程。build流程非常灵活，然而因为大多数执行流程是可配置和可扩展的，因此理解在底层到底执行了什么动作对你（以后配置或自定义时）很有用。

![Android Apps的生成过程](http://developer.android.com/images/build.png)

一个普通的build流程大概如下面所述。Build系统会合并所有roduct flavors、build types和dependencies生成的资源。如果不同目录包含同名或者同样配置的资源，将会遵循“dependencies覆盖build types，build types覆盖product flavors，product flavors覆盖main source目录”

* Android Asset Packaging Tool(aapt)访问并编译应用资源文件，如AndroidManifest.xml和Activities的XML文件。同时生成一个让你可以在Java代码中引用这些资源的R.java文件。
* aidl工具转换所有的.aidl接口，生成Java interface。
* 你的所有Java代码，包括R.java和.aidl文件，全部被Java编译器编译成.class文件。
* 然后dex工具将所有的.class文件转成Dalvik字节码。所有程序引用的第三方库和.class文件也在这里被转成.dex文件。
* 所有不需要编译的文件（如图片）、编译过的资源、和.dex文件被送到apkbuilder巩固，并打包成.apk文件。
* 一单.apk生成了，它必须使用debug或者release的key进行签名，这样它才能在设备中安装。
* 最后，如果你是使用release模式签名.apk文件，你还必须使用zipalign工具进行整理。这会让最后的.apk在设备中运行时减少内存使用量。

注意：Android应用内部的函数引用数目限制为64K。如果你的应用超过这个限制，build程序将会输出下面的错误信息：
``` 
Unable to execute dex: method ID not in [0, 0xffff]: 65536.
``` 
如何避免这个错误，参考[Building Apps with Over 65K Methods](http://developer.android.com/tools/building/multidex.html)

## 输出结果
build工具针对每个gradle的build生成的APK存放在app/build目录中：app/build/outputs/apk/目录包含以app-&lt;flavor>&gt;-&lt;buildtype&gt;.apk的包；例如，app-full-release.apk或app-demo-debug.apk。
