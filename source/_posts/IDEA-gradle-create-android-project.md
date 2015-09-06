title: 使用IntelliJ-IDEA 创建基于Gradle管理的android项目
date: 2015-09-06 15:58:41
categories:
- Android
tags:
- IDEA
- Gradle
- Android
---

刚接触Gradle和IDEA。仅以本文档记录使用IDEA创建基于Gradle的Android项目的过程中遇到的问题与解决方法。

![IDEA创建基于Gradle的Android项目](/images/idea_create_gradle_android_project.png)

请原谅本文不会教你具体如何创建。如果您需要具体的步骤，请参考下面附带的链接。我个人的学习方式：一般我会先搜索一篇教程类的博文

* [如何使用IDEA和Gradle创建Android项目](http://www.tuicool.com/articles/a2MNna)

然后到官网了解进一步内容

* [IDEA官文：关于Android-Gradle](http://www.jetbrains.com/idea/help/android-gradle.html)
* [IDEA官文：使用Gradle创建Android的详细步骤](http://www.jetbrains.com/idea/help/creating-a-gradle-android-project.html)
* [IDEA官文：Gradle配置页面介绍](http://www.jetbrains.com/idea/help/gradle-2.html)

阅读上面几个链接中的内容，如果环境正确的话应该是能够正常使用了。


因为在搭建过程中因为我的Android SDK的build-tool太老，导致出现了很多问题。这些问题解决的比较慢的原因是对这些工具都比较陌生，也正是因为陌生才需要总结与备忘。

我的配置如下：
1. IntelliJ-IDEA版本：14.1.4
2. Android SDK: platform为API-18、build-tool为18.1.0
3. Gradle版本：2.6
4. IntelliJ-IDEA内置的Gradle插件版本：显示N/A

配置过程中出现的问题我按顺序罗列下来。

##### 1. 网络问题
解决方法：必然是翻墙

##### 2. 正确执行完创建Android项目的步骤，IDEA卡死
原因：以为IDEA内置了Gradle，因此没有下载安装Gradle。IDEA内置的仅仅是Gradle的插件。
解决方法：下载Gradle，解压并配置其中的bin目录到Path环境变量中。

##### 3. 提示Error:The SDK Build Tools revision (18.1.0) is too low for project ':xxx'. Minimum required is 19.1.0
原因：Android SDK的Build tool版本太老。IDEA版本决定了Gradle插件的版本，Gradle插件决定了build-tool的版本，build-tool和Gradle插件决定Gradle的版本。
解决方法：
1. 下载build-tool，解压到SDK的build-tool目录。我使用SDK Manager.exe翻墙下载还是失败。直接下载zip包却能成功。这里给个链接吧[android-sdk_r24.3.4-windows.zip](http://dl-ssl.google.com/android/repository/build-tools_r23.0.1-windows.zip)
2. 在创建了项目之后，修改项目下面的build.gradle文件中的buildToolsVersion为你所解压的那个工具包的版本，如我的"23.0.1".

##### 4. 提示Can not resolve: com.android.support:appcompat-v7:18.+
原因：在IDEA生成的项目build.gradle中下面这一段，其左右不是太清楚，大概是包含libs/*.jar和一个外部对Android support库的依赖。
``` 
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:18.+'	// 有问题的地方
}
``` 
解决方法：
1. 注释掉这一句应噶是没问题的。
2. 考虑到support包还是比较常用，这里有两个解决方法。A:查看sdk目录下extras\android\m2repository\com\android\support中有哪个版本的support包，然后修改上述语句的版本信息。B:直接让IDEA安装。IDEA在扫描到问题时会在message窗口给出解决方法。点击安装就可以了。



