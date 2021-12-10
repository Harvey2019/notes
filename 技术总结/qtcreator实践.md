### 程序调试

#### Attach to a unstarted process

1. Next in Qt Creator browse to the file you wish to debug and insert break points.
2. Menu Bar > Debug > Start Debugging > Attach to Unstarted Application...
3. Browse to the executable then select Start Watching.
4. Now run your project. Ctrl + R
5. Now depending on where the breakpoints were placed in qt, it should be stopped at a break point when it reaches one.

#### Attach to a running process

1. Next in Qt Creator browse to the file you wish to debug and insert break points.
2. Now run your project. Ctrl + R
3. Menu Bar > Debug > Start Debugging > Attach to Running Application...
4. Now select the Process ID and then click the button Attach to Process.
5. Now depending on where the breakpoints were placed in qt, it should be stopped at a break point when it reaches one.



#### 选择运行或调试的程序

* 项目-Run-添加运行配置：Custom Exe 和 ROS Run Config

* Custom Exe 需要选择程序的可执行文件（/devel/lib/{name}） 一次只能调试一个程序
* ROS Run Config 直接选择文件 调试的时候必须选择一个调试节点（只能调试一个），另外可以选择同时运行其他程序（node launch等）



#### C++ console添加头文件路径

打开.pro文件，在后面输入INCLUDEPATH += /usr/include/eigen3（头文件路径）