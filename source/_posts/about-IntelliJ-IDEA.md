title: 使用IntelliJ IDEA
date: 2015-08-28 10:52:09
categories:
- IDEA
tags:
- IDEA
- Tomcat
- Maven
- Android
---

### 基本知识

* Project是抽象概念，它由Module等其他构件组成。Project类似于Eclipse的workspace
* 创建项目实际上是在Project中创建一个Module。
* 创建Java的项目在IDEA中要选择的是J2EE。
* IDEA执行时有两种类型的Run configuration的，分别是临时和持久的，它们在整个项目中可见。


### 在IDEA中使用Tomcat

1. 关联：需要先把Tomcat关联到IDEA中。在选中file->settings->Application servers->tomcat server后弹出的对话框中，选中tomcat解压的目录即可。
2. 配置持久的Run configuration：在Run->Edit configuration弹出的对话框中，左侧“+”按钮中点击选择添加Tomcat server。一般使用local进行测试。选中之后左边的栏目会增加Tomcat的信息。右侧给它命名即可。
3. 添加需要部署的项目：在部署之前，J2EE Web Module（Dynamic Web项目）需要先配置编译输出路径为自身web/WEB-INF/classes （不知道能不能不要这一步，但是不要这一步我又不知道怎么部署）。对Tomcat来说，所有的项目都叫Atifact。有两种添加方式：
![IDEA中部署项目到Tomcat的方法1](/images/idea_tomcat_add_atifact1.png)
方法1：部署可以再Run configuration配置tomcat的时候，的deployment的标签页里面添加这个项目
![IDEA中部署项目到Tomcat的方法2](/images/idea_tomcat_add_atifact2.png)方法2：在下方的Application Servers中你配置的Local tomcat右键选择Atifact进行配置。配置仅仅是选中项目到部署列表中。
4. 启动：在Application Servers的Local Tomcat server右键点击Run/Connect。


### 使用Maven

1. 创建的时候选择Module为Maven，它会自动创建pom.xml。
2. 在pom.xml里面输入依赖文件，重新reimport就可以自动下载依赖文件并引用。
3. 在部署的时候需要在File->ProjectStructure->Artifacts中添加对应部署项目的Output Layout。如下图：
![Maven项目部署前的配置](/images/maven_project_deploy_prepare.png)
即指明你的classes文件是生成到哪里？需要引用那些外部依赖到lib？一般classes默认已经设置好。需要在WEB-INF中添加lib目录，并把右边对应项目的外部引用jar列表全部移动过来即可。

