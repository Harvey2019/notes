##ubuntu VSCode C++

1. 安装C/C++ 扩展

2. 配置库路径

   shift+ctrl+p 弹出命令窗口

   输入命令C/C++: Edit Configurations (UI)，设置includePath，以引用在文件夹以外的头文件 ，对应文件**c_cpp_properties.json** 

3. 编写C++文件

4. 编译

   Terminal > Configure Default Build Task >C/C++: g++ build active file

   创建**tasks.json**，该文件决定如何编译文件

   args参数决定编译的文件以及输出的文件

   shift+ctrl+B or Terminal > Run build task编译文件，生产可执行文件

5. 调试

   Run> Add Configuration >C++ (GDB/LLDB) >g++ build and debug active file 创建**launch.json**，该文件决定如何调试

   program参数决定调试的文件

   F5启动调试

**注意：**

1. 以上三个配置文件都在文件夹.vscode中
2.  在进行操作4和5时，最好打开要用的C++文件，软件会自动填入参数，执行相应文件

**具体参考**：[Get Started with C++ on Linux in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-linux) 







## 在VSCode中编译和调试ROS

1. 安装扩展：ROS，MSGshow

2. 创建并在VSCode打开工作空间

3. 配置**c_cpp_properties.json**

   * 在includepath中加上"/opt/ros/melodic/include" ，这样能够找到ROS头文件。
   * 如果还是找不到，编译catkin_make-DCMAKE_EXPORT_COMPILE_COMMANDS=1，并修改文件 "compileCommands":"${workspaceFolder}/build/compile_commands.json" 

4. 配置**tasks.json**(也可以不用，直接在终端catkin_make)

   选择编译类型catkin_make:bulid即可，**编译时记得修改cmakelist**

5. 配置**launch.json**

   * 单程序调试：添加（gdb）启动，修改program为执行文件路径，**在cmakelist文件中加入SET(CMAKE_BUILD_TYPE Debug)**，否则无法断点；也可以添加ROS:attach(提高效率，但尚未研究)。
   * 多程序调试：添加ROS:launch，修改target为launch文件路径即可，运行时页面要停留在launch文件上。

**参考**：[使用VScode搭建ROS开发环境_白鸟无言的博客-CSDN博客](https://blog.csdn.net/qq_42688495/article/details/107750466) 

[ROS——vscode配置_偷得浮生半日闲-CSDN博客_ros vscode](https://blog.csdn.net/Kalenee/article/details/103828448) （attach）

[使用VScode调试ROS_淡水苦舟-CSDN博客](https://blog.csdn.net/weixin_42268975/article/details/106021808) 



#### 2021.7.18

