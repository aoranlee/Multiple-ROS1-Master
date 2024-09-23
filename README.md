# Multiple-ROS1-Master
Multiple ROS Master

**My configuration:**

Ubuntu 20.04

ROS Noetic

**Prerequisites:**

1. hostname
2. bashrc
3. /etc/hosts file
4. Multi-master setup
5. swarm-formation simulation

**Configuration:**

1. hostname

Modify on computers a, b, c respectively:

```
sudo hostnamectl set-hostname name1
sudo hostnamectl set-hostname name2
sudo hostnamectl set-hostname name3
```

```
hostname
--Check if modification is successful
```

2. bashrc 

Modify on computers a, b, c respectively:

```
export ROS_MASTER_URI=http://a_ip:11311
export ROS_HOSTNAME=name1

export ROS_MASTER_URI=http://b_ip:11311
export ROS_HOSTNAME=name2

export ROS_MASTER_URI=http://c_ip:11311
export ROS_HOSTNAME=name3
```

3. /etc/hosts file

```
sudo nano /etc/hosts
```

Add inside the file:

```
a_ip amov1
b_ip amov2
c_ip amov3
```

Test: ping other hosts

4. Multi-master setup

```
sudo apt-get install python3-catkin-pkg python3-rospkg python3-yaml ros-noetic-roslaunch ros-noetic-rostest ros-noetic-rospy ros-noetic-rosunit

mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
source devel/setup.bash
cd src
git clone https://github.com/fkie/multimaster_fkie.git
cd ..
catkin_make
source devel/setup.bash
```

Write a launch file: change.launch

```
<launch>
    <node pkg="fkie_master_discovery" type="master_discovery" name="master_discovery" output="screen"/>
  	<node pkg="fkie_master_sync" type="master_sync" name="master_sync" output="screen"/>
</launch>
```

Test:Establish multi-master communication on a, b, c

```
roslaunch demo change.launch
```

Note: You need to create the demo package by yourself

5. swarm-formation simulation

```
https://github.com/ZJU-FAST-Lab/Swarm-Formation
--Official link
```

6、Final test:

On computer a, open two terminals each, a inputs:

```
roslaunch demo change.launch
```

After observing that full synchronous communication is established, then:

Host a:

```
roslaunch ego_planner rviz.launch
roslaunch ego_planner normal_hexagon.launch
```

Hosts b and c:

```
roslaunch ego_planner normal_hexagon.launch
```

The final video result will appear!

Note:

When there is too much communication, it indicates that the actual host cannot meet the heavy communication requirements. A recommended strategy is to disconnect from the internet.


