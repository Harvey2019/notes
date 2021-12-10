#### ROS参数

* 所有参数  $ printenv | grep ROS  
* 功能包路径  $ echo $ROS_PACKAGE_PATH
* ROS参数介绍http://wiki.ros.org/Parameter%20Server
* C++操作参数http://wiki.ros.org/roscpp/Overview/Parameter%20Server



#### 常用指令

* 软件包路径

```
$ rospack find [package_name]
```

* 切换到软件包路径或软件包集的子目录

```
$ roscd [locationname[/subdir]]
```

* 查看软件包目录下文件

```
$ rosls [locationname[/subdir]]
```

* 查看节点

```
$ rosnode list
$ rosnode info /rosout
$ rosnode ping my_turtle
```

* 运行节点

```
$ rosrun [package_name] [node_name]
```

* 话题

```
#查看
$ rostopic echo [topic]
$ rostopic list -h

#消息类型
$ rostopic type [topic]
#消息详细
$ rosmsg show geometry_msgs/Twist

#发布消息
$ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
$ rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'

#话题发布频率
$ rostopic hz /turtle1/pose
```

* 服务

```
$ rosservice list
$ rosservice type [service]
$ rosservice call [service] [args]
$ rosservice type /spawn | rossrv show
```

* 参数

```
rosparam set            设置参数
rosparam get            获取参数
rosparam load           从文件中加载参数
rosparam dump           向文件中转储参数
rosparam delete         删除参数
rosparam list           列出参数名
```

#### 软件包

* workspace结构

```
workspace_folder/        -- WORKSPACE
  src/                   -- SOURCE SPACE
    CMakeLists.txt       -- 'Toplevel' CMake file, provided by catkin
    package_1/
      CMakeLists.txt     -- CMakeLists.txt file for package_1
      package.xml        -- Package manifest for package_1
    ...
    package_n/
      CMakeLists.txt     -- CMakeLists.txt file for package_n
      package.xml        -- Package manifest for package_n
```

* 创建

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
```

```
$ cd ~/catkin_ws/src
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
$ catkin_make
$ . ~/catkin_ws/devel/setup.bash
```

* 查看软件包依赖

```
$ rospack depends1 beginner_tutorials 
$ rospack depends beginner_tutorials
```

* 编译

```
# 在catkin工作空间下
$ catkin_make
$ catkin_make install  # （可选）

使用catkin_make install，在完成devel后，将Cmakelist声明的文件安装到install文件中
```

```
# 在catkin工作空间下 代码存在my_src下
$ catkin_make --source my_src
$ catkin_make install --source my_src  # （可选）
```

```
#仅对个别包编译
$ catkin_make -DCATKIN_WHITELIST_PACKAGES="package1;package2"
#回退编译
$ catkin_make -DCATKIN_WHITELIST_PACKAGES=""
```

#### Workspace文件

* 官网介绍http://wiki.ros.org/catkin/workspaces

* build: 构建空间的默认位置，同时`cmake`和`make`也是在这里被调用来配置和构建你的软件包

* devel: 开发空间的默认位置, 在安装软件包之前，这里可以存放可执行文件和库
* src: 源空间的默认位置存放软件包，源码
* install: 默认安装到/usr/local，创建一个可以运行，但不含源码的工程包，以便于给客户等其他人使用，但同时不至于泄露源码，结构与devel类似



#### 非标准msg的使用

* 确保msg文件能被转换为C++、Python和其他语言的源代码

```
#在package.xml中
<build_depend>message_generation</build_depend>
 <exec_depend>message_runtime</exec_depend>
  
  #在cmakelist.txt中
  find_package(catkin REQUIRED COMPONENTS
	...
    message_generation
)

catkin_package(
  ...
  CATKIN_DEPENDS message_runtime ...
  ...)
  
  add_message_files(
  FILES
  Num.msg
   )
   
   generate_messages(
  DEPENDENCIES
  std_msgs
)
```



#### 非标准srv的使用

* 确保srv文件能被转换为C++、Python和其他语言的源代码

```
#在package.xml中
<build_depend>message_generation</build_depend>
 <exec_depend>message_runtime</exec_depend>

  #在cmakelist.txt中
  find_package(catkin REQUIRED COMPONENTS
	...
    message_generation
)

catkin_package(
  ...
  CATKIN_DEPENDS message_runtime ...
  ...)

add_service_files(
  FILES
  AddTwoInts.srv
)

   generate_messages(
  DEPENDENCIES
  std_msgs
)
```

* `msg`目录中的任何`.msg`文件都将生成所有支持语言的代码。C++消息的头文件将生成在`devel/include/beginner_tutorials/`。Python脚本将创建在`devel/lib/python2.7/dist-packages/beginner_tutorials/msg`。而Lisp文件则出现在`devel/share/common-lisp/ros/beginner_tutorials/msg/`。 类似地，`srv`目录中的任何`.srv`文件都将生成支持语言的代码。对于C++，头文件将生成在消息的头文件的同一目录中。对于Python和Lisp，会在`msg`目录旁边的`srv`目录中。

* rosmsg 和 rossrv -h 可以获得相关命令



#### ROS节点编写

* qt creator直接生成