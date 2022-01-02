## 程序运行框架

### Simulation框架

```c++
main_helper(argc, argv, new MIT_Controller()); //判断机器人型号、仿真还是硬件
SimulationBridge simulationBridge(gMasterConfig._robot, ctrl);//绑定仿真的机器人数据和控制器
simulationBridge.run();//开始仿真程序，判断仿真模式：修改控制参数、运行控制程序、什么都不做
simulationBridge::runRobotControl();//给RobotRunner初始化，运行程序
RobotRunner::run(); //运行控制程序
```

### MIT_Controler

```c++
_gaitScheduler->step(); //获得接触状态
_desiredStateCommand->convertToStateCommands();//将遥控信号转换成期望状态轨迹x_des
_controlFSM->runFSM();//运行状态机
```





**RobotRunner::run（）：**

更新腿部关节的位置、速度，末端位置、速度（body-fixed）

获取传感器数据，估计目前状态

**运行控制器_robot_ctrl->runController()  得到关节命令q_cmd**

向关节发送命令



**ControlFSM<T>::runFSM()：**（在此程序之前，已获得所有状态）

检查姿势，姿势不安全时operatingMode改成ESTOP

根据遥控器调节control_mode（control_mode决定下次状态是什么）

判断operatingMode是否ESTOP：

* 若是：状态切换为passive
* 若NORMAL：判断下次状态是否改变
  - 若改变：operatingMode改成TRANSITIONING
  - 若不改变：执行currentState->run()，**得到扭矩命令**，检查位置安全

* 若TRANSITIONING：检查位置安全，退出当前状态，进入下一个状态，operatingMode改成NORMAL

