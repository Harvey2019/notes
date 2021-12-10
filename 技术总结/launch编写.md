## launch文件的编写

各种标签的详细介绍http://wiki.ros.org/roslaunch/XML

### param 设置参数

```
<param name="foo" value="ruihua"/>
```

### arg 设置变量

```
<arg name="foo" default="1" />
<arg name="foo" value="bar" />
```

变量可以被读取：

```
  <arg name="num" value="1" /> 
  <!-- read value of arg -->
  <param name="quantity" value="$(arg num)"/>
```

也可以跨文件传递		

`my_file.launch`: 

```xml
<include file="included.launch">
  <!-- all vars that included.launch requires must be set -->
  <arg name="hoge" value="fuga" />
</include>
```

`included.launch`: 

```xml
<launch>
  <!-- declare arg to be passed in -->
  <arg name="hoge" /> 

  <!-- read value of arg -->
  <param name="param" value="$(arg hoge)"/>
</launch>
```

在命令行设置变量：

```
$ roslaunch %YOUR_ROS_PKG% my_file.launch hoge:=my_value
```



------------------------------------------------------------------------------------------------------------------------



* 参数和变量会被最后一次赋值覆盖（但不完全一定）

* $(env ENVIRONMENT_VARIABLE) 置换为已存在的环境变量

  ```xml
  <launch>
    <machine name="c1" address="pre1" ros-root="$(env ROS_ROOT)" ros-package-path="$(env ROS_PACKAGE_PATH)" default="true" />
  </launch>
  ```

* $(optenv ENVIRONMENT_VARIABLE)  or $(optenv ENVIRONMENT_VARIABLE default_value) 置换为环境变量(带默认值)

  ```xml
    <param name="foo" value="$(optenv NUM_CPUS 1)" />
    <param name="foo" value="$(optenv CONFIG_PATH /home/marvin/ros_workspace)" />
    <param name="foo" value="$(optenv VARIABLE ros rocks)" />
  ```

​		自定义环境变量：**export ROBOT=my_robot**



* $(find pkg) 置换为文件路径

  ```
  (find rospy)/manifest.xml
  ```

* $(eval expression) 计算表达式

  ```xml
  <param name="circumference" value="$(eval 2.* 3.1415 * arg('radius'))"/>
  为了方便：
  "$(eval arg('foo'))"=="$(eval foo)"
  ```

* $(dirname) 返回launch文件的绝对路径

  ```
  <include file="$(dirname)/other.launch" />
  ```

* if 和 unless 条件执行

  ```xml
  <group if="$(arg foo)">
    <!-- stuff that will only be evaluated if foo is true -->
  </group>
  
  <param name="foo" value="bar" unless="$(arg foo)" /> 
  ```

* include 引入其它文件（launch/yaml等）

  ```xml
  <include file="$(find 2dnav_pr2)/config/base_odom_teleop.xml" />
  <include file="$(find 2dnav_pr2)/move_base/2dnav_pr2.launch" ns="foo"  />
  ```

* 运行节点 node

  ```xml
  <node name="listener1" pkg="rospy_tutorials" type="listener.py"  args="$(arg a) $(arg b) respawn="true" />
  
  ----------
  
  <node pkg="move_base" type="move_base" name="move_base"  args="10" machine="c2">
    <remap from="odom" to="pr2_base_odometry/odom" />
    <param name="controller_frequency" value="10.0" />
    <rosparam file="$(find 2dnav_pr2)/move_base/navfn_params.yaml" command="load" />
    <rosparam file="$(find 2dnav_pr2)/move_base/base_local_planner_params.yaml" command="load" />
  </node>
  ```

* rosparam 支持通过yaml文件载入param

  ```xml
  <rosparam command="load" file="$(find rosparam)/example.yaml" ns="myns" />
  如果yaml前几行定义了ns，那么ns项可以不要
  <rosparam command="delete" param="my/param" />
  ```

* remap 重映射话题

  ```xml
  <remap from="/different_topic" to="/needed_topic"/>
  注意：话题名称要跟程序中的一样，看清楚带不带“/”
  ```

* group 分组

  ```xml
    <group ns="wg2">
      <!-- remap applies to all future statements in this scope. -->
      <remap from="chatter" to="hello"/>
      <node pkg="rospy_tutorials" type="listener" name="listener" />
      <node pkg="rospy_tutorials" type="talker" name="talker">
        <!-- set a private parameter for the node -->
        <param name="talker_1_param" value="a value" />
        <!-- nodes can have their own remap args -->
        <remap from="chatter" to="hello-1"/>
        <!-- you can set environment variables for a node -->
        <env name="ENV_EXAMPLE" value="some value" />
      </node>
    </group>
  ```
