# Motion Control Development Guide

This document provides development instructions for motion control of the Elf3 robot.

---

## Using the Robot
```
#There is a bxi_ws folder in the root directory of the robot controller,which contains the robot motion control program.

#Original project repository. Please read the README.md carefully:
https://github.com/bxirobotics/bxi_rl_controller_ros2_example
```
## Launching the Robot Program
- Start Simulation Program
```
source /opt/bxi/bxi_ros2_pkg/setup.bash
cd bxi_ws/bxi_rl_controller_ros2_example
source install/setup.bash
ros2 launch bxi_example_py_elf3 example_launch_demo.py
```
- Start Real Robot Program (⚠️ Pay Attention to Safety)
```
# Programs interacting with robot motors require root privileges

# Switch to root
sudo su 

source /opt/bxi/bxi_ros2_pkg/setup.bash
cd /home/bxi/bxi_ws/bxi_rl_controller_ros2_example
source install/setup.bash
ros2 launch bxi_example_py_elf3 example_launch_demo_hw.py
```
![alt text](../../assets/elf3/motion1.png)<br>
After startup, the motors will be powered on. You will observe green lights on all motors.
The robot will enter a self-check process.
If the green lights turn off after about 6 seconds, it indicates that the self-check has passed.<br>
- Start Remote Controller Program
```
#Real Robot
sudo su 
source /opt/bxi/bxi_ros2_pkg/setup.bash
cd /home/bxi/bxi_ws/bxi_rl_controller_ros2_example
source install/setup.bash
ros2 launch remote_controller remote_conroller_launch.py 

#Simulation
source /opt/bxi/bxi_ros2_pkg/setup.bash
cd ~/bxi_ws/bxi_rl_controller_ros2_example
source install/setup.bash
ros2 launch remote_controller remote_conroller_launch.py 

```
# Common Issues
### 1.How to Stop the Background Remote Controller Service
If the green lights remain on after about 6 seconds, it means the self-check has passed.<br>
- Check Service Status
```
systemctl status ros_elf_launch.service
```
![alt text](../../assets/elf3/motionservice.png)<br>
You can stop it using the following methods:<br>
- Stop the Service Temporarily
```
systemctl stop ros_elf_launch.service
```
- Restart the Service
```
systemctl start ros_elf_launch.service
```
- Disable Permanently
```
systemctl stop ros_elf_launch.service
sudo systemctl disable ros_elf_launch.service
#This disables auto-start on boot
```
### 2.Viewing Logs When Real Robot Program Fails to Start

- The official program stores logs in the following directory:
```
/var/log/bxi_log
```
Logs are sorted by time.

