### ROS基本操作
APT用于下载ROS应用  
https://index.ros.org/  资料查询网站

##### 创建工作空间
1.建立项目结构：
```
mkdir catkin_ws  // 在主文件夹下建立一个项目文件夹
cd catkin_ws   // 进入该文件夹
mkdir src  // 项目源文件的文件夹
cd src
```
2.脚本安装：下载好文件后，在scripts安装需要的脚本，在“scripts”文件夹下安装，例如`./install_for_noetic.sh`  
3.编译：再回到项目文件夹，输入`catkin_make`进行编译  
4.设置空间环境：使用`source 路径/项目文件夹/devel/setup.bash`设置工作空间环境  
5.运行程序：使用`roslaunch wpr_simulation wpb_simple.launch`,其中`wpr_simulation`是包的名称，`wpb_simple.launch`是节点的名称。  
    

**注**：  
在终端使用source只能在当前终端生效，若要所有终端都生效，则要在配置文件中加入设置语句。  
将source指令添加到.bashrc脚本中，这样可以不用重复设置环境(保持谨慎)
```
gedit ~/.bashrc
```
在文件末尾输入
```
source ~/vsworkspace/ros_learning/catkin_ws/devel/setup.bush
```

##### 创建功能包
在src文件夹下，输入指令`catkin_create_pkg package_name depend1 depend2`。  
其中`<package_name>`是包的名称，`depend_x`是依赖项，表示“package_name”功能包依赖于“depend_x”功能包。  
例子`catkin_create_pkg snsr_pkg rospy roscpp std_msgs`  

##### 节点Node
###### 编写与编译
在功能包的src目录下新建文件，进行编写  
在CMakeList.txt中配置编译，如生成exe(可执行文件),在build中找到‘Declare a C++ executable’，将其语句复制到文件末尾  
`add_executable(${PROJECT_NAME}_node src/snsr_pkg_node.cpp)`  
其中`${PROJECT_NAME}_node`是**节点名字**，可以命名为`ultrasonic_node`,与原文件命名一致。  
`src/snsr_pkg_node.cpp`**指定编译文件**，可以命名为`src/ultrasonic_node.cpp`  
依赖包连同编译  
```
target_link_libraries(ultrasonic_node    # 改为节点名字
  ${catkin_LIBRARIES}
)
```


按下`ctrl+shift+B`进行编译,选择"make：bulid"  

###### 运行
1.启动ROS核心，`roscore`  
2.新开终端，加载工作空间`source ~/catkin_ws/devel/setup.bash`
3.`rosrun package_name node_file_name`
`rosrun snsr_pkg ultrasonic_node`

  
注：每次编译产生新文件后需要重新加载工作空间
报错：`Couldn't find executable named`,可能是CMakeList.txt某处的名字打错了  
错误：编译后未能产生覆盖的效果，还是编译之前的模样。可能原因是catkin_ws下有“bulid_isolated”和“devel_isolated”文件夹，将它们删掉。另外编译的时候选择"make：bulid"而不是“make_ioslated:build”  

#### 话题topic
一个话题可以有多个订阅者和发布者  
一个节点可以发布多个话题或订阅多个话题  
同一个时间最多只能有一个发布者  
`rqt_graph` 图形化显示当前节点和话题

##### tips
`roscd package_name`，跳转到包所在的文件夹下  
`code file_name`，用vscode打开目标文件
有.xml的文件夹可以看成是功能包  
`rostopic list`，查看当前活跃话题 
`rostopic echo 主题名称`，查看话题中消息包内容  
`rostopic he 主题名称`，消息包发送频率  


#### launch文件
作用是同时开启多个节点  
先配置工作环境，再输入`roulaunch snsr_pkg r1.launch`,即"roslaunch 包名 launch文件名.launch"  
基本格式如下，type是节点，name是名字  
```xml
<launch>

    <node pkg="snsr_pkg" type="ultrasonic_node" name="ultrasonic_node"/>

    <node pkg="snsr_pkg" type="imu_node" name="imu_node" launch-prefix="gnome-terminal --"/>

    <node pkg="atr_pkg" type="motor_node" name="motor_node" output="screen"/>


</launch>
```
`output="screen"`是将要输出的信息显示到终端
`launch-prefix="gnome-terminal --"`是给指定的节点新开一个终端  

#### 更改功能包的名字
1.修改文件夹的名字（只是为了方便）  
2.修改功能包下package.xml文件，将`<name>`和`<description>`里的名字进行修改  
3.修改CMakeLists.txt文件，修改`project()`的内容  
4.重新编译  


